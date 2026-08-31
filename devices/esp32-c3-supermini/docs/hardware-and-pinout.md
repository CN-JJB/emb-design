# ESP32-C3 Super Mini 硬件与引脚说明

## 1. 板型和芯片识别

这是一块约 18.00 mm × 22.50 mm 的 ESP32-C3 Super Mini，使用 QFN32 裸片、板边 PCB 天线、单 USB-C、BOOT/RST 按键和两排各 8 个焊盘。

原理图主芯片只写 `ESP32-C3`，尺寸图只写 `QFN32-5×5`，没有完整料号。板上也没有看到独立 SPI Flash，说明应使用带封装内 Flash 的 C3 变体，但现有资料不足以判断是 C3FN4、C3FH4、C3FH4X 或其他后缀，也不足以确认 4 MB/8 MB 容量。

官方数据手册 v2.4 标明 C3FN4 已 EOL、C3FH4AZ 不推荐新设计、C3FH4X 为推荐型号。这是芯片采购状态，不代表手中开发板一定是哪一种。收到实物后必须读取芯片丝印，并用 `esptool chip-id` / `flash-id` 验证。

## 2. 核心能力

ESP32-C3 是最高 160 MHz 的单核 32 位 RISC-V SoC，支持 2.4 GHz 802.11b/g/n Wi-Fi 和 Bluetooth 5 LE。它不支持 Bluetooth Classic。芯片还提供 USB Serial/JTAG、UART、SPI、I2C、I2S、LEDC、RMT、ADC、TWAI 等外设。

板卡没有集成 CAN/TWAI 收发器、电机驱动或传感器；这些能力需要外接外围电路。

## 3. 排针映射

以本目录电路图中 H1/H2 编号为准：

| H1 引脚 | 网络 | 关键复用 |
|---:|---|---|
| 1 | GPIO5 | ADC2_CH0；图中建议作 SCL |
| 2 | GPIO6 | JTAG MTCK |
| 3 | GPIO7 | JTAG MTDO |
| 4 | GPIO8 | 板载用户 LED；启动配置脚 |
| 5 | GPIO9 | BOOT；启动配置脚 |
| 6 | GPIO10 | 通用 GPIO |
| 7 | GPIO20 | UART0 RX |
| 8 | GPIO21 | UART0 TX |

| H2 引脚 | 网络 | 关键复用 |
|---:|---|---|
| 1 | GPIO0 | ADC1_CH0；图中标 U1TXD |
| 2 | GPIO1 | ADC1_CH1；图中标 U1RXD |
| 3 | GPIO2 | ADC1_CH2；启动配置脚 |
| 4 | GPIO3 | ADC1_CH3 |
| 5 | GPIO4 | ADC1_CH4；JTAG MTMS；图中建议作 SDA |
| 6 | 3.3V | LDO 输出 |
| 7 | GND | 地 |
| 8 | VBUS/5V | USB 5 V 入口 |

图中的 GPIO0/GPIO1 `U1TXD/U1RXD` 只是一个复用方案；UART 信号可通过 GPIO Matrix 重映射。默认 ROM 日志和 UART0 更应关注 GPIO21/GPIO20。

## 4. USB、按键和 LED

| 信号 | 板载连接 | 注意事项 |
|---|---|---|
| GPIO18 | USB D-，经 22 Ω 到 USB-C | 默认 USB Serial/JTAG；未接主排针 |
| GPIO19 | USB D+，经 22 Ω 到 USB-C | 默认 USB Serial/JTAG；未接主排针 |
| GPIO9 | BOOT 按键到 GND、10 kΩ 上拉 | 按住后复位进入下载模式 |
| CHIP_EN | RST 按键到 GND、10 kΩ 上拉、100 nF 到 GND | 低电平复位 |
| GPIO8 | 用户 LED 连接到 3.3 V 侧 | **低电平点亮**；同时是启动配置脚 |
| PWR LED | VSYS 经 10 kΩ 到 GND | 上电指示，与 GPIO 无关 |

板上没有 CH340/CP210x USB-UART 芯片。USB-C 直接连接 ESP32-C3 的 GPIO18/GPIO19 原生 USB Serial/JTAG；只有充电功能的 USB 线不能用于下载。

## 5. 启动配置脚

ESP32-C3 的启动配置脚是 GPIO2、GPIO8、GPIO9：

- 正常 SPI Boot：GPIO9=1；官方建议 GPIO2 上拉；GPIO8 可为任意值，但还会影响 ROM 日志设置；
- USB/UART 联合下载模式：GPIO2=1、GPIO8=1、GPIO9=0；
- 本板 GPIO8、GPIO9 各有 10 kΩ 上拉，BOOT 按键把 GPIO9 拉低；
- 原理图未显示 GPIO2 的外部上拉，而新版数据手册建议为抗毛刺给 GPIO2 上拉。若外设连接 GPIO2，复位时不要把它拉低。

GPIO8 同时接用户 LED，外接负载或过强下拉可能导致启动/日志异常。GPIO2、GPIO8、GPIO9 在复位采样后的正常运行阶段可以当 GPIO 使用，但外部电路仍须兼容复位窗口电平。

## 6. ADC 和调试引脚

- ADC1：GPIO0-GPIO4，共 5 通道，出厂校准；
- ADC2：GPIO5，仅 1 通道，未出厂校准；某些芯片修订的 ADC2 不可用，必须查 ESP32-C3 勘误；
- JTAG 默认占 GPIO4-GPIO7，但通常可直接使用 USB Serial/JTAG 调试，从而释放这些脚；
- UART0 默认 GPIO21 TX / GPIO20 RX；
- GPIO18/GPIO19 若改为普通 GPIO，会失去原生 USB 下载、串口与 JTAG。

芯片的 GPIO Matrix 让数字外设可灵活映射，但应优先避开启动脚、USB、UART0 和板载 LED 的冲突。

## 7. 供电

电源路径为：

1. USB-C VBUS 经过 BAT60J 肖特基二极管形成 `VSYS`；
2. `VSYS` 进入 3.3 V LDO；
3. LDO 的 EN 接输入，输出 `3.3V`；
4. USB CC1/CC2 各有 5.1 kΩ 下拉，声明为 USB 设备端。

商家附带 ME6211 系列手册，原理图 PU1 也是 5 引脚 ME6211 类接法，但完整料号仍应以实物丝印/BOM 为准。ME6211 系列手册给出 2.0-6.0 V 输入、特定条件下最高 500 mA 和低压差特性；开发板实际 3.3 V 可用电流必须扣除 ESP32-C3 射频峰值并考虑小板散热和二极管压降。

注意：

- 这张原理图没有电池充电/保护电路，不要把它当成带锂电管理的小板；
- 3.3V 不能容忍 5V 逻辑输入；
- 电机、继电器和大电流 LED 必须使用外部驱动与独立供电；
- 图中未显示额外 USB ESD 保护，产品化时应补充。

## 8. 射频与机械

- 板外形约 18.00 mm × 22.50 mm，两排焊盘跨距约 15.24 mm；
- PCB 天线在远离 USB-C 的板端；安装时天线附近避免铜、金属、电池和线束；
- [STEP 模型](<../model/Super Mini-ESP32C3.step>) 适合外壳干涉检查；
- [PcbDoc](<../mechanical/Super Mini-ESP32C3-外形.PcbDoc>) 主要是机械外形/排针文件，不是完整可生产 PCB 工程。

依据：[电路图](../electrical/schematic.png)、[尺寸图](../mechanical/board-18x22.5mm.png)、[官方数据手册 v2.4](../references/chip/ESP32-C3-Series-Datasheet-v2.4.pdf)、[官方技术参考手册 v1.4](../references/chip/ESP32-C3-Technical-Reference-Manual-v1.4.pdf)。
