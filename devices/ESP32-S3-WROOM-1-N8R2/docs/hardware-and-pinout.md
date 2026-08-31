# 硬件、针序与封装

依据：[模组规格书 v1.8](../references/chip/ESP32-S3-WROOM-1-WROOM-1U-Datasheet-v1.8.pdf)、[芯片规格书 v2.2](../references/chip/ESP32-S3-Series-Datasheet-v2.2.pdf)。图是从这两份官方 PDF 按图号裁出来的，不是商家海报。

## 1. 本目录对象

**ESP32-S3-WROOM-1-N8R2** 是乐鑫 SMT 模组，不是开发板。

| 项目 | N8R2 |
|---|---|
| 天线 | 板载 PCB |
| Flash | 8 MB Quad SPI（模组上独立颗粒） |
| PSRAM | 2 MB Quad SPI（芯片封装内） |
| 芯片变体 | 规格书写 ESP32-S3R2 |
| 环境温度 | −40～85 ℃ |
| 外形 | 18.0 × 25.5 × 3.1 mm |

同系列不要混：

| 不要当成 N8R2 | 原因 |
|---|---|
| WROOM-1U | U.FL，长度 19.2 mm，无天线 keepout |
| N8R8 / N16R8 / N16R16VA | Octal PSRAM，**GPIO35/36/37 被占用**；N16R16VA 的 VDD_SPI 为 1.8 V |
| DevKitC-1 | 开发板排针、USB-UART、按键都不是本模组的一部分 |
| WROOM-2 | 另一款模组 |

芯片手册 v2.2 把 **ESP32-S3R2 标为 EOL**，升级为 **ESP32-S3RH2**（仍是 2 MB Quad PSRAM，温度上限到 105 ℃）。模组规格书 v1.8 的 Table 1-1 仍列出 N8R2。新到货模组的封装芯片可能是 S3R2 或 S3RH2，以丝印和 `esptool` 为准；对 GPIO35/36/37 可用这一点，两种都是 Quad PSRAM，行为相同。

## 2. 顶视针序

天线 keepout 在图上方，脚 1 / 脚 40 靠近天线。编号顺时针。完整图：[pin-layout-top.png](../images/pin-layout-top.png)。

左列（1→14，靠近天线往下）：

| 脚 | 名称 | 默认/关键功能 |
|---:|---|---|
| 1 | GND | 地 |
| 2 | 3V3 | 3.3 V 供电 |
| 3 | EN | 芯片使能（CHIP_PU），高电平工作，**禁止悬空** |
| 4 | IO4 | GPIO4，TOUCH4，ADC1_CH3 |
| 5 | IO5 | GPIO5，TOUCH5，ADC1_CH4 |
| 6 | IO6 | GPIO6，TOUCH6，ADC1_CH5 |
| 7 | IO7 | GPIO7，TOUCH7，ADC1_CH6 |
| 8 | IO15 | GPIO15，ADC2_CH4，XTAL_32K_P |
| 9 | IO16 | GPIO16，ADC2_CH5，XTAL_32K_N |
| 10 | IO17 | GPIO17，U1TXD，ADC2_CH6 |
| 11 | IO18 | GPIO18，U1RXD，ADC2_CH7 |
| 12 | IO8 | GPIO8，TOUCH8，ADC1_CH7 |
| 13 | IO19 | GPIO19，**USB_D−** |
| 14 | IO20 | GPIO20，**USB_D+** |

底边（15→26）：

| 脚 | 名称 | 默认/关键功能 |
|---:|---|---|
| 15 | IO3 | GPIO3，TOUCH3，ADC1_CH2；JTAG 选择 strapping |
| 16 | IO46 | GPIO46；boot / ROM 打印 strapping，默认内部下拉 |
| 17 | IO9 | GPIO9，TOUCH9，ADC1_CH8 |
| 18 | IO10 | GPIO10，TOUCH10，ADC1_CH9 |
| 19 | IO11 | GPIO11，ADC2_CH0 |
| 20 | IO12 | GPIO12，ADC2_CH1 |
| 21 | IO13 | GPIO13，ADC2_CH2 |
| 22 | IO14 | GPIO14，ADC2_CH3 |
| 23 | IO21 | GPIO21 |
| 24 | IO47 | GPIO47 |
| 25 | IO48 | GPIO48 |
| 26 | IO45 | GPIO45；VDD_SPI strapping，默认内部下拉 |

右列（40→27，靠近天线往下）：

| 脚 | 名称 | 默认/关键功能 |
|---:|---|---|
| 40 | GND | 地 |
| 39 | IO1 | GPIO1，TOUCH1，ADC1_CH0 |
| 38 | IO2 | GPIO2，TOUCH2，ADC1_CH1 |
| 37 | TXD0 | **U0TXD / GPIO43** |
| 36 | RXD0 | **U0RXD / GPIO44** |
| 35 | IO42 | GPIO42，MTMS |
| 34 | IO41 | GPIO41，MTDI |
| 33 | IO40 | GPIO40，MTDO |
| 32 | IO39 | GPIO39，MTCK |
| 31 | IO38 | GPIO38 |
| 30 | IO37 | GPIO37（N8R2 **可用**） |
| 29 | IO36 | GPIO36（N8R2 **可用**） |
| 28 | IO35 | GPIO35（N8R2 **可用**） |
| 27 | IO0 | GPIO0；boot strapping，默认内部上拉 |
| 41 | EPAD | 地。手册：不是必须焊，焊上有利于散热；锡膏不要过多 |

数字外设可通过 GPIO Matrix 重映射。上表黑体是下载、USB、UART0 和 strapping，不要随便改用途。

## 3. 供电、复位与 USB

推荐工作电压 3.0～3.6 V，典型 3.3 V。绝对最大 3.6 V。手册要求外部电源至少能提供 **0.5 A**。Wi-Fi 发射峰值约 355 mA（802.11b @20.5 dBm，100% 占空比）。

典型底板（[Figure 9-1](../electrical/typical-application.png)）：

- VDD33：22 µF + 0.1 µF；
- EN：不要悬空。正文建议约 **10 kΩ 上拉 + 1 µF 到地** 做上电延时；图里复位键旁画的是 0.1 µF，延时以正文参数为准，按实际上电时序微调；
- USB：IO19/IO20，图中串联 0 Ω，硬件设计指南常用约 22 Ω；
- UART0：模组 TXD0/RXD0 接到主机 RX/TX。

N8R2 带封装内 PSRAM，VDD_SPI 由 eFuse 固定为 3.3 V，**上电时 GPIO45 不再决定 VDD_SPI**。GPIO45 仍是 strapping 脚，复位采样后才当普通 GPIO。

IO 高电平输入上限为该电源域 VDD+0.3 V，不能直接接 5 V UART/USB 口线。

## 4. 机械与天线

[外形图](../mechanical/module-18x25.5mm.png)、[焊盘](../mechanical/land-pattern.png)：

| 项目 | 数值 |
|---|---|
| 长 × 宽 × 高 | 25.5±0.2 × 18.0±0.2 × 3.1±0.15 mm |
| 脚距 | 1.27 mm |
| 天线区（顶视） | 约 6 mm |
| 推荐封装 | 见 land-pattern；热焊盘过孔按图中红圈 |

天线在脚 1/40 一侧。底板在 Antenna Area / Keepout Zone 下不要铺地、不要走过孔、不要放金属壳或连接器。具体净空见 [ESP32-S3 Hardware Design Guidelines](https://docs.espressif.com/projects/esp-hardware-design-guidelines/en/latest/esp32s3/index.html) 的模组布局一节。

官方 STEP 在规格书第 45 页有提及，但没有稳定的直链；需要 3D 时从该页 Autodesk/3D 链接取，不要用第三方 DevKit 模型代替本模组。
