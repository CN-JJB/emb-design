# 硬件、青春版配置与引脚占用

## 1. 板卡身份

本目录对应的实物是 **立创·天空星 STM32F407VET6 开发板—青春版**，核心器件为 STMicroelectronics `STM32F407VET6`（LQFP100）。原始资料中的 `原理图必读.md` 明确说明这是 STM32F407VET6 版本；EDA 工程虽然保留“国产芯片兼容”设计页，但不能据此把本板当作 GD32/AT32 型号使用。

MCU 关键参数：

- Arm Cortex-M4F，最高 168 MHz，带单精度 FPU 和 DSP 指令。
- 512 KB 内部 Flash。
- 192 KB SRAM（其中包含 64 KB CCM）以及 4 KB 备份 SRAM。
- LQFP100 封装；板上约引出 70 个普通 GPIO。
- 板卡外形约 45.0 mm × 69.9 mm。

完整电气参数必须以 [STM32F407VE 数据手册](../references/chip/STM32F407VE-Datasheet-DS8626-Rev12.pdf) 为准，寄存器和外设行为以 [RM0090](../references/chip/RM0090-Rev22.pdf) 为准。

## 2. 青春版实装状态

原理图同时画出了可选器件，不能把“有焊盘”误认为“青春版已安装”。根据青春版商品资料、实物宣传图和原理图，默认状态如下：

| 项目 | 青春版默认状态 | 影响 |
|---|---|---|
| STM32F407VET6 | 已安装 | 512 KB Flash 的目标器件 |
| 8 MHz HSE 晶振 | 已安装 | 原始工程按 8 MHz HSE 配置系统时钟 |
| W25Q128 SPI Flash | 未安装 | 两个 `014 ... SPI(flash)` 例程需先补焊兼容器件 |
| TF 卡座 | 已安装 | 可使用 SDIO；TF 接口 ESD 阵列默认未安装 |
| 32.768 kHz LSE 晶振 | 未安装 | 原始 RTC 例程会等待 LSE 就绪，不能直接运行 |
| CR1220 RTC 电池座 | 未安装 | 掉电保持实验需补焊电池座及相关器件 |
| 外部参考源 REF3030 | 未安装 | `REF` 引脚不要当作已存在的精密 3.0 V 基准 |
| P1/P2 两组 2×20 排针 | 未随板焊接/配套 | 需要自行焊接后接入 70 路引脚 |
| 2×5 调试排针 | 未随板焊接/配套 | 可直接飞线或自行焊接排针 |
| 金属收纳盒 | 无 | 与高配套装差异，不影响电路 |

## 3. 板载功能与固定引脚

| 功能 | MCU 引脚 | 说明 |
|---|---|---|
| 用户按键 / WKUP | PA0 | 按键与普通 GPIO 复用，使用时检查上下拉与外部连接 |
| 用户 LED / BOOT1 | PB2 | LED 接向 GND；原始点灯例程把 PB2 置高点亮，即有效高 |
| RESET | NRST | 板载复位按键 |
| DFU / BOOT0 | BOOT0 | 配合复位进入系统存储器 Bootloader；BOOT1 应保持低 |
| USB Device D− | PA11 | USB-C 接口，经 22 Ω 串联电阻并带 USB ESD 保护 |
| USB Device D+ | PA12 | USB-C 接口，经 22 Ω 串联电阻并带 USB ESD 保护 |
| USART1 TX | PA9 | 调试串口发送；板上无 USB-UART 芯片 |
| USART1 RX | PA10 | 调试串口接收；接外部 3.3 V USB-TTL |
| SWDIO | PA13 | SWD 下载/调试 |
| SWCLK | PA14 | SWD 下载/调试 |
| SWO | PB3 | 单线跟踪输出，可选 |
| 调试复位 | NRST | ST-Link 可选连接 |

PB2 同时是 BOOT1 和用户 LED。正常从用户 Flash 启动时保持默认配置即可；进入系统 Bootloader 时不要用外部电路把 PB2 拉高。

## 4. TF 卡与可选 SPI Flash

### TF 卡（SDIO 四线）

| 信号 | MCU 引脚 |
|---|---|
| SDIO_CK | PC12 |
| SDIO_CMD | PD2 |
| SDIO_D0 | PC8 |
| SDIO_D1 | PC9 |
| SDIO_D2 | PC10 |
| SDIO_D3 | PC11 |
| 卡检测 | PD3 |

上述引脚也会出现在扩展排针上。启用 TF 卡后，不要再把它们分配给普通 GPIO、UART5 或其他复用功能。卡座已安装，但青春版默认缺少 TF 接口的可选 ESD 阵列，接触和带电插拔时应更谨慎。

### 可选 W25Q128（SPI1）

| 信号 | MCU 引脚 |
|---|---|
| CS | PA4 |
| SCK | PA5 |
| MISO | PA6 |
| MOSI | PA7 |

原始软、硬 SPI 示例都使用这组引脚。青春版只有焊盘，没有默认安装 W25Q128；未补焊时读 ID、擦除和写入均不会得到有效结果。

## 5. 70 路扩展 GPIO 范围

两组 2×20 排针中的普通 GPIO 合计约 70 路，按端口归纳如下：

- `PA0–PA8`、`PA15`：10 路。
- `PB0–PB1`、`PB4–PB15`：14 路。
- `PC0–PC13`：14 路。
- `PD0–PD15`：16 路。
- `PE0–PE15`：16 路。

合计 70 路。没有列入普通扩展排针的主要引脚用途如下：

- `PA9/PA10`：USART1 调试串口。
- `PA11/PA12`：USB FS。
- `PA13/PA14`：SWD。
- `PB2`：用户 LED / BOOT1。
- `PB3`：SWO。
- `PC14/PC15`：LSE 晶振位置。

逐针对应关系见[P1/P2 双排针逐针对应表](pin-header-p1-p2-map.md)（由官方 EDA 工程反解，含排针间距）。

扩展排针的复用功能很多，接线前还应查阅[原始引脚分配图](../electrical/pin-assignment.pdf)，不要只依赖端口归纳表。

## 6. 电源与机械

- USB-C 提供 5 V 输入，输入侧有约 500 mA 自恢复保险丝。
- 板上通过 3.3 V LDO 为 MCU 和外设供电，并带电源指示灯。
- 扩展排针同时提供 5 V、3.3 V、GND，以及模拟地/参考相关位置；外部供电前先核对原理图，避免与 USB 5 V 反向并联。
- 四个安装孔连接 GND，可作为机壳接地参考，但机械固定件是否导电需由整机设计决定。

## 7. 易冲突资源

| 场景 | 冲突点 |
|---|---|
| 点亮用户 LED | 占用 PB2，同时影响 BOOT1 电平环境 |
| 使用 WKUP 按键 | 占用 PA0，无法同时无条件作为外部模拟/定时器输入 |
| 使用 USB Device/DFU | 占用 PA11/PA12 |
| 使用 TF 卡 | 占用 PC8–PC12、PD2、PD3 |
| 使用板上 SPI Flash 焊盘 | 占用 PA4–PA7 |
| 使用 SWD/SWO | 保留 PA13、PA14、PB3，避免过早重映射或禁用调试口 |
| 使用外部 LSE | 占用 PC14/PC15，并需要青春版补焊晶振及负载器件 |

## 8. 本页依据

- 本地[板卡原理图](../electrical/schematic-2024-01-17.pdf)、[引脚分配图](../electrical/pin-assignment.pdf)及 [EDA 工程](../references/eda/立创梁山派·天空星开发板_2024-01-17.epro)。
- 立创开发板官方[青春版项目页](https://lckfb.com/project/detail/lckfb-lspi-skystar-stm32f407vet6-lite?param=baseInfo&collection=1a5d9324392d46b98958a8f223846ab0)。
- 立创开发板技术文档中心[天空星 STM32 开发板介绍](https://wiki.lckfb.com/zh-hans/tkx/tkx-stm32f407vxt6/)。
- STMicroelectronics [STM32F407VE 产品页](https://www.st.com/en/microcontrollers-microprocessors/stm32f407ve.html)。

在线来源核验日期：2026-08-09。更多来源和版本信息见[官方来源清单](official-sources.md)。
