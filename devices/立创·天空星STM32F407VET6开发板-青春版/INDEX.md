# 立创·天空星 STM32F407VET6 青春版资料索引

> 目录 id：[`stm32f407-skystar`](../catalog.yaml) · 结构化卡片：[device.yaml](device.yaml)
>
> 仅青春版 STM32F407VET6。不是其它 F4 / GD32 / AT32。

## 按内容看过之后的结论

| 文件 | 处理 |
|---|---|
| 原理图 PDF、引脚分配图 PDF、嘉立创 `.epro` | 本板设计依据，收入 `electrical/` 与 `references/eda/` |
| ST DS8626 Rev12、RM0090 Rev22、勘误 ES0182 | 日常查阅，`references/chip/` |
| 15 个基础 SPL 例程 + 空白模板 | 本板教程，`software/board-examples/` |
| 板背面图、宣传页 | 青春版识别，`images/` |
| 商家第 04 章旧手册 | 与官方新版重复且更旧 | **已删除** |
| Keil 安装包、CubeProgrammer、CH340/ST-Link 驱动、串口工具、DFP、标准库 ZIP | 通用工具，不是本板资料 | **已删除**（约 1.5 GB+） |
| 1.72 GB 视频 7z | 教程视频，不是本板规格 | **已删除** |
| 86 个外接模块例程（MQ、OLED、ESP-01S 烧录器等） | 别的器件的代码 | **已删除** |

## 优先查阅

- [硬件与青春版实装](docs/hardware-and-pinout.md)
- [P1/P2 逐针表](docs/pin-header-p1-p2-map.md)（两排间距是 **35.56 mm**，不是 33.02 mm）
- [上电与下载](docs/bring-up-and-programming.md)
- [板级例程](docs/examples-index.md)

## 本地资料

- [引脚分配图](electrical/pin-assignment.pdf)
- [原理图 2024-01-17](electrical/schematic-2024-01-17.pdf)
- [嘉立创 EDA 工程](references/eda/立创梁山派·天空星开发板_2024-01-17.epro)
- [数据手册 DS8626 Rev 12](references/chip/STM32F407VE-Datasheet-DS8626-Rev12.pdf)
- [RM0090 Rev 22](references/chip/RM0090-Rev22.pdf)
- [勘误 ES0182](references/chip/ES0182-STM32F405-407-Errata.pdf)
- [板级例程](software/board-examples/)
- [宣传页](images/promotion/)
- [背面](images/board-back.png)

## 使用前必看

- 青春版默认未焊 W25Q128、LSE、RTC 电池座、外部基准。
- USB-C 是 MCU USB FS（PA11/PA12），不是 USB 转串口；USART1 需外接 3.3 V USB-TTL。
- P1/P2 底座间距用 35.56 mm。33.02 mm 是内侧空档，用错板子报废。
