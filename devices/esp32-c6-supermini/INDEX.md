# ESP32-C6 Super Mini 资料索引

> 目录 id：[`esp32-c6-supermini`](../catalog.yaml) · 结构化卡片：[device.yaml](device.yaml)
>
> 仅 **ESP32-C6FH4** Super Mini。HXB 模组板资料已移除。

## 按内容看过之后的结论

| 文件 | 实际是什么 | 归入 |
|---|---|---|
| 原理图 PNG | C6FH4、原生 USB、TP4054 充电、LDO、GPIO8 RGB、GPIO15 蓝灯 | `electrical/` |
| 正面尺寸图 | 26.04×18.00 mm，主焊盘跨距 22.86 mm | `mechanical/` |
| 背面尺寸图 | B+/B−、GPIO 编号 | `mechanical/` |
| 彩色引脚总图 | USB 在上、电源列在右，与顶视一致；GPIO Matrix 画得过宽，只能浏览 | `images/` |
| 官方 DS v1.5 / TRM v1.2 | 日常查阅 | `references/chip/` |
| 商家旧 DS/TRM | 过时 | **已删除** |
| 测试固件 .bin | 验板用，无源码 | `software/` |
| .elf / .map | 体积大、不是源码 | **已删除** |

排针方向以实物为准：与 C3 从 USB 端对齐后第 1 针都是 `5V`。

## 优先查阅

- [硬件与引脚](docs/hardware-and-pinout.md)
- [固件与烧录](docs/firmware-and-flashing.md)

## 本地资料

- [原理图](electrical/schematic.png)
- [正面尺寸](mechanical/board-front-26x18mm.png)
- [背面尺寸](mechanical/board-back-26x18mm.png)
- [引脚总图](images/pinout-overview.jpg)
- [数据手册 v1.5](references/chip/ESP32-C6-Series-Datasheet-v1.5.pdf)
- [技术参考手册 v1.2](references/chip/ESP32-C6-Technical-Reference-Manual-v1.2.pdf)
- [ME6211](references/ldo/ME6211A33M3G-N.PDF)
- [测试固件](software/)
