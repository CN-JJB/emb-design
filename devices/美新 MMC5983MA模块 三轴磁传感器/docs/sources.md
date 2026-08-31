# 官方及开源来源清单

在线来源核验日期：**2026-08-09**。

## 1. MEMSIC 官方来源

- [MMC5983MA 英文产品页](https://www.memsic.com/magnetometer-5)：器件身份、概要规格和官方数据手册入口。
- [MMC5983MA 中文产品页](https://www.memsic.com/cn/magnetometer-5)：美新中文产品介绍。
- [MMC5983MA Datasheet Rev A 官方 PDF](https://www.memsic.com/Public/Uploads/uploadfile/files/20220119/MMC5983MADatasheetRevA.pdf)：本地归档来源。
- [MEMSIC Datasheets](https://www.memsic.com/datasheets)：官方数据手册总入口。

本地官方归档：

| 文件 | SHA-256 |
|---|---|
| [MMC5983MA-Datasheet-RevA.pdf](../references/chip/MMC5983MA-Datasheet-RevA.pdf) | `D82083B922C2421757035CEB064E6F316A8F8C20B4593277C13EF750115731F7` |

截至核验日期，MEMSIC 产品页公开提供的是数据手册，没有找到可直接下载的官方参考驱动或该淘宝模块原理图。

## 2. 推荐开源实现

### SparkFun MMC5983MA Arduino Library

- [GitHub 仓库](https://github.com/sparkfun/SparkFun_MMC5983MA_Magnetometer_Arduino_Library)
- [固定提交 6e5c914](https://github.com/sparkfun/SparkFun_MMC5983MA_Magnetometer_Arduino_Library/tree/6e5c914bf3c38c336b68456450b3e065d22e5ac4)
- [SparkFun Hookup Guide](https://learn.sparkfun.com/tutorials/qwiic-micro-magnetometer---mmc5983ma-hookup-guide/all)
- [Arduino Library 目录页](https://docs.arduino.cc/libraries/sparkfun-mmc5983ma-magnetometer-arduino-library/)

本地固定版本：

| 文件 | 版本/提交 | 许可证 | SHA-256 |
|---|---|---|---|
| [SparkFun-MMC5983MA-Arduino-v1.1.5-6e5c914.zip](../software/SparkFun-MMC5983MA-Arduino-v1.1.5-6e5c914.zip) | v1.1.5 / `6e5c914bf3c38c336b68456450b3e065d22e5ac4` | 代码 MIT，硬件 CC BY-SA 4.0 | `1BEC9C96FC7831ABA4C3DC98FBD28BA505D5656D567B2894044E299854567EA1` |

该库是目前本目录的主要开源实现依据。它面向 SparkFun 自己的 Qwiic 板，因此只能复用芯片寄存器、数据拼接、SET/RESET 和驱动结构，不能把其板级电源、SPI、INT 或轴向直接套到当前淘宝模块。

## 3. 次要在线对照

- [VincentChantreau/mmc5983ma](https://github.com/VincentChantreau/mmc5983ma)：通用 C、GPL-3.0、README 标注 WIP；不作为生产基线。
- [Ahreo/MMC5983MA-Driver](https://github.com/Ahreo/MMC5983MA-Driver)：STM32H723 C++ 示例，许可证不明确；只用于阅读思路。
- [kriswiner/MMC5983MA](https://github.com/kriswiner/MMC5983MA)：Arduino 多传感器 AHRS 示例，最后主要代码提交较早。

## 4. 通用磁力计校准参考

- Analog Devices [Hard & Soft Iron Correction for Magnetometer Measurements](https://ez.analog.com/mems/w/documents/4493/hard-soft-iron-correction-for-magnetometer-measurements)：hard-iron、soft-iron 和校正矩阵概念。
- Analog Devices [Hard & Soft Iron Correction for Magnetometers II](https://ez.analog.com/mems/w/documents/4433/hard-soft-iron-correction-for-magnetometers-ii)：进一步说明三维校正。
- VectorNav [Magnetometer Hard & Soft Iron Calibration](https://www.vectornav.com/resources/inertial-navigation-primer/specifications--and--error-budgets/specs-hsicalibration)：误差来源和校准概念。

这些资料不定义 MMC5983MA 的寄存器或当前模块电路，只用于通用校准理论。

## 5. 来源使用优先级

1. **芯片电气极限、寄存器和时序**：MEMSIC 官方数据手册。
2. **当前模块外形和四针名称**：保留的淘宝原始图片。
3. **可移植驱动结构和示例**：固定提交的 SparkFun 源码。
4. **整机磁校准**：Analog Devices/VectorNav 通用方法。
5. **其他社区驱动**：仅交叉核对，不覆盖官方数据手册。

若以后获得卖家原理图，其板级供电、上拉和轴向结论应优先于对图片的推断，但仍不能覆盖 MMC5983MA 芯片本体的绝对最大额定值。
