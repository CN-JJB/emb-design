# ESP32-C6 Super Mini 原始资料审计

审计日期：2026-08-09。2026-08-27：删除过时 DS/TRM 与测试固件 `.elf`/`.map`；`.bin` 留在 `software/`。图纸分到 `electrical/` `mechanical/` `images/`。

## 1. 范围清理

原始目录曾混入另一块 `ESP32-C6 HXB` 开发板的原理图和 `ESP32C6HXB-TEST` 固件。HXB 使用 WROOM-1 模组、CH340K/CH343P、AMS1117 和双 USB-C，和本目录的裸片 Super Mini 不兼容，已全部移除。

删除前已建立 Git 存档：

- `4bfb39e`：保存 6 个 HXB 文件；
- `9282adb`：从工作目录删除这 6 个文件。

如确需取回，只从 Git 历史恢复，不应重新混入本目录主索引。

## 2. 板级图纸

| 文件 | 解析结论 | 建议用途 |
|---|---|---|
| `schematic/Supermini-C6 原理图.png` | 核心证据；含 C6FH4、USB、充电、LDO、天线、LED、按键和焊盘网络 | 查板级连接时首选 |
| `schematic/Supermini-C6 尺寸图.png` | 正面视图，26.04 mm × 18.00 mm，标出主排针和额外焊盘 | 布板、外壳和接线 |
| `schematic/Supermini-C6 尺寸图2.png` | 背面视图，标出 B+/B- 与背面丝印 | 电池和机械设计 |
| `schematic/引脚图.jpg` | 汇总 GPIO 功能，直观但把 GPIO Matrix 能力画得较宽泛 | 快速浏览；关键限制回查官方手册 |

## 3. 芯片与电源 PDF

| 文件 | 页数/版本 | 状态 |
|---|---|---|
| 商家旧 DS/TRM（英/中 v0.3–v1.0） | 过时 | **已删除** |
| `references/ldo/ME6211A33M3G-N.PDF` | ME6211 V14 | 板载 LDO 系列 |
| `references/official/ESP32-C6-Series-Datasheet-v1.5.pdf` | 86 页，英文 v1.5 | 2026-08-09 获取的官方新版，优先使用 |
| `references/official/ESP32-C6-Technical-Reference-Manual-v1.2.pdf` | 1394 页，英文 v1.2 | 2026-08-09 获取的官方新版，优先使用 |

官方新版 SHA-256：

- 数据手册：`372a5b42b2900c83ef4309149c8835e9ae2d1be19244995bf2fb83af4dc5edf1`
- 技术参考手册：`e08d5b61ebcfa2a78f9735e3f721817cb1dc2df9cf05096d2f0399eaba9f1734`

## 4. 商家测试固件

目录 `esp32.esp32.esp32c6/` 含 6 个 Super Mini 文件。解析确认目标是 ESP32-C6、4 MB Flash，并采用双 OTA 分区。

局限：

- 没有 Arduino 源代码；
- 由 Arduino ESP32 3.0.0 / ESP-IDF 5.1.2 附近的旧工具链构建；
- 包含 Wi-Fi 与 NeoPixel 依赖；
- 二进制中残留固定 SSID/明文密码；
- ELF 和 MAP 很大，只适合诊断，不应视作源码备份。

因此它的定位是“恢复/验板固件”，不是新项目模板。

## 5. 共享 Flash Download Tool 3.9.6

| 文件 | 说明 |
|---|---|
| `flash_download_tool_3.9.6.exe` | Windows GUI 烧录器，约 20.6 MB |
| `doc/Flash_Download_Tool__cn.pdf` | 中文用户指南 v1.9，16 页，2024 |
| `doc/Flash_Download_Tool__en.pdf` | 英文用户指南 v1.7，17 页，2024 |
| `doc/release_note.txt` | 3.9.2-3.9.6 变更记录 |

3.9.4 变更记录明确加入 ESP32-C6 支持；3.9.6 增加 C6 Secure Boot v2 / Flash Encryption 支持。后者涉及不可逆 eFuse，不应在普通开发阶段试用。

该工具支持多个 Espressif 芯片系列，已移到 [共享工具目录](../../../tools/espressif/flash-download-tool-3.9.6/)，避免单板目录混入多芯片通用资料。C6 文档只保留入口。

## 6. 当前资料缺口

- 没有可编辑的 KiCad/Altium 原理图和 PCB 文件；
- 没有 BOM、板厂 Gerber、具体 LDO/充电电流确认和硬件修订号；
- 没有商家测试 `.ino` 源码；
- 没有实物测量记录（空载电流、深睡电流、充电电流、射频性能）；
- 没有该卖家批次的 USB VID/PID、合规和生产测试说明。

后续拿到实物后，建议新增 `tests/board-bring-up.md`，记录照片、芯片丝印、供电电流、GPIO8/9/15 复位电平、USB 枚举和无线测试结果。
