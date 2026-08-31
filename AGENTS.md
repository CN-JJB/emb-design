# emb-design AI 规则

本仓库的顶层治理规则位于 `docs/governance/`。任何方案检索、资料新增、历史整理、KiCad 资产维护，先遵守：

1. `docs/governance/repository-architecture.md`
2. `docs/governance/verification-policy.md`
3. `docs/governance/maintenance-workflow.md`
4. 各域 README 与实体 YAML

## 设计检索顺序

1. 查 `devices/catalog.yaml` 与 `material/catalog.yaml`，确定具体 id / variant。
2. 读对应 `device.yaml` / `item.yaml`，不要从文件夹名或图片自行猜规格。
3. 检查 warnings / pending / quality。
4. 对影响电压、针序、机械、封装的关键事实，读取对应原始证据。
5. 查 KiCad 资产登记与实际 private/thirdparty 文件。
6. 输出方案时明确：
   - 器件/物料 id；
   - 当前库存状态；
   - 关键连接；
   - symbol；
   - footprint；
   - 3D；
   - 来源/验证状态；
   - 未决项。

## 硬规则

- `material/_inbox/`、未来任何 quarantine 区不得作为设计依据。
- “KiCad 官方”只表示来源，不表示 footprint 自动匹配手上实物。
- 私人库“已经存在”只表示有候选，最终仍需 mechanical/pinout 证据。
- 关键来源发生冲突时必须显式标 conflict/verify，不静默选一个。
- 不根据同名商品、相似模块、宣传图猜 pinout/footprint。
- 库存存在不能覆盖技术不适配。
- Git history 是历史保存手段；工作树不保留无意义重复/过时/无关资料。

## 历史资料整改

先完成治理设计，再逐 id 整理。每个 id 先做文件动作表：KEEP / MOVE / REHOME / QUARANTINE / DELETE，然后更新 YAML、证据、INDEX、关联、KiCad，最后验证。

不要在没有逐文件审计前进行全库批量移动或批量重命名。

## 新增资料

新增流程见 `docs/governance/maintenance-workflow.md`。身份未知先隔离；身份明确后再入正式 catalog。

## Web AI ↔ 本地 AI / CLI

当 Web AI 无法完成二进制、本地 KiCad/FreeCAD 或大批量 Git 操作时，按 `docs/local-ai-handoff.md` 生成完整执行 Prompt。

本地 AI 负责执行既定 manifest，不自行决定仓库结构。push 后 Web AI 复核 commit。
