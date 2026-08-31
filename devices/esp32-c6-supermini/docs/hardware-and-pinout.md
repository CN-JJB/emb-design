# ESP32-C6 Super Mini 硬件与引脚说明

## 1. 板型结论

本目录对应的是 26.04 mm × 18.00 mm 的 ESP32-C6 Super Mini。板上主芯片在原理图中标为 `ESP32-C6FH4`，即 QFN32 封装并内置 4 MB Flash 的 ESP32-C6。它不是 ESP32-C6-WROOM-1 模组板，也不是先前混入目录的 ESP32-C6 HXB 板。

识别依据：

- 裸片 ESP32-C6FH4 和板边 PCB 天线；
- 单个 USB-C，使用芯片原生 USB Serial/JTAG；
- `B+` / `B-` 单节锂电池焊盘和 TP4054 充电电路；
- GPIO8 上的 WS2812B RGB LED、GPIO15 上的蓝色 LED；
- BOOT 与 RST 两个按键。

本地证据：[Super Mini 原理图](<../electrical/schematic.png>)、[正面尺寸图](<../mechanical/board-front-26x18mm.png>)、[背面尺寸图](<../mechanical/board-back-26x18mm.png>)。

## 2. 核心能力

ESP32-C6 是单核 32 位 RISC-V SoC，最高 160 MHz，支持 2.4 GHz Wi-Fi 6、Bluetooth LE 5、IEEE 802.15.4（Zigbee / Thread），并提供 USB Serial/JTAG、UART、SPI、I2C、I2S、LEDC、RMT、ADC、TWAI 等外设。Super Mini 使用的 C6FH4 在封装内集成 4 MB Flash。

这些是芯片能力，不表示每项功能都已经在开发板上配好外围电路。例如 TWAI/CAN 仍需外接收发器，Zigbee/Thread 仍需相应协议栈和网络角色配置。

## 3. 排针与焊盘

排针相对位置以实物试插为准，不以商家正面尺寸图的方向表述为几何真值。

商家图写「USB-C 在左侧」。若按该表述旋转，会把电源列算到与 ESP32-C3 SuperMini 相反的一侧。
2026-08-13 用户实测：C3 与 C6 的排针间距、行距、孔位相同；两块板都从 USB 端对齐后，
第 1 针 = `5V`、第 2 针 = `GND`、第 3 针 = `3V3`。本项目按这个实测方向使用。

下表仍按「从 USB 端数起」列出，不再使用「图面左侧」这种会翻转的说法。

| USB 端起，电源列 | 5V | GND | 3V3 | GPIO20 | GPIO19 | GPIO18 | GPIO15 | GPIO14 | GPIO9 | GPIO8 |
|---|---|---|---|---|---|---|---|---|---|---|
| USB 端起，对面列 | TX/GPIO16 | RX/GPIO17 | GPIO0 | GPIO1 | GPIO2 | GPIO3 | GPIO4 | GPIO5 | GPIO6 | GPIO7 |

额外小焊盘：

| 焊盘 | 说明 |
|---|---|
| GPIO12 | 原生 USB D-，经 22 Ω 串联电阻连接 USB-C |
| GPIO13 | 原生 USB D+，经 22 Ω 串联电阻连接 USB-C |
| GPIO21、GPIO22、GPIO23 | 原理图标为 Micro:bit Pads，未接主排针 |
| B+、B- | 单节锂离子/锂聚合物电池接口；焊接前必须核对实物极性 |

## 4. 板载占用和启动限制

| GPIO / 信号 | 板载连接 | 使用注意 |
|---|---|---|
| GPIO8 | WS2812B DIN；10 kΩ 上拉 | 同时是启动配置脚；下载模式要求为高。外设不得在复位时强拉低 |
| GPIO9 | BOOT 按键到 GND；10 kΩ 上拉 | 启动配置脚；按住 BOOT 后复位进入下载模式 |
| GPIO15 | 蓝色 LED 经 1 kΩ 到 GND | 高电平点亮；同时影响 JTAG 信号源选择，复位窗口避免外部强驱动 |
| GPIO12 | USB D- | 默认供 USB Serial/JTAG 使用；改作普通 GPIO 会失去原生 USB 功能 |
| GPIO13 | USB D+ | 默认供 USB Serial/JTAG 使用；改作普通 GPIO 会失去原生 USB 功能 |
| GPIO16 | UART0 TX | 板上标为 TX |
| GPIO17 | UART0 RX | 板上标为 RX |
| CHIP_EN | RST 按键到 GND，10 kΩ 上拉 | 低电平复位芯片 |

ESP32-C6 的下载启动组合为 GPIO8=1、GPIO9=0。板载 10 kΩ 上拉保证 GPIO8 的默认状态，BOOT 按键负责把 GPIO9 拉低。

## 5. 模拟、总线和 GPIO 选择

- ADC1 通道只位于 GPIO0-GPIO6；GPIO7 不是 ADC 引脚。
- ESP32-C6 GPIO Matrix 允许多种数字外设信号灵活映射，但应优先避开 GPIO8、GPIO9、GPIO15 的复位约束，以及 GPIO12/GPIO13 的 USB 占用。
- GPIO16/GPIO17 是 UART0 默认脚；如需保留硬件串口调试，不要复用。
- 对噪声敏感的 ADC 输入应靠近芯片去耦、缩短走线，并按官方数据手册建议增加滤波；Wi-Fi 工作会增加模拟测量噪声。
- 单 GPIO 的绝对最大值不是负载驱动预算。电机、继电器、大功率 LED 和总线收发器必须使用外部驱动/电平转换。

建议的普通外设优先候选是 GPIO0-GPIO7、GPIO14、GPIO18-GPIO23，再按 ADC、串口、USB和启动需求排除冲突。

## 6. 供电与电池

原理图显示：

1. USB-C VBUS 形成 `VBUS`，一支路进入 TP4054 给 `BAT` 充电；
2. `VBUS` 与 `BAT` 分别经二极管汇合到 `VCC`，形成简单电源 OR；
3. `VCC` 经 3.3 V LDO 输出 `3V3`；
4. USB 插入时有独立充电指示 LED。

商家附带的是 ME6211 系列 LDO 数据手册。原理图中的 U2 为 5 引脚、CE 接 VIN 的接法，更接近 ME6211C33M5G 一类器件；但原理图未明确写出完整料号，最终应以实物丝印/BOM 为准。

ME6211 系列手册给出的典型能力包括 2.0-6.0 V 输入、标称最大 500 mA（特定压差条件）和 100 mA 时约 100 mV 压差。500 mA 是芯片条件值，不等于 3V3 排针可长期全部使用的电流；还要扣除 ESP32-C6 的射频峰值电流并考虑小板散热、二极管压降和器件批次。

电池注意事项：

- 仅接单节 3.7 V 标称锂离子/锂聚合物电池；
- 图中可见充电 IC，但未见独立过放/过流保护 IC，优先使用带保护板的电池；
- 不要同时从 5V、3V3 和电池端反向灌电；
- 连接电池前实测 `B+`、`B-` 极性和充电电压。

## 7. 射频和机械安装

- 外形约 26.04 mm × 18.00 mm，主焊盘跨距约 22.86 mm。
- PCB 天线位于板边。安装时让天线区域伸出载板边缘，天线前后及附近避免铜皮、电池、屏蔽罩、金属外壳和密集线束。
- 最终产品应按 Espressif ESP32-C6 硬件设计指南复核电源去耦、复位、射频净空和 ESD。

## 8. 依据与可信度

- 板级连接：以本目录商家原理图和尺寸图为依据，可信度高，但仍可能存在不同卖家批次改板。
- 芯片电气与启动：以 [官方数据手册 v1.5](../references/chip/ESP32-C6-Series-Datasheet-v1.5.pdf) 为依据。
- 寄存器与外设细节：以 [官方技术参考手册 v1.2](../references/chip/ESP32-C6-Technical-Reference-Manual-v1.2.pdf) 为依据。
- 量产前必须对手中实物做连通性、空载电流、3V3 电压和板载 LED/按键实测，不能只依赖商家图片。
