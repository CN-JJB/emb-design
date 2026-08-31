# ESP32-C3 Super Mini 资料索引

> 目录 id：[`esp32-c3-supermini`](../catalog.yaml) · 结构化卡片：[device.yaml](device.yaml)
>
> 仅此 Super Mini 小板。芯片后缀 FN4/FH4/FH4X 以实物丝印和 `esptool flash-id` 为准。

## 按内容看过之后的结论

| 文件 | 实际是什么 | 归入 |
|---|---|---|
| 电路图 PNG | 板级原理图：原生 USB、ME6211 类 LDO、两排 1×8、GPIO8 LED、GPIO9 BOOT | `electrical/` |
| 尺寸图 | 顶视 18.00×22.50 mm，排针跨距 15.24 mm；USB 朝上时 `5V/G/3.3` 在右列 | `mechanical/` |
| PcbDoc | 外形/排针机械文件，不是可生产工程 | `mechanical/` |
| STEP | 干涉检查用 | `model/` |
| 官方 DS v2.4 / TRM v1.4 | 日常查阅 | `references/chip/` |
| ME6211 手册 | 板载 LDO 系列，完整料号未写死 | `references/ldo/` |
| 商家旧 DS/TRM、未锁版本 HTML | 过时或无法作为版本依据 | **已删除** |

## 优先查阅

- [硬件与引脚](docs/hardware-and-pinout.md)
- [上电与烧录](docs/bring-up-and-flashing.md)
- [资料审计](docs/source-audit.md)
- [官方在线入口](docs/official-sources.md)

## 本地资料

- [电路图](electrical/schematic.png)
- [尺寸图](mechanical/board-18x22.5mm.png)
- [PcbDoc 外形](<mechanical/Super Mini-ESP32C3-外形.PcbDoc>)
- [STEP](<model/Super Mini-ESP32C3.step>)
- [数据手册 v2.4](references/chip/ESP32-C3-Series-Datasheet-v2.4.pdf)
- [技术参考手册 v1.4](references/chip/ESP32-C3-Technical-Reference-Manual-v1.4.pdf)
- [ME6211](references/ldo/ME6211A33M3G-N.PDF)

## 使用前必看

- GPIO8 用户 LED（低电平点亮），GPIO9 BOOT；GPIO2/8/9 是启动脚。
- USB 走 GPIO18/19 原生 USB，没有 CH340。GPIO20/21 是 UART0 RX/TX。
- 封装朝向按尺寸图顶视：USB 朝上时电源列在右。网上把电源画在左边的图是镜像/底视。
