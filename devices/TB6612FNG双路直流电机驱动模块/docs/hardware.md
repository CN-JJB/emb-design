# D153C 针序与使用

针序已由用户对照实物逐字确认。图片来源：[接口](../images/d153c-pinout.png)、[保护](../images/d153c-protection-components.png)、[尺寸](../mechanical/d153c-mechanical-dimensions.png)。

## 排针（均自上而下）

| 排针 | 丝印 | 1–7 |
|---|---|---|
| H2 | Output_IO | E2B, E2A, E1B, E1A, ADC, GND, 5V |
| H1 | Input_IO | PWMB, BIN2, BIN1, STBY, AIN1, AIN2, PWMA |

## 电机白座 6P

| 座 | 1–6 |
|---|---|
| 电机 B | BO1, **3V3**, E2A, E2B, GND, BO2 |
| 电机 A | AO2, **3V3**, E1A, E1B, GND, AO1 |

编码器由 `3V3` 供电，输出是 3.3 V 电平。H2 的 `5V` 是给用户的辅助输出。

与 MG513 的 `1 电机+ / 2 编码器地 / 3 B / 4 A / 5 编码器电源 / 6 电机−` **方向相反**。接线前量电机插头第 5 脚对第 2 脚是否为 3.3 V。

螺钉端子 VM 4.5–15 V；J3 为输入并联输出。底部另有 5 V / GND / 3.3 V 辅助。PWM 推荐 10 kHz。

主电源未通不要从 IO 反灌。用绝缘柱安装。持续电流 1.2 A/路。精密传感器不要跟电机共用这块板的 5 V/3.3 V。
