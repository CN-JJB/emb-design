# 板级例程索引

本目录只保留 **天空星本板** 的 Keil + SPL 例程，全部目标 `STM32F407VETx`。解压到短路径英文目录再打开 `.uvprojx`。

位置：[software/board-examples](../software/board-examples/)

| 编号 | 压缩包 | 内容 | 注意 |
|---:|---|---|---|
| 001 | `001寄存器点灯.zip` | 寄存器点灯 | PB2 / BOOT1 |
| 002 | `002库函数点灯.zip` | SPL GPIO | PB2 置高点亮，首次验证用这个 |
| 003 | `003滴答定时器灯闪烁.zip` | SysTick | PB2 |
| 004 | `004位带操作.zip` | 位带 | 移植时留意地址 |
| 005 | `005串口打印信息.zip` | USART1 | PA9/PA10，外接 USB-TTL |
| 006 | `006按键点灯.zip` | 轮询按键 | PA0 / PB2 |
| 007 | `007外部中断按键点灯.zip` | EXTI | PA0 |
| 008 | `008定时器灯闪烁.zip` | 定时器中断 | PB2 |
| 009 | `009PWM呼吸灯.zip` | PWM | 核对通道 |
| 010 | `010串口中断DMA接收二合一.zip` | USART DMA | USART1 |
| 011 | `011ADC采集.zip` | ADC | 先看工程选的引脚 |
| 012 | `012软件I2C(SHT20).zip` | 软件 I²C | 外接 SHT20 |
| 014 | `014硬件SPI(flash).zip` | SPI1 Flash | 青春版 W25Q128 未焊 |
| 014 | `014软件SPI(flash).zip` | 软件 SPI Flash | 同上 |
| 015 | `015RTC时钟实验(可做掉电实验).zip` | RTC | 青春版 LSE/电池座未焊 |

空白模板：[STM32F407_ProjectTemplate.zip](../software/board-examples/STM32F407_ProjectTemplate.zip)。

原 86 个外接模块 ZIP、Keil/Cube 安装包、DFP、视频教程已删除。新项目建议用 STM32CubeF4 HAL/LL。
