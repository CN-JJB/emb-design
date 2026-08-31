# 0.96 寸 OLED 模块资料索引

> 目录 id：[`ssd1306-096`](../catalog.yaml) · 结构化卡片：[device.yaml](device.yaml)
>
> 适用对象：**SSD1306、128×64、4 针 I²C** 模块，排针丝印为 `GND / VCC / SCL / SDA`。本目录不作为 SH1106、128×32、六针或七针 SPI 模块的资料库。

## 按内容看过之后的结论

| 文件 | 实际是什么 | 归入 |
|---|---|---|
| 商品长图（原 WEBP） | 正面丝印 `GND VCC SCL SDA`；背面 `662K` 稳压器与地址焊盘 `0x78 / 0x7A`。文案把孔写成 M3、外形写成 27×27×2 mm，与图纸不符，文案不作依据 | `images/` |
| 四针 B 版结构图 | PCB 27.300×27.800 mm，孔内径 2 mm / 外径 3.5 mm。图上排针文字印成 `VCC GND SCL SDA`，与丝印和原理图矛盾，**接线不以这行字为准** | `mechanical/` |
| 中景园四针 I²C 原理图 | 接插件 J1：1=`GND`、2=`VCC_IN`、3=`SCL`、4=`SDA`；662K 约 3.3 V LDO；SCL/SDA 各 4.7 kΩ 上拉 | `electrical/` |
| Allvision 面板规格 Ver C | 玻璃 26.70×19.26×1.4 mm，有效区 21.744×10.864 mm，驱动 IC SSD1306，料号 `QG-2864KLBEG01` | `references/panel/` |
| Solomon SSD1306 Rev 1.1 | 芯片手册，日常查阅这份 | `references/chip/` |
| Adafruit SSD1306 2.5.17 | 推荐驱动；库内还有 128×32 / SPI 例程，构造本模块时必须 128×64、I²C、地址 `0x3C` | `software/` |
| 中景园手册 V2.0、商家 RAR 其余 | 七针 SPI、六针图、Rev 1.0 手册、字表、U8glib、Bluno、STM32 SPI | **已删除** |

## 优先查阅

- [模块识别、接线、地址、初始化与故障排查](docs/module-guide.md)
- [原始资料审计、型号纯净性与全部来源](docs/source-audit-and-sources.md)

## 本地资料

- [正面 / 背面与丝印](images/module-front-back-pinout.webp)
- [四针 B 版结构图](mechanical/pcb-4pin-B-27.3x27.8mm.pdf)
- [四针 I²C 模块原理图](electrical/module-i2c-4pin-schematic.pdf)
- [SSD1306 Rev 1.1 芯片手册](references/chip/SSD1306-Datasheet-Rev1.1.pdf)
- [Allvision 面板规格 QG-2864KLBEG01 Ver C](references/panel/Allvision-QG-2864KLBEG01-VerC.pdf)
- [Adafruit SSD1306 v2.5.17](software/Adafruit_SSD1306-d94f699.zip)

## 关键识别值

| 项目 | 当前模块 |
|---|---|
| 驱动芯片 | SSD1306 |
| 面板料号（规格书） | Allvision `QG-2864KLBEG01` / `ZJY-2864KLBEG01` Ver C |
| 显示分辨率 | 128×64；蓝 / 白 / 黄蓝是发光颜色变体 |
| 主机接口 | 4 针 I²C |
| 引脚顺序 | 丝印与原理图：`GND / VCC / SCL / SDA` |
| 常用 7 位地址 | `0x3C`；改焊地址位后可为 `0x3D` |
| 商家标注地址 | `0x78 / 0x7A`（8 位写字节） |
| 模块供电 | 手册标称 3～5.5 V；商品图背面可见 662K。首次接线用 3.3 V |
| PCB | 27.300 × 27.800 mm，厚 1.5 mm（B 版结构图，未实测） |
| 安装孔 | 内径 2.0 mm、外径 3.5 mm，**不是 M3** |
| 显存 | 128 × 64 / 8 = 1024 字节 |

## 使用前必看

- MCU 驱动填 **7 位地址** `0x3C`，不要填 `0x78`。
- Adafruit SSD1306 地址参数为 0 时，128×64 会默认 `0x3D`；必须显式传 `0x3C`。
- 结构图上的 `VCC GND SCL SDA` 与实物丝印相反，接线只看丝印。
- 3.3 V MCU 首次调试时模块 VCC 也接 3.3 V。
- 同外观的六针、七针 SPI 模块不是本目录对象。

## 文件结构

```text
.
├── INDEX.md
├── device.yaml
├── docs/                 # 本库写的中文说明
├── images/               # 本模块照片（丝印、背面）
├── mechanical/           # 板框、孔位、排针位置
├── electrical/           # 本模块原理图
├── software/             # 推荐驱动
└── references/
    ├── chip/             # SSD1306 原厂手册
    └── panel/            # 玻璃面板规格
```
