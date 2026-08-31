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
