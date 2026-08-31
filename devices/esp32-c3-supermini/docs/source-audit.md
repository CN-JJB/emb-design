# ESP32-C3 Super Mini 原始资料审计

审计日期：2026-08-09。2026-08-27：删除商家旧版数据手册/TRM 和未锁版本 HTML；保留官方 v2.4 / v1.4 与 ME6211。电路图、尺寸图、PcbDoc、STEP 按内容分到 `electrical/` `mechanical/` `model/`。

## 1. 范围结论

现有 9 个原始文件均与 ESP32-C3 Super Mini、ESP32-C3 芯片或其板载 ME6211 类 LDO 有关，未发现其他芯片或板型文件，因此没有执行型号清理。

原文件夹名 `schemetic` 拼写错误，已改为 `schematic`，其中 3 个文件内容未改。

## 2. PDF 与离线网页

| 文件 | 版本/规模 | 审计结论 |
|---|---|---|
| `datasheet/esp32-c3_datasheet_en.pdf` | 旧版 v1.2 | **已删除** |
| `datasheet/esp32-c3_technical_reference_manual_en.pdf` | 预发布 v0.6 | **已删除** |
| `references/ldo/ME6211A33M3G-N.PDF` | ME6211 V14 | 板载 LDO 系列 |
| 未锁版本 HTML 快照 | latest 未钉版本 | **已删除** |
| `references/official/ESP32-C3-Series-Datasheet-v2.4.pdf` | 76 页，v2.4 | 2026-08-09 获取的官方新版，优先使用 |
| `references/official/ESP32-C3-Technical-Reference-Manual-v1.4.pdf` | 903 页，v1.4 | 2026-08-09 获取的官方新版，优先使用 |

官方新版 SHA-256：

- 数据手册：`833fc000b4b3c3d39c496fcbd597fed5806956503ce7390b19cc8ae82f19f968`
- 技术参考手册：`90ef825653e7a2657dc1bb294041aace9f9b69da87d775c2f1b5bd11eb317423`

## 3. 已移除的商家链接清单

原始 `官方资料链接.txt` 有可用入口，但不能直接照单执行：

- ESP-IDF 链接写成 `.../esp32/get-started/...`，目标是 ESP32，不是 ESP32-C3；新文档已改为 `.../esp32c3/get-started/...`；
- 推荐安装 CH340 驱动，但板级原理图没有 CH340，USB-C 使用 GPIO18/GPIO19 原生 USB Serial/JTAG；
- 引脚图片链接指向官方 ESP32-C3-DevKitM-1，它不是 Super Mini，只能用于理解芯片功能，不能替代本板电路图；
- MicroPython 总下载页没有固定到 C3 构建，新文档改为 `ESP32_GENERIC_C3` 页面。

为保证单板目录纯净，该 TXT 已在 Git 提交 `5e3b3bf` 中存档，并由提交 `68e763d` 从工作目录移除。正确入口集中在 [official-sources.md](official-sources.md)。

## 4. 电路图与尺寸图

| 文件 | 解析结论 |
|---|---|
| `schematic/电路图.png` | 核心板级证据：原生 USB、ME6211 类 LDO、两排 1×8、GPIO8 用户灯、GPIO9 BOOT、40 MHz 晶振和 PCB 天线 |
| `schematic/ESP32-C3尺寸图.png` | 外形 18.00 mm × 22.50 mm、排针跨距 15.24 mm；图中的芯片字样只是封装占位，不是完整料号 |

原理图未标硬件修订号、完整 BOM 和芯片后缀。它适合开发接线，不足以直接用于量产采购。

## 5. Altium PcbDoc

`schematic/Super Mini-ESP32C3-外形.PcbDoc` 是 Altium `PCB 6.0 Binary File`（OLE Compound File）。只读解析得到：

- 内部源路径：`C:\Onion3\SuperMini-ESP32-C3\Hardware\...`；
- 记录时间：2023-04-20 23:31:08；
- 主要组件仅两个 `1X8 PICO HEADER`；
- `Models/Data` 为空，没有嵌入三维模型；
- 文本含 `ESP32-C3`、`Super Mini`；
- 网络表虽保留 VBUS、GPIO、USB、SPI 等名称，但它不是一套完整的板级可生产工程。

因此应把它视为外形、排针与机械对位参考，不能当成完整 PCB 源码、Gerber 或 BOM。

## 6. STEP 模型

`model/Super Mini-ESP32C3.step`：

- ISO 10303-21 / AP214；
- Open CASCADE 6.8 导出；
- 文件头时间 2025-06-11 11:30:20；
- 产品根节点名 `PCB`；
- 约 28.6 MB，适合外壳、连接器和装配干涉检查。

STEP 是机械模型，不包含电气网络或可直接生产的 PCB 信息。

## 7. 当前资料缺口

- 精确芯片后缀与 Flash 容量；
- 完整可编辑原理图、完整 PCB、BOM、Gerber 和硬件修订号；
- 可验证的 Arduino/ESP-IDF/MicroPython 示例工程；
- 空载/射频/深睡电流、USB VID/PID 和实际 ADC2 revision 测试；
- LDO 完整料号、最大持续电流和 USB ESD 方案。

拿到实物后建议新增 `tests/board-bring-up.md`，记录芯片丝印、`esptool` 探测结果、3.3 V、电流、LED、BOOT/RST、USB 和 Wi-Fi/BLE 测试。
