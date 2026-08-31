# Devices 器件库

器件怎么用、针序、尺寸、手册，优先查这里。  
「手上有没有、有几块」查 [`material/`](../material/README.md)（总目录 [`catalog.yaml`](../material/catalog.yaml)）。两边用同一个 `id`。

本文件是 **devices 的操作说明**：根目录有什么、器件目录怎么放、怎么新增、怎么删除、改完要动哪些文件。

## 根目录有什么

```text
devices/
  README.md       ← 本说明 + 已编目清单
  catalog.yaml    ← 总目录（检索第一份必读）
  <器件文件夹>/   ← 一器件一目录，见下方
```

不要在 `devices/` 根下再放散文件（图、PDF、压缩包、临时脚本）。资料一律进对应器件文件夹。不要在根下按类别再建一层（不要 `sensors/`、`power/`）；类别写在卡片的 `category` / `roles` 里。

| 根文件 | 职责 |
|---|---|
| [`catalog.yaml`](catalog.yaml) | 全库过滤用字段：`id`、`aliases`、`roles`、`interfaces`、`path`。没写进这里的文件夹 = 还没编目。 |
| `README.md` | 怎么维护；文末「已编目」表必须与 `catalog.yaml` 同步。 |

## 每个器件文件夹

已编目器件的根上必须有：

| 文件 | 作用 |
|---|---|
| `device.yaml` | 结构化卡片：电压、针序、尺寸、警告、文件指针。没确认的字段不写，禁止按同名模块猜。 |
| `INDEX.md` | 阅读入口：适用范围、看过文件后的结论、指向 `docs/` 与原件。 |

`id` 是主键：小写、芯片或明确型号、无空格。例如 `p03b`、`ssd1306-096`。中文文件夹名可以暂时保留，`catalog.yaml` 的 `path` 指向它。

子目录 **按文件内容** 建，缺的不要空建：

| 子目录 | 放什么 | 不放什么 |
|---|---|---|
| `docs/` | 本库写的中文说明 | 商家原 PDF |
| `images/` | 本器件照片（丝印、正反面） | 结构图、购物海报 |
| `mechanical/` | 板框、孔位、外形图、3D | 原理图 |
| `electrical/` | 本模块原理图 | 芯片手册 |
| `software/` | 推荐驱动、本板例程 | 其它芯片/其它模块的例程包 |
| `references/chip/` | 该芯片官方手册 | 过时副本 |
| `references/panel/` | 面板/玻璃规格 | — |
| `references/vendor/` | **仅针对本器件** 的商家手册 | 混有其它针数/其它板的包 |
| `model/` | STEP 等机械模型 | — |
| `README.md` | 可选：旧链接兼容，只指向 `INDEX.md` | 不要和第二份长文并存 |

样板：[`0.96寸OLED/`](0.96寸OLED/INDEX.md)（`ssd1306-096`）。

## 检索（人与 AI 相同）

1. 读 `catalog.yaml`，按 `id` / `aliases` / `roles` / `interfaces` 选出器件。
2. 打开该目录的 `device.yaml` 和 `INDEX.md`。
3. 需要寄存器、时序、封装细节时，再打开卡片 `files:` 里列出的 PDF。

不要从根目录漫游文件夹名来猜。

## 新增器件

在 `devices/` **根下**新建一个文件夹，不要套进别的器件里，也不要先建分类目录。

1. **定 `id`**  
   查 `catalog.yaml` 有没有同 id。小写、可检索、以后不改。别名（中文商品名、SKU）写进 `aliases`。

2. **建文件夹**  
   根下建目录。名称能让人认出来即可（可暂时用中文商品名）。不要预建空的 `images/` `mechanical/` 等。

3. **看每一份原件再归档**  
   用户丢进来的图/PDF/压缩包，按内容放进上表对应子目录，并改成能看懂的文件名。  
   同一张图：有清晰 PDF 就不留糊截图。  
   混合包：只抽出**确定属于本器件**的文件，然后把混合包和无关内容删掉。  
   没有百分之百把握的网上资料不要下。

4. **写 `device.yaml`**  
   至少：`id`、`name`、`aliases`、`category`、`roles`、`interfaces`、`warnings`、`files`。  
   电压、针序、尺寸只写看过原件后能确认的；不确定就省略或写 `measured: false`。

5. **写 `INDEX.md`**  
   开头标明 id 和卡片链接。写清「适用对象 / 不是什么」。用「按内容看过之后的结论」表说明每个文件是什么、放哪、删了什么。

6. **登记三处（缺一不可）**  
   - `catalog.yaml` 的 `devices:` 追加一条（`path` 用文件夹名，`entry: INDEX.md`）。  
   - 本 README「已编目」表追加一行。  
   - 若 [`material/`](../material/README.md) 已有该型号：库存卡用同一个 `id`，`devices_id` / `devices_entry` 指到本目录 `INDEX.md`。没有库存记录就先不改 material。

7. **收尾**  
   删空文件夹。需要兼容旧路径时，器件根上可留一份只含跳转的 `README.md`。

未做完第 6 步的，不算编目完成；别人和 AI 都不应把它当已入库。

## 删除器件

删除是「连同编目一起拿掉」，不是只删文件夹。

1. 在 `catalog.yaml` 里删掉该 `id` 整条。
2. 在本 README「已编目」表删掉该行。
3. 在 `material/` 对应库存卡里去掉 `devices_id` / `devices_entry`（库存行可以留「有实物」但不链到已删资料）。`catalog.yaml` 同步。
4. 全库搜该文件夹名、`id`、旧 `README.md` 路径（项目原理图、`component_mapping.csv` 等），改掉或标明资料已撤。
5. 删除整个器件文件夹（含子目录）。
6. 不要留下空的父目录或空的 `original/`。

只删器件里的个别无关文件（过时手册、混合包）走下面「维护」，不必走整器件删除。

## 维护已有器件

| 要做的事 | 做法 |
|---|---|
| 多了一张图 / 一份 PDF | 看内容归档到已有子目录；新类目有文件再建模。更新 `INDEX.md` 结论表和 `device.yaml` 的 `files:`。 |
| 发现针序/尺寸写错 | 改 `device.yaml` 和 `INDEX.md`（及 `docs/` 里相关段），两处一致。 |
| 官方手册升版 | 新文件放 `references/chip/`（带版本号文件名），旧版删除；改卡片路径。 |
| 混进别的模块/安装包/视频 | 抽出本器件文件后 **整包删除**。 |
| 同一内容重复（糊图 vs PDF） | 留清晰的，删另一份。 |
| 改文件夹名 | 同步改 `catalog.yaml` 的 `path`、本 README 表、material 链接、以及仓库内其它引用。`id` 尽量不动。 |
| 改 `id` | 同时改：文件夹内 `device.yaml`、`INDEX.md` 开头、`catalog.yaml`、本 README 表、material。避免改 id。 |

改完器件资料后，看一眼上层：`catalog.yaml`、本 README 表、`material/` 是否还指向正确路径（控制仓 [`README.md`](../README.md) 的「文档层级联动」）。

## 已编目

与 [`catalog.yaml`](catalog.yaml) 同步。新增或删除器件时改这一表。

| id | 文件夹 |
|---|---|
| `ssd1306-096` | `0.96寸OLED/` |
| `p03b` | `5V 3.3V开关稳压模块轮趣/` |
| `sku-17423` | `DC-DC降压电源模块直流双路可调电压输出/` |
| `esp32-c3-supermini` | `esp32-c3-supermini/` |
| `esp32-c6-supermini` | `esp32-c6-supermini/` |
| `esp32-s3-wroom-1-n8r2` | `ESP32-S3-WROOM-1-N8R2/` |
| `tb6612-d153c` | `TB6612FNG双路直流电机驱动模块/` |
| `icm45686` | `六轴IMU ICM45686模块/` |
| `yb-mpp01` | `机器人稳压电源拓展板/` |
| `stm32f407-skystar` | `立创·天空星STM32F407VET6开发板-青春版/` |
| `mmc5983ma` | `美新 MMC5983MA模块 三轴磁传感器/` |
| `inmp441` | `INMP441全向麦克风/` |
| `mg513-gmr` | `轮趣MG513电机（GMR编码器）/` |
