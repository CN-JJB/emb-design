# ICM-45686 六轴 IMU 模块资料索引

> 目录 id：[`icm45686`](../catalog.yaml) · 结构化卡片：[device.yaml](device.yaml)

## 按内容看过之后的结论

| 文件 | 实际是什么 | 归入 |
|---|---|---|
| 正面照片 | 丝印 `ICM45686`，芯片 14586 | `images/` |
| 背面照片 | V1.3；主排 `VCC GND AD0/MISO SDA/MOSI SCL/SCLK CS INT1`；侧 `SDX SCX INT2`；排针未焊 | `images/` |
| 尺寸图 | 15.000×18.000 mm，孔 Ø3.000、孔心距 13.500 mm | `mechanical/` |
| 原理图 | ME6211C33M5G-N LDO；H3 7P / H2 3P | `electrical/` |
| ICM-45686 DS rev1.0 | 芯片手册 | `references/chip/` |
| InvenSense 驱动与多平台例程 | 推荐软件 | `software/driver/`、`software/examples/` |

原理图标题曾残留 `lsm6dsv`，以丝印和 `WHO_AM_I=0xE9` 为准。

## 优先查阅

- [硬件与接线](docs/hardware.md)
- [寄存器速查](docs/register-quick-reference.md)
- [驱动说明](docs/software.md)

## 关键识别值

| 项目 | 结论 |
|---|---|
| I²C 地址 | AD0 接地 `0x68`，接 3.3 V `0x69` |
| WHO_AM_I | `0x72` → `0xE9` |
| 模块 VCC | 3.3–5 V（板载 LDO）；IO 按 3.3 V |
