# 原始资料审计

审计日期：2026-08-28。用户新建了空目录 `ESP32-S3-WROOM-1-N8R2/`，要求按公开官方资料建档。没有商家图、没有开发板资料包。

## 1. 下载的官方 PDF

三个文件均来自 `espressif.com` / `documentation.espressif.com`，文件头为 `%PDF`，封面版本与页脚一致。

| 本地文件 | 版本 | 页数 | 大小 | SHA-256 |
|---|---|---:|---:|---|
| `ESP32-S3-WROOM-1-WROOM-1U-Datasheet-v1.8.pdf` | v1.8（2026-03-02） | 53 | 1,280,501 | `27D71971DA07C280C6068D08C74720D1A25B8F20CF8494DC1765BDD28D40D435` |
| `ESP32-S3-Series-Datasheet-v2.2.pdf` | v2.2（2026-03-05） | 87 | 1,098,115 | `2D5A7CB7FD559D8D972BD88DB32669C0196D23F22D7AFAAFB0F63D099B589A3F` |
| `ESP32-S3-Technical-Reference-Manual-v1.8.pdf` | v1.8 | 1531 | 15,215,232 | `4484BF8A69035EC42A731C58C64ADA6FBD1F1618C5559409F134D9EA083F444F` |

`sites/default/files/...` 与 `documentation.espressif.com/...` 上的模组规格书、芯片规格书二进制相同。

未保存：

| 来源 | 原因 |
|---|---|
| `esp32-s3_hardware_design_guidelines_en.pdf`（sites 路径） | 返回 HTML，不是 PDF |
| HTML “master” 设计指南 PDF | 未钉版本 |
| 第三方 STEP / DevKit 模型 | 不是本模组官方直链 |
| WROOM-1U 尺寸图、1U 框图 | 不是本料号 |

## 2. 从规格书裁出的图

用模组规格书 v1.8 按图号裁切，未改图内尺寸文字。

| 裁切 | 图号 | 归档 |
|---|---|---|
| 封面左侧模组照片 | 封面 | `images/module-top.png` |
| 顶视 41 脚 | Figure 3-1 | `images/pin-layout-top.png` |
| WROOM-1 外形（去掉同页 Figure 10-2 的 1U） | Figure 10-1 | `mechanical/module-18x25.5mm.png` |
| WROOM-1 焊盘 | Figure 11-1 | `mechanical/land-pattern.png` |
| 模组框图（去掉 Figure 2-2 的 1U） | Figure 2-1 | `electrical/module-block.png` |
| 底板典型接法 | Figure 9-1 | `electrical/typical-application.png` |

| 文件 | SHA-256 |
|---|---|
| `images/module-top.png` | `F539C975910930EA4602313945D4B840A691BDBDDB365CCA3CD44DA2A89C4D5C` |
| `images/pin-layout-top.png` | `2E2284641C0F7CED53A015F6419DB2233D78EB076D0F98C22EF87CBE441B84D5` |
| `mechanical/module-18x25.5mm.png` | `7D9BB3D18B4879A0ED5CD2B050A51EB2C0092FD24D321869F45DABC0F9B0C588` |
| `mechanical/land-pattern.png` | `F1B10AE2A621A5DD4CEC7EDE820E7F7F098EDC7DEE548D1A87DF01839F48D202` |
| `electrical/module-block.png` | `4C9E091EC1782A7EC3E0B2D8C05779CED7F12F88F28FC5D6F92796BB242E7084` |
| `electrical/typical-application.png` | `C79CEC3876F32F1804F6C58C41F57AE9698756BC7C3CC067CE0D41501B76D86E` |

## 3. 规格交叉

- Table 1-1 明确有 `ESP32-S3-WROOM-1-N8R2`：8 MB Quad Flash、2 MB Quad PSRAM、−40～85 ℃、18.0×25.5×3.1 mm。
- Table 3-1 note b：只有 Octal PSRAM（R8/R16V）占用 IO35/36/37。N8R2 不占用。
- 芯片手册 Table 1-1：`ESP32-S3R2 (EOL)`，升级为 `ESP32-S3RH2`。这是芯片料号状态，不是把 N8R2 模组从模组表里删掉。
- 典型应用图标题仍写 WROOM-1/1U 通用；N16R16V 的 1.8 V IO47/48 脚注不适用于 N8R2。

## 4. 维护规则

- 官方手册升版：新文件带版本号放入 `references/chip/`，旧版删除，改卡片路径和哈希。
- 不要把 DevKitC、WROOM-1U、N8R8 的图放进本目录。
- 到货后补实物丝印和 `esptool` 记录，用来确认是 S3R2 还是 S3RH2，不改变 N8R2 的 Flash/PSRAM 容量结论。
