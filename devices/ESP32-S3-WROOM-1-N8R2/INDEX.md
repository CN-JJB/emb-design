# ESP32-S3-WROOM-1-N8R2 资料索引

> 目录 id：[`esp32-s3-wroom-1-n8r2`](../catalog.yaml) · 结构化卡片：[device.yaml](device.yaml)
>
> 适用对象：**乐鑫 ESP32-S3-WROOM-1-N8R2** SMT 模组：PCB 天线，8 MB Quad Flash + 2 MB Quad PSRAM，外形 18.0×25.5×3.1 mm。
>
> 不是 WROOM-1U、不是 N8R8/Octal PSRAM、不是 DevKitC 开发板。

## 按内容看过之后的结论

| 文件 | 实际是什么 | 归入 |
|---|---|---|
| 模组规格书 v1.8 | 针序、电气、尺寸、焊盘、典型应用电路。N8R2 在 Table 1-1 | `references/chip/` |
| 芯片规格书 v2.2 | SoC 极限、strapping、S3R2→S3RH2 | `references/chip/` |
| TRM v1.8 | 寄存器与外设，日常不必先打开 | `references/chip/` |
| 封面图裁切 | 仅 WROOM-1（PCB 天线），不含 1U | `images/` |
| Figure 3-1 | 顶视 41 脚布局 | `images/` |
| Figure 10-1 | WROOM-1 尺寸，已去掉同页的 1U 图 | `mechanical/` |
| Figure 11-1 | WROOM-1 推荐焊盘 | `mechanical/` |
| Figure 2-1 / 9-1 | 模组框图、底板典型接法 | `electrical/` |

官方 STEP 没有找到可钉死的直接下载地址，`model/` 未建。硬件设计指南现为 HTML，只在 docs 里给链接，不存未钉版本 PDF。

## 优先查阅

- [硬件、针序与封装](docs/hardware-and-pinout.md)
- [上电与烧录](docs/bring-up-and-flashing.md)
- [官方入口](docs/official-sources.md)
- [资料审计](docs/source-audit.md)

## 本地资料

- [模组顶视](images/module-top.png)
- [41 脚顶视布局](images/pin-layout-top.png)
- [外形 18.0×25.5×3.1 mm](mechanical/module-18x25.5mm.png)
- [推荐焊盘](mechanical/land-pattern.png)
- [底板典型接法](electrical/typical-application.png)
- [模组框图](electrical/module-block.png)
- [模组规格书 v1.8](references/chip/ESP32-S3-WROOM-1-WROOM-1U-Datasheet-v1.8.pdf)
- [芯片规格书 v2.2](references/chip/ESP32-S3-Series-Datasheet-v2.2.pdf)
- [TRM v1.8](references/chip/ESP32-S3-Technical-Reference-Manual-v1.8.pdf)

## 关键识别值

| 项目 | 结论 |
|---|---|
| 料号 | ESP32-S3-WROOM-1-N8R2 |
| Flash / PSRAM | 8 MB Quad / 2 MB Quad |
| GPIO35/36/37 | **可用**（Octal 料号才占用） |
| 供电 | 3.0–3.6 V，典型 3.3 V，外供 ≥ 0.5 A |
| USB | GPIO19 = D−，GPIO20 = D+ |
| UART0 | TXD0 = GPIO43，RXD0 = GPIO44 |
| 下载 | GPIO0=0 且 GPIO46=0 |
| 外形 | 18.0×25.5×3.1 mm，脚距 1.27 mm |
| 天线 | PCB，keepout 约 6 mm，在 1/40 脚一端 |

## 使用前必看

- 按模组 41 脚铸锡孔画封装，不要用 DevKit 排针图。
- IO 不耐 5 V。EN 不要悬空。
- 底板在天线 keepout 下不要铺铜、不要放连接器或金属壳。
