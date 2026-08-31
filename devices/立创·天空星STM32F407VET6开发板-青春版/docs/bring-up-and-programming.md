# 首次上电、下载与调试

## 1. 建议的首次验证路线

1. 对照[青春版实装表](hardware-and-pinout.md#2-青春版实装状态)检查板上器件，尤其不要把 W25Q128、LSE 晶振和 RTC 电池座当作已安装。
2. 仅通过 USB-C 给板供电，确认电源指示灯正常，3.3 V 电源无明显短路或异常发热。
3. 使用 ST-Link 的 SWD 模式连接 `3V3/VTref`、`GND`、`PA13/SWDIO`、`PA14/SWCLK`，必要时再接 `NRST`。
4. 先烧录 `002库函数点灯.zip`，验证 PB2 用户 LED。
5. 再运行 `005串口打印信息.zip`，用外接 3.3 V USB-TTL 验证 USART1（PA9/PA10，115200 baud）。
6. 基础通路稳定后，再测试按键、定时器、ADC、TF 卡或外接模块。

## 2. SWD 接线

| ST-Link | 开发板 | 说明 |
|---|---|---|
| VTref / 3.3V sense | 3V3 | 作为目标电平参考；是否由 ST-Link 供电取决于调试器型号 |
| GND | GND | 必接，共地 |
| SWDIO | PA13 / SWDIO | 双向数据 |
| SWCLK | PA14 / SWCLK | 调试时钟 |
| NRST | NRST | 推荐接入，连接异常时更容易恢复 |
| SWO | PB3 / SWO | 可选，仅需要跟踪输出时连接 |

初次连接建议把 SWD 时钟降到 1–4 MHz。若程序把调试口重配、进入低功耗或时钟配置失败，可在 STM32CubeProgrammer 中使用 “Connect under reset”，并确保 NRST 已连接。

## 3. 使用原始 Keil 标准库工程

原始模板和全部板级示例使用 Keil MDK 工程文件 `.uvprojx`，目标配置为：

- Device：`STM32F407VETx`
- Vendor：STMicroelectronics
- 预处理宏：`USE_STDPERIPH_DRIVER, STM32F40_41xxx`
- 外部高速晶振：8 MHz
- 标准外设库（SPL），不是 STM32Cube HAL 工程

基本步骤：

1. 安装 Keil MDK；原始安装包位于 `第05章...开发工具/KeilMDK安装包.zip`。
2. 安装 `Keil.STM32F4xx_DFP.2.17.1.pack`，或从 Keil Pack Installer 获取当前兼容版本。
3. 解压[空白工程模板](../software/board-examples/STM32F407_ProjectTemplate.zip)或某个例程到短路径、纯英文工作目录。
4. 打开 `.uvprojx`，确认目标仍是 `STM32F407VETx`，不要改选 F407VG/ZG 或其他 F4 型号。
5. 选择 ST-Link Debug，编译并下载。

原始工程把 8 MHz HSE 配置写入 CMSIS 系统文件。若自行新建工程，必须再次确认 HSE 值、PLL 参数、Flash 等待周期以及 512 KB Flash 的链接范围。

## 4. 使用 STM32Cube 生态新建工程

SPL 已是历史维护状态。新项目可使用 STM32CubeMX / STM32CubeIDE 或 [STM32CubeF4](https://github.com/STMicroelectronics/STM32CubeF4) 的 HAL/LL：

1. MCU 选择 `STM32F407VETx`，不要仅凭“F407”选择其他封装或 Flash 容量。
2. RCC 选择外部晶振 HSE，频率设为 8 MHz；常规目标 SYSCLK 可设为 168 MHz。
3. Debug 选择 Serial Wire，保留 PA13/PA14；需要 SWO 时保留 PB3。
4. 根据实际功能启用 USART1、USB FS、SDIO 或 SPI1，参照[引脚占用表](hardware-and-pinout.md)。
5. 链接脚本/Flash size 必须是 512 KB；不要直接复制 `STM32F407VG` 的 1 MB Flash 配置。
6. CubeF4 没有这块天空星板的官方 BSP，需要按本目录原理图建立自己的 board 层。

## 5. 串口调试

板上没有 CH340/CP210x 等 USB-UART 芯片。USB-C 的 D+/D− 直接连接 PA12/PA11，用作 MCU USB FS 或系统 DFU。

USART1 调试接线：

| USB-TTL | 开发板 |
|---|---|
| TXD | PA10 / USART1_RX |
| RXD | PA9 / USART1_TX |
| GND | GND |

使用 3.3 V TTL 电平，不要把 RS-232 电平直接接入 MCU。`005串口打印信息` 示例调用 `uart1_init(115200U)`；常用终端设置为 115200、8 数据位、无校验、1 停止位。

原始资料中的 CH340 驱动是为外接 USB-TTL 适配器准备的，不代表板载 CH340。

## 6. USB DFU / 系统 Bootloader

STM32F407 的系统存储器 Bootloader 支持 USB OTG FS DFU。板上 DFU 按键连接 BOOT0，典型进入方法为：

1. 保证 PB2/BOOT1 处于默认低电平环境。
2. 按住 DFU/BOOT0。
3. 按一下 RESET，先松开 RESET，再松开 DFU。
4. 在 [STM32CubeProgrammer](https://www.st.com/en/development-tools/stm32cubeprog.html) 中选择 USB，刷新并连接 DFU 设备。
5. 下载完成后复位，使 BOOT0 回到低电平，从用户 Flash 启动。

系统 Bootloader 的接口和芯片修订适用性以 ST [AN2606](https://www.st.com/resource/en/application_note/an2606-stm32-microcontroller-system-memory-boot-mode-stmicroelectronics.pdf) 为准。若 USB 无法枚举，先排除线材仅供电、驱动、BOOT 电平和 PA11/PA12 被外部电路占用等问题。

## 7. 代表性例程的硬件前提

| 例程 | 已核对的配置 | 青春版注意事项 |
|---|---|---|
| 002 库函数点灯 | PB2 输出，置高点亮 | 可直接验证；PB2 同时是 BOOT1 |
| 005 串口打印 | USART1，PA9/PA10，115200 | 需要外接 3.3 V USB-TTL |
| 014 硬件 SPI Flash | SPI1，PA4/PA5/PA6/PA7，W25Q128 | W25Q128 默认未焊，不能直接验证 |
| 014 软件 SPI Flash | GPIO 模拟 SPI，仍使用 PA4–PA7 | W25Q128 默认未焊 |
| 015 RTC | 代码启用 LSE 并选择 LSE 为 RTC 时钟 | LSE 晶振和电池座默认未焊；需补焊或改用 LSI |

RTC 示例包含等待 `RCC_FLAG_LSERDY` 的流程。青春版未补焊 LSE 时可能一直等待；若改用 LSI，需要同时修改 RTC 时钟源和预分频，并接受 LSI 精度较低的事实。

## 8. 常见故障定位

| 现象 | 优先检查 |
|---|---|
| ST-Link 找不到芯片 | 3.3 V、共地、SWDIO/SWCLK 是否接反；降低 SWD 频率；接 NRST 并 under-reset 连接 |
| 下载后 LED 不亮 | 是否烧录到正确目标；PB2 是否被作为 BOOT1/外部电路拉扯；例程是否真正执行到置高 |
| 串口乱码/无输出 | 115200 8N1；TX/RX 交叉；3.3 V 电平；确认使用 PA9/PA10 而非 USB-C |
| 程序卡在系统初始化 | HSE 是否为 8 MHz；PLL 宏是否被替换；Flash 等待周期是否正确 |
| RTC 示例卡死 | 青春版缺 LSE；补焊或改用 LSI |
| SPI Flash ID 异常 | 青春版缺 W25Q128；检查补焊方向、供电、PA4–PA7 和 CS 电平 |
| DFU 不枚举 | BOOT0/BOOT1 时序、USB 数据线、USB 驱动、PA11/PA12 占用 |

## 9. 参考

- ST [STM32CubeProgrammer](https://www.st.com/en/development-tools/stm32cubeprog.html)
- ST [STM32CubeF4](https://github.com/STMicroelectronics/STM32CubeF4)
- ST [AN2606 系统存储器 Boot 模式](https://www.st.com/resource/en/application_note/an2606-stm32-microcontroller-system-memory-boot-mode-stmicroelectronics.pdf)
- ST [PM0214 Cortex-M4 编程手册](https://www.st.com/resource/en/programming_manual/pm0214-stm32-cortexm4-mcus-and-mpus-programming-manual-stmicroelectronics.pdf)

在线来源核验日期：2026-08-09。
