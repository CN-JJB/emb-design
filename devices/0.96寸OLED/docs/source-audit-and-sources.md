# 原始资料审计、型号纯净性与来源

## 1. 整理结论

当前模块已经按 **0.96 寸 / SSD1306 / 128×64 / 四针 I²C** 收束。2026-08-27 按文件内容重新分目录：照片进 `images/`，结构图进 `mechanical/`，本模块原理图进 `electrical/`，芯片与面板进 `references/`，推荐驱动进 `software/`。

从商家 RAR 只抽出属于这块四针板或该玻璃的文件。其余（六针/七针图与原理图、SSD1306 Rev 1.0、字表、U8glib、Bluno、STM32 SPI、整份混杂手册、RAR 本身）已删除，不留档。

## 2. 原始文件对应关系

| 原始路径/名称 | 整理后位置 | 处理 |
|---|---|---|
| `O1CN01ZCED...2658592015.webp` | `images/module-front-back-pinout.webp` | 内容是正反面照片与丝印，不是结构图 |
| 同名 `(1).webp` | 不再保留工作副本 | 与上一文件 SHA-256 完全相同；原始状态已由 Git 提交 `0b0b0c4` 存档 |
| `QQ20260809-151936.png` | （已删） | 与 B 版结构图 PDF 同一张图，只留 PDF |
| RAR 内 `0.96寸4针B版本结构图.pdf` | `mechanical/pcb-4pin-B-27.3x27.8mm.pdf` | 机械尺寸用这张；针序文字与丝印矛盾 |
| RAR 内四针 I²C 原理图 | `electrical/module-i2c-4pin-schematic.pdf` | J1 为 GND / VCC_IN / SCL / SDA |
| RAR 内 `0.96寸OLED规格书.pdf` | `references/panel/Allvision-QG-2864KLBEG01-VerC.pdf` | 面板规格，不是 PCB 模块说明书 |
| RAR 内使用手册 V2.0 | （已删） | 大半是七针 SPI；四针结论已写入 INDEX / 原理图 / 照片 |
| `70836/资料/YX70836-...rar` | （已删） | 抽出本模块文件后整包删除 |

## 3. 商家 RAR 审计

压缩包共有 679 个条目，约 49.3 MB。2026-08-27 按内容打开了全部 12 份 PDF（含六针/七针图和字表），只把确认属于本四针板或该玻璃的文件抽到正式目录：

| 内部文件 | 审计结果 |
|---|---|
| `0.96寸4针B版本结构图.pdf` | PCB 27.300×27.800 mm。图上排针文字是 `VCC GND SCL SDA`，与丝印矛盾，只作机械尺寸 |
| 四针 I²C 原理图 | J1：GND / VCC_IN / SCL / SDA；662K LDO；默认写地址 `0x78` |
| `0.96寸OLED规格书.pdf` | Allvision `QG-2864KLBEG01` Ver C；玻璃 26.70×19.26 mm；SSD1306 |
| `中景园电子0.96OLED显示屏_驱动芯片手册.pdf` | SSD1306 Rev 1.0，Sep 2007；旧于 Rev 1.1，不抽出 |
| 使用手册 V2.0 | 大半是七针 SPI；已删除 |
| 六针/七针结构图与 SPI 原理图 | 不是本模块；已删除 |
| ASCII / GB2312 字表 | 无关；已删除 |

STM32F103 SPI 工程、STM8 例程、Bluno、旧 U8glib 随 RAR 一并删除。

## 4. 新增本地参考资料

### 4.1 SSD1306 Rev 1.1 数据手册

- 本地文件：[SSD1306-Datasheet-Rev1.1.pdf](../references/chip/SSD1306-Datasheet-Rev1.1.pdf)
- 文档身份：Solomon Systech `SSD1306 Rev 1.1`，Apr 2008，核心数据手册标注 59 页；镜像 PDF 另附 6 页应用说明，共 65 页。
- 获取方式：Solomon Systech 官网产品页用于确认器件身份；官网下载入口当前未直接提供可访问 PDF，因此本地文件取自 Adafruit 的原厂文档镜像。
- 核验：已提取全文，并将全部页面渲染为联系表检查；封面、I²C 地址与控制字节、GDDRAM、命令表、电气参数和封装页均可正常阅读。

商家压缩包中的芯片手册是 Rev 1.0，已随无关内容删除；日常查阅以 Rev 1.1 为准。

### 4.2 Adafruit SSD1306 驱动

- 本地文件：[Adafruit_SSD1306-d94f699.zip](../software/Adafruit_SSD1306-d94f699.zip)
- 版本：2.5.17。
- 固定提交：`d94f699451d72286357cba7259055ffff2c2940b`，2026-05-29。
- 许可证：BSD 3-Clause；许可证文本包含在 ZIP 的 `license.txt`。
- 完整性：ZIP 共 34 个条目，CRC 检查无错误。
- 依赖：`Adafruit GFX Library`，建议通过 Arduino Library Manager 安装依赖。
- 范围说明：该库只面向 SSD1306 控制器，但支持多种 SSD1306 面板尺寸；当前模块必须构造为 128×64、I²C、无外接复位，并显式使用地址 `0x3C`。

## 5. 在线来源

检索与下载日期：2026-08-09。

1. [Solomon Systech SSD1306 官方产品页](https://www.solomon-systech.com/product/ssd1306/) - 器件型号的一手来源。
2. [Adafruit 托管的 Solomon Systech SSD1306 Rev 1.1 PDF](https://cdn-shop.adafruit.com/datasheets/SSD1306.pdf) - 本地原厂文档镜像的下载来源。
3. [Adafruit SSD1306 GitHub 仓库](https://github.com/adafruit/Adafruit_SSD1306) - 开源驱动上游仓库。
4. [固定提交 d94f699](https://github.com/adafruit/Adafruit_SSD1306/commit/d94f699451d72286357cba7259055ffff2c2940b) - 本地 ZIP 对应的精确源码版本。
5. [固定提交源码 ZIP](https://codeload.github.com/adafruit/Adafruit_SSD1306/zip/d94f699451d72286357cba7259055ffff2c2940b) - 本地源码归档下载地址。

淘宝图片、B 版原理图、结构图和面板规格书来自用户提供的本地商家资料。中景园使用手册因混有七针 SPI 已删除，四针结论以丝印照片和原理图为准。没有把无法验证的第三方博客作为规格依据。

## 6. SHA-256 完整性记录

| 文件 | SHA-256 |
|---|---|
| `images/module-front-back-pinout.webp` | `3539E792475DE71248B15DB2050BBC55E78AA4A8A5DDC322D13B88DBC0A5B455` |
| `mechanical/pcb-4pin-B-27.3x27.8mm.pdf` | `E5CF451497BA605DD745D83C4CE87FC8125640B2EEDEB0F7D1806E802446C458` |
| `electrical/module-i2c-4pin-schematic.pdf` | `668638F56E89A252EB6B23DF9D6F3A310BE128FB89B228E46C38402B4AC1AAFF` |
| `references/panel/Allvision-QG-2864KLBEG01-VerC.pdf` | `1938BBF23EEB66E502E7CDA5E07DE028EFFD6375E33C7B7CF7B173045CCB471A` |
| `references/chip/SSD1306-Datasheet-Rev1.1.pdf` | `D55F875357DE96D8C0E92153A389ACC57E8BAB4DB7A0687F2E0BD3362F0036F6` |
| `software/Adafruit_SSD1306-d94f699.zip` | `95E40FAD38C76CE93D9E68E41976302DA823287395FFDA87ECB09B25ED6DB045` |

## 7. 维护规则

- 新增资料前先确认同时满足 SSD1306、128×64、四针 I²C；仅同为“0.96 寸 OLED”不够。
- SH1106、CH1116、SSD1309、128×32、SPI 或并口资料应放到各自设备目录。
- 再出现混合包：抽出属于本四针模块的文件后，把混合包和无关内容删掉，不留档。
- 在线资料保存时记录来源 URL、版本/提交号、下载日期和 SHA-256。
