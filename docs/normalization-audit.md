# 现有资料规范化审计

更新：2026-08-30

目标：逐器件、逐物料把“迁移进仓库”提升为“可可靠检索并用于设计决策”。

状态：
- `normalized`：目录职责、主要原件、索引引用已完成一轮审计。
- `review`：已有结构，但仍需逐文件检查来源/重复/归属。
- `pending-local/text`：仍有旧仓库文本/源码待补入后再完成整理。
- `quarantined`：资料存在，但身份未闭环，不可作为设计权威。

## Devices

| id | 状态 | 当前结论 / 下一步 |
|---|---|---|
| ssd1306-096 | review | 结构完整；复核商家图与驱动 ZIP 是否仍需保留 |
| p03b | review | 结构简单；安装孔距仍按 material pending 复核 |
| sku-17423 | review | 复核商家图来源与双路参数 |
| esp32-c3-supermini | review | 原理图/PcbDoc/STEP 已归类；后续核 KiCad 板级封装需求 |
| esp32-c6-supermini | review | 固件 BIN 属本板软件；后续核版本/来源 |
| esp32-s3-wroom-1-n8r2 | review | 官方资料结构较规范；复核版本与官方 KiCad 对应关系 |
| tb6612-d153c | review | 复核模块图、针序和 material 库链接 |
| icm45686 | pending-local/text | 根级 driver/examples 已开始收口到 software；旧仓库 32 个驱动/例程文本待按新路径补入 |
| yb-mpp01 | review | 复核商家资料与 6P 输出警告 |
| stm32f407-skystar | pending-local/text | 宣传图移为 vendor reference；旧仓库 .epro 待补；板级例程保留 |
| mmc5983ma | review | 芯片 DS + 第三方 Arduino 库；模块无原理图，继续保留 3.3 V bring-up 警告 |
| inmp441 | review | 与私有 symbol/footprint/3D 交叉核验 |
| mg513-gmr | review | 电机、编码器、支架/轮尺寸需区分“套件资料”和独立 material 物料 |

## Material

当前核心问题：
- 迁移第一轮只带入 YAML，旧仓库仍有约 115 个 INDEX/README 类 Markdown 未补入新仓库；这些属于可由 Web AI 迁移的文本缺口。
- 历史 `wait_organize/` 不再作为正式资料区；能确认归属的文件迁入正式 item，不能确认的进入 `_inbox/`。

第一批处理：
- `led-3528-rgb`：历史侧视图、推荐焊盘、内部电路图归回正式物料目录。
- `ss12d00`：历史 PDF 归入 datasheets。
- `usb-a-male-smd`：历史 datasheet / mechanical PDF 归回正式目录。
- `tact-3x4-2p`：历史 PDF 归入 datasheets。
- MX 热插拔轴座：身份未闭环，转入 `material/_inbox/mx-hotswap-socket/`，状态 quarantined。

## KiCad

- 官方 KiCad 10.0：按 upstream 使用，不复制整库。
- 私有 symbol / footprint / 3D：当前实体已迁齐；后续每个物料审计时反向核对是否有对应资产、来源和机械验证。
- 私有库“存在”不等于“优先使用”：仍以 datasheet / 实测尺寸 / pin numbering 为最终判据。


## Phase 1 migration-gap status — 2026-08-30

- material historical INDEX/README text: **115/115 restored**.
- devices UTF-8 source/config text: **19/19 restored**.
- remaining non-UTF8/binary-like source files: **14/14 restored by local AI** in commit `25c83ef835b920444965f42640fe8a5dc49c7e6c`.
- Web-side verification: all 14 target paths exist; Git blob SHA and size match the manifest exactly.
- Full Phase 1 closure check: all 118 source material Markdown files are present in the target; all 33 device C/H/INO/SYSCFG/EPRO source files are present, with ICM45686 driver/examples normalized under `software/`.
- **Phase 1 status: CLOSED.**
