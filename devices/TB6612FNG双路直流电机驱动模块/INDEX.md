# D153C（TB6612FNG 双路稳压版）资料索引

> 目录 id：[`tb6612-d153c`](../catalog.yaml) · 结构化卡片：[device.yaml](device.yaml)
>
> 适用对象：丝印 `D153C` 的 TB6612 双路带稳压电机驱动。针序已由用户对照实物确认。

## 按内容看过之后的结论

| 文件 | 实际是什么 | 归入 |
|---|---|---|
| `d153c-pinout.png` | 正面照片+接口标注。H2 Output_IO、H1 Input_IO、电机 A/B 白座、J8 输入、J3 并联输出、底 5V/GND/3V3 | `images/` |
| `d153c-protection-components.png` | 同一块板的保护器件标注（TVS、防反接、3.3 V 短路保护） | `images/` |
| 尺寸图 | 50×50 mm，孔 44×44 mm，4×Ø3.2 mm | `mechanical/` |

两张板图不是重复：一张讲接口，一张讲保护。

## 优先查阅

- [针序、电机座与使用注意](docs/hardware.md)

## 关键识别值

| 项目 | 结论 |
|---|---|
| 芯片 | TB6612FNG |
| VM | 4.5–15 V |
| 单路 | 持续 1.2 A，峰值 3.2 A |
| H2 自上而下 | E2B E2A E1B E1A ADC GND 5V |
| H1 自上而下 | PWMB BIN2 BIN1 STBY AIN1 AIN2 PWMA |
| 电机座 6P | 编码器由 **3V3** 供电，不是 5V |
| 与 MG513 | 6P 线序方向相反 |

H2 上的 `5V` 是辅助输出，编码器电平是 3.3 V。
