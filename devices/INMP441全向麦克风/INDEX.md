# INMP441 I²S 全向麦克风模块资料索引

> 目录 id：[`inmp441`](../catalog.yaml) · 结构化卡片：[device.yaml](device.yaml)
>
> 适用对象：**圆形约 14 mm、已焊两排 1×3、丝印 `L/R WS SCK` / `SD VDD GND`** 的 I²S 数字麦模块，商家编码 `MK025336`。不是驻极体、不是 PDM 麦、不是未焊排针的 SKU。

## 按内容看过之后的结论

| 文件 | 实际是什么 | 归入 |
|---|---|---|
| 带引脚说明的正面图 | 丝印 `L/R WS SCK`、`SD VDD GND`；VDD 标 1.8–3.3 V | `images/` |
| 丝印照片 | 同一块圆板的丝印，排针已焊 | `images/` |
| 背面尺寸图 | PCB 约 Ø14 mm；两排针距约 12 mm；中央是 MEMS 封装 | `mechanical/` |
| 商品合成尺寸图 | 商家编码 MK025336；另标厚 1 mm、带针高 9 mm。购物合成图，未实测 | `mechanical/` |
| InvenSense DS-INMP441-00 Rev 1.1 | 芯片手册（底孔、I²S 24 bit）。只约束芯片电气，不证明板上焊的就是这颗 | `references/chip/` |
| 接口说明、FAQ、语音识别海报、已焊/未焊对比、同内容重复 WEBP | 购物文案或重复文件 | **已删除** |

没有模块原理图，没有可辨认的芯片丝印特写，不能证明是原厂 TDK INMP441。

## 优先查阅

- [识别、接线、I²S 与首次采集](docs/hardware.md)
- [原始资料审计与来源](docs/source-audit-and-sources.md)

## 本地资料

- [正面针序说明](images/module-front-pinout.webp)
- [正面丝印](images/module-front-silkscreen.webp)
- [背面与 Ø14 mm / 12 mm](mechanical/pcb-14mm-back.webp)
- [商家尺寸叠加（含 SKU）](mechanical/merchant-overlay-14mm.jpg)
- [INMP441 DS-INMP441-00 Rev 1.1](references/chip/INMP441-Datasheet-DS-INMP441-00-Rev1.1.pdf)

## 关键识别值

| 项目 | 结论 | 依据 |
|---|---|---|
| 接口 | I²S 从机，两排 1×3 | 丝印照片 |
| 丝印 | `L/R` `WS` `SCK` / `SD` `VDD` `GND` | 丝印照片 |
| L/R | 低 = 左声道，高 = 右声道 | 手册 Table 6；商家图一致 |
| VDD | 首次 3.3 V；禁止 5 V | 手册 1.62–3.63 V |
| I²S | Philips，24 bit，每帧 64×SCK | 手册 p.11 |
| PCB | Ø14 mm，两排间距 12 mm | 商家尺寸图，未实测 |
| SKU | MK025336，已焊排针，国产版 | 商品合成图 |

## 使用前必看

- 接线只看丝印，不要套别家圆板的针序照片。
- 3.3 V MCU 的 VDD 接 3.3 V，共地；L/R 接 GND 或 3.3 V，不要悬空当正式配置。
- 主控做 I²S 主机。SD 接到 MCU 的 I²S 数据输入。
- 双麦共用 SCK/WS/SD 时：一颗 L/R 接地，另一颗接 VDD；SD 加 100 kΩ 下拉。
- 两排间距按 12 mm 的两个独立 1×3 处理，未量实物前不要画成标准 2×3。
