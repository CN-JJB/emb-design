# 美新 MMC5983MA 三轴磁传感器资料索引

> 目录 id：[`mmc5983ma`](../catalog.yaml) · 结构化卡片：[device.yaml](device.yaml)
>
> 仅 **15×10 mm、四针 `VIN/GND/SDA/SCL`** 这块模块。芯片手册里的 SPI/INT 不能直接套。

## 按内容看过之后的结论

| 文件 | 实际是什么 | 归入 |
|---|---|---|
| 淘宝尺寸图 | PCB 15.00×10.00 mm；单排 VIN/GND/SDA/SCL；U2 为传感器位 | `mechanical/` |
| Datasheet Rev A | 芯片手册 | `references/chip/` |
| SparkFun Arduino 库 v1.1.5 | 可用，但库内 SPI/INT 示例不适用于本四针板 | `software/` |

没有模块原理图、没有正反面实物照片，不能证明 VIN 支持 5 V。

## 优先查阅

- [接线与首次验证](docs/module-and-wiring.md)
- [寄存器](docs/registers-and-measurement.md)
- [校准与安装](docs/calibration-and-placement.md)

## 关键识别值

| 项目 | 结论 |
|---|---|
| I²C 7 位地址 | `0x30` |
| Product ID | `0x2F` → `0x30` |
| 量程 | ±8 G / 轴 |
| 首次供电 | 3.3 V |
