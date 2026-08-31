# 寄存器、18 位数据拼接与采样流程

## 1. 接口基线

- I²C 7 位地址：`0x30`。
- I²C 最高时钟：400 kHz。
- Product ID：读取 `0x2F`，期望 `0x30`。
- 上电或软件复位后等待至少 10 ms。
- 这块四针模块没有 INT 和 SPI 引脚，以下流程以 I²C 轮询为主。

完整定义以 [MEMSIC Datasheet Rev A](../references/chip/MMC5983MA-Datasheet-RevA.pdf) 为准。

## 2. 寄存器速查

| 地址 | 名称 | 用途 |
|---:|---|---|
| `0x00` | Xout0 | X[17:10] |
| `0x01` | Xout1 | X[9:2] |
| `0x02` | Yout0 | Y[17:10] |
| `0x03` | Yout1 | Y[9:2] |
| `0x04` | Zout0 | Z[17:10] |
| `0x05` | Zout1 | Z[9:2] |
| `0x06` | XYZout2 | X[1:0]、Y[1:0]、Z[1:0] |
| `0x07` | Tout | 8 位温度原始值 |
| `0x08` | Status | 数据就绪和 OTP 状态 |
| `0x09` | Internal Control 0 | 单次测量、温度、SET/RESET、自动 SR |
| `0x0A` | Internal Control 1 | 带宽、通道禁止、软件复位 |
| `0x0B` | Internal Control 2 | 连续频率、连续模式、周期 SET |
| `0x0C` | Internal Control 3 | 3-wire SPI、自检磁场 |
| `0x2F` | Product ID 1 | 固定值 `0x30` |

## 3. 关键状态与控制位

### Status `0x08`

| 位 | 名称 | 含义 |
|---:|---|---|
| 4 | OTP_Rd_Done | OTP 读取完成 |
| 1 | Meas_T_Done | 温度转换完成 |
| 0 | Meas_M_Done | 磁场测量完成 |

对 bit 0/1 写 1 可清除对应中断状态。启动新测量时相应 Done 位会回到 0。

### Internal Control 0 `0x09`

| 位 | 名称 | 用途 |
|---:|---|---|
| 6 | OTP_Read | 重新加载 OTP |
| 5 | Auto_SR_en | 自动 SET/RESET |
| 4 | Reset | 执行 RESET，自动清零 |
| 3 | Set | 执行 SET，自动清零 |
| 2 | INT_meas_done_en | 测量完成中断；本模块未引出 INT |
| 1 | TM_T | 启动温度测量 |
| 0 | TM_M | 启动三轴磁场测量 |

### Internal Control 1 `0x0A`

| 位 | 名称 | 用途 |
|---:|---|---|
| 7 | SW_RST | 软件复位，清寄存器并重读 OTP |
| 4:3 | YZ-inhibit | 禁止 Y/Z 通道 |
| 2 | X-inhibit | 禁止 X 通道 |
| 1:0 | BW[1:0] | 测量时间/数字滤波带宽 |

| BW | 测量时间 | 带宽 | 典型 RMS 噪声 |
|---|---:|---:|---:|
| `00` | 8 ms | 100 Hz | 0.4 mG |
| `01` | 4 ms | 200 Hz | 0.6 mG |
| `10` | 2 ms | 400 Hz | 0.8 mG |
| `11` | 0.5 ms | 800 Hz | 1.2 mG |

### Internal Control 2 `0x0B`

| 位 | 名称 | 用途 |
|---:|---|---|
| 7 | En_prd_set | 启用周期 SET |
| 6:4 | Prd_set[2:0] | 每 1/25/75/100/250/500/1000/2000 次测量 SET |
| 3 | Cmm_en | 连续测量使能 |
| 2:0 | CM_Freq[2:0] | 连续输出频率 |

连续频率编码：`001=1 Hz`、`010=10 Hz`、`011=20 Hz`、`100=50 Hz`、`101=100 Hz`、`110=200 Hz`、`111=1000 Hz`。200 Hz 需要 BW=`01`，1000 Hz 需要 BW=`11`；`000` 表示关闭。

### Internal Control 3 `0x0C`

| 位 | 名称 | 用途 |
|---:|---|---|
| 6 | SPI_3w | 3-wire SPI；当前模块不可用 |
| 2 | St_enm | 施加负向内部自检磁场 |
| 1 | St_enp | 施加正向内部自检磁场 |

## 4. 控制寄存器不能盲目读改写

数据手册把 `0x09–0x0C` 标为写寄存器。SparkFun 库也明确记录：对这些寄存器做普通 read-modify-write 可能读回意外位值。因此驱动应在 RAM 中维护四个 shadow byte：

1. 上电或软件复位后把 shadow 清零。
2. 修改位时只改 shadow。
3. 把完整 shadow byte 写回对应控制寄存器。
4. 对自清零命令位（TM_M、TM_T、SET、RESET、SW_RST）发出后，在 shadow 中主动清掉该位。

这是移植到 STM32、ESP32 或裸机驱动时最容易遗漏的细节之一。

## 5. 单次磁场测量

推荐流程：

1. 向 `0x09` 写入含 `TM_M=1` 的 Control 0 shadow。
2. 轮询 `0x08` bit 0，设置超时；BW=`00` 时可给 15–20 ms 余量。
3. 从 `0x00` 连续读取 7 字节。
4. 按下面方式拼出三轴 18 位无符号值。

```c
uint32_t x = ((uint32_t)b[0] << 10)
           | ((uint32_t)b[1] << 2)
           | ((b[6] >> 6) & 0x03);

uint32_t y = ((uint32_t)b[2] << 10)
           | ((uint32_t)b[3] << 2)
           | ((b[6] >> 4) & 0x03);

uint32_t z = ((uint32_t)b[4] << 10)
           | ((uint32_t)b[5] << 2)
           | ((b[6] >> 2) & 0x03);
```

不要把输出直接当作有符号补码。它是无符号值，零场中心典型为 `2^17 = 131072`。

## 6. 原始值换算

数据手册给出的 18 位灵敏度是 16384 counts/G：

```text
field_G  = (raw - 131072) / 16384
field_uT = field_G × 100
```

等价写法：

```text
field_G = ((raw - 131072) / 131072) × 8
```

数据手册首页把 18 位分辨率概括为 0.0625 mG/LSB；按表中 16384 counts/G 精确计算约为 0.0610 mG/LSB。工程换算应优先使用明确给出的 `16384 counts/G`，再通过实物校准修正灵敏度误差。

若只使用 16 位高位数据：

```text
raw16 = (MSB << 8) | LSB
field_G = (raw16 - 32768) / 4096
```

所有减法都应先转换为有符号 32 位或浮点类型，避免无符号下溢。

## 7. 温度读取

1. 在 Control 0 shadow 中置 `TM_T`。
2. 轮询 Status bit 1。
3. 读取 `Tout(0x07)`。

数据手册给出近似关系：

```text
temperature_C ≈ -75 + 0.8 × raw_T
```

SparkFun 库使用 `-75 + raw_T × 200 / 255` 将 0–255 映射到 -75–125 ℃。该温度主要服务于传感器补偿，是芯片结温的粗略指示，不应直接当作高精度环境温度。

## 8. STM32 HAL 最小身份检查

```c
#define MMC5983_ADDR_7BIT  0x30u
#define MMC5983_ADDR_HAL   (MMC5983_ADDR_7BIT << 1)

uint8_t id = 0;
HAL_StatusTypeDef ok = HAL_I2C_Mem_Read(
    &hi2c1,
    MMC5983_ADDR_HAL,
    0x2F,
    I2C_MEMADD_SIZE_8BIT,
    &id,
    1,
    100);

if ((ok != HAL_OK) || (id != 0x30)) {
    /* 接线、供电、地址或器件身份错误 */
}
```

单次采样时可用 `HAL_I2C_Mem_Write` 向 `0x09` 写 Control 0 shadow，再轮询 `0x08`，最后用一次 `HAL_I2C_Mem_Read` 从 `0x00` 读 7 字节。

## 9. 连续模式建议

对当前无 INT 引脚模块，优先从低速配置开始：

1. BW=`00`（低噪声）。
2. CM_Freq=`010`（10 Hz）。
3. 置 `Auto_SR_en`。
4. 设置所需的周期 SET 间隔，再置 `En_prd_set`。
5. 置 `Cmm_en`。
6. 定时轮询 Status bit 0并读取 7 字节；任何等待都必须有超时。

达到 1000 Hz 需要更高带宽，噪声也会增加，而且 400 kHz I²C、主控调度、校准滤波和其他总线设备都会影响实际可持续速率。先确认应用确实需要高频，不要把数据手册最大值作为默认配置。

## 10. SET/RESET 桥路偏置消除

高精度测量可执行：

1. SET。
2. 测量得到 `S = +H + O`。
3. RESET。
4. 测量得到 `R = -H + O`。
5. `H = (S - R) / 2`。
6. `O = (S + R) / 2`。

对 18 位无符号数据，SparkFun 示例把 `(S + R) / 2` 保存为各轴 raw offset，后续使用 `raw - offset`。SET/RESET 脉冲依赖 CAP 引脚附近的低 ESR 10 µF 电容；芯片官方参考电路明确要求该电容，当前模块照片无法核实其实际容值。

SET/RESET 去除的是 AMR 桥路内部偏置，不会自动消除整机磁铁、螺丝、电机和外壳造成的 hard-iron/soft-iron 误差。后者见[校准与安装说明](calibration-and-placement.md)。
