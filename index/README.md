# 检索入口

方案探讨时优先使用下列入口：

| 目标 | 首选入口 | 下一步 |
|---|---|---|
| 找已有器件/模块 | `devices/catalog.yaml` | 对应 `device.yaml`、`INDEX.md`、`docs/` |
| 找现有/已购物料 | `material/catalog.yaml` | 对应 `item.yaml` |
| 判断 KiCad symbol / footprint / 3D | `kicad-assets.yaml` | `../kicad/libraries/` |
| 查第一轮迁移边界 | `../docs/migration-2026-08-30.md` | 旧仓库对应路径 |

检索时优先按型号、别名、接口、功能、封装和状态组合匹配，而不是只按文件名。
