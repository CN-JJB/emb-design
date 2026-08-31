# 第一轮迁移记录 — 2026-08-30

源仓库：`CN-JJB/embbed-projects`  
目标仓库：`CN-JJB/emb-design`

## 已迁移

### devices
- 源目录共 194 个文件，约 171.58 MiB。
- 已迁移全部 73 个 Markdown / YAML 检索文件：
  - `catalog.yaml`
  - 各器件 `device.yaml`
  - `INDEX.md`
  - 硬件、引脚、来源审计、bring-up 等 `docs/*.md`

### material
- 源目录共 244 个文件，约 5.82 MiB。
- 已迁移 105 个核心检索文件：
  - 完整 `catalog.yaml`
  - 全部 YAML 物料卡
  - 根 `README.md`
  - `pending.md`
  - `pending-specs.md`

### KiCad
- 源 `kicad/` 共约 17077 个文件 / 503.37 MiB。
- `libraries/public` 约 368.44 MiB，属于 KiCad 10.0 官方通用库镜像；新仓库不重复镜像，保留 `public/VERSION.md` 作为版本与来源清单。
- 已迁移 `libraries/private` 的 symbol、5 个 footprint、README 与许可证。
- 已迁移库表样例和 thirdparty 说明。

## 尚未实体迁移的二进制

当前 GitHub 连接器无法从私有源仓库跨仓库复制 PDF、PNG/JPG/WebP、ZIP、STEP/STP/STL、XLSX 等二进制。

因此第一轮采取：
1. 先迁移所有用于方案检索的机器可读/文本资料；
2. 旧二进制继续以 `CN-JJB/embbed-projects` 为来源；
3. 对关键 KiCad 3D 模型在 `index/kicad-assets.yaml` 中记录旧路径、SHA、大小与状态；
4. 后续通过本地 git/批量上传等方式补齐二进制时，再把状态改为 `ready`。

## 后续新增规则

以后用户在对话中给出新器件、购买信息或库资源时，优先直接加入本仓库并同步索引。不要只上传孤立文件；必须更新可检索元数据和 KiCad 资源状态。

## 2026-08-30 第二轮：本地二进制迁移完成

按 `docs/local-ai-binary-migration.md` 与 `docs/local-ai-binary-manifest.yaml`，通过本地 Git/CLI 将剩余二进制资产从 `CN-JJB/embbed-projects` 复制到本仓库：

- devices：88 个文件，148,890,336 字节（PDF / PNG / JPG / WebP / ZIP / BIN / STEP / PcbDoc）
- material：24 个文件，5,926,368 字节（PNG / PDF / XLSX）
- kicad/libraries/private/vibecoder.3dshapes：6 个文件，8,794,871 字节（STEP / STP / STL）

复制以源仓库 git blob 为权威字节（`git cat-file blob HEAD:<path>`），逐文件校验：

- 源 blob 的 git blob SHA 与 manifest `git_blob_sha` 一致：118/118
- 源 blob 大小与 manifest `size` 一致：118/118
- 目标文件 SHA-256 与源 blob SHA-256 一致：118/118
- 目标 index blob 与 manifest `git_blob_sha` 一致：118/118

为防 Windows `core.autocrlf` 造成行尾转换破坏二进制字节，新增 `.gitattributes` 将 `*.pdf *.png *.jpg *.jpeg *.webp *.zip *.bin *.step *.stp *.stl *.xlsx *.PcbDoc` 标记为 `-text`。

`index/kicad-assets.yaml` 中 6 个私有 3D 模型状态已由 `source-only` 改为 `ready`，并记录本仓库实际路径（`kicad/libraries/private/vibecoder.3dshapes/...`）。

远端二进制迁移提交：`71fb9bdbcd104761d657bcbce8385105a0b84b8e`（`Migrate remaining device, material, and KiCad binary assets`）


### Web AI 复核

- 复核时间（America/Los_Angeles）：2026-08-30。
- 按 manifest 逐路径比对目标仓库：118/118 文件存在。
- 逐文件 Git blob SHA 与源仓库一致：118/118。
- 6 个私有 KiCad 3D 模型均已存在于目标路径，且 `index/kicad-assets.yaml` 状态均为 `ready`。
- 未发现 LFS 指针替代原文件或字节不一致情况。
