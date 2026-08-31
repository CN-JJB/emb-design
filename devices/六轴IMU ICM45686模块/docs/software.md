# 驱动和例程说明

## 公共驱动

`driver/invensense/` 保留了资料包中唯一一份 InvenSense 驱动，共 8 个文件。原始七套 MCU 工程里的同名文件哈希完全一致，因此不再按工程重复保存。

移植时主要实现：

- `read_reg`：从连续寄存器读取数据。
- `write_reg`：写入连续寄存器。
- `sleep_us`：微秒延时。
- `serif_type`：选择 I²C 或四线 SPI。

## 精选例程

`examples/` 只保存传感器相关的移植代码和引脚配置，不再保留完整 IDE 工程、MCU SDK 或构建输出。

| 平台 | 保留内容 | 原资料引脚 |
|---|---|---|
| Arduino | 单文件快速验证示例 | 由 Wire 默认引脚决定，地址 `0x69` |
| STM32F103 I²C | 传输/初始化示例与软件 I²C 端口 | PB6=SCL，PB7=SDA |
| STM32F103 SPI | 传输/初始化示例与 SPI 端口 | PA2=CS，PB13=SCLK，PB14=MISO，PB15=MOSI |
| STM32F407 HAL | I²C/SPI 合并示例与端口文件 | I²C：PC1/PC2；SPI：PA4/PA5/PA6/PA7 |
| MSPM0 CCS I²C | 驱动适配文件与 SysConfig | PA0=SDA，PA1=SCL |
| MSPM0 CCS SPI | 驱动适配文件与 SysConfig | PB5=CS，PB7=MISO，PB8=MOSI，PB9=SCLK |
| MSPM0 Keil I²C | 驱动适配、SysConfig 和生成的引脚配置 | PA0=SDA，PA1=SCL |
| MSPM0 Keil SPI | 驱动适配、SysConfig 和生成的引脚配置 | PA2=CS，PA4=MISO，PB17=MOSI，PB18=SCLK |

这些是移植参考片段，不是可直接打开编译的完整工程。新项目应使用当前 MCU SDK/启动文件重新建工程，再加入公共驱动和对应端口代码。

## 初始化流程

资料中的 STM32 示例执行以下步骤：

1. 绑定读、写和延时回调。
2. 选择 I²C 或四线 SPI。
3. 等待供电稳定。
4. 读取并校验 `WHO_AM_I = 0xE9`。
5. 软复位。
6. 配置 INT1/DRDY。
7. 设置量程、ODR、低通带宽和工作模式。
8. 读取加速度、角速度和温度。

## 原始例程的已知问题

- 部分注释仍写有 `ICM42688`、`ICM45688` 等旧型号名称。
- 多处中文注释编码损坏。
- Arduino 示例的函数名误写为 `ICM45688_getacc/getgyro`。
- Arduino 示例直接写入部分内部滤波地址，而 ICM-45686 的 IPREG 有专用间接访问流程；这部分不能作为权威配置方法。
- 原始 STM32F103 的两个 `main.c` 完全相同且同时初始化 I²C/SPI，实际总线由 `basic_read_reg.c` 中的宏选择。
- 原始 MSPM0 README 是 TI Empty Project 模板，没有记录 IMU 引脚。

## 使用建议

- 优先从公共 InvenSense 驱动移植，不从 Arduino 示例重新写寄存器层。
- 先只验证 `WHO_AM_I` 和三轴原始数据，再加入中断、FIFO、APEX 或辅助总线。
- 驱动、单位换算、校准和姿态解算分层，避免把旧示例中的全局状态直接带入新工程。
- 量程、ODR、滤波及单位换算集中配置，不在应用代码中散落魔法数。
