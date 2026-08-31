# Maintenance Workflow

## 1. 新增资料：先识别，再归档

收到用户新器件、购买信息、PDF、STEP、商家图或链接时：

1. 搜现有 id / aliases，先去重。
2. 判断属于：
   - Device；
   - Material；
   - KiCad asset；
   - 或尚无法识别。
3. 身份未确认 → 进入 `material/_inbox/` 或对应临时调查区，不进正式 catalog。
4. 身份明确 → 归入实体目录。
5. 从原件提取**当前有证据支持**的事实；未知字段不猜。
6. 更新 machine-readable YAML。
7. 更新 INDEX。
8. 需要上 PCB 时，做 EDA binding。
9. 跑验证。
10. 完成后才把 quality 提升为 `normalized`。

## 2. 新购/到货

用户说“买了/到了/有 X 个”时：
- 先更新 material；
- 若是已有 device，用同 id 链接；
- 数量与库存状态只写 material；
- 不因为买了一个模块就自动复制一份 devices 资料；
- 到货后若实物与历史资料不一致，优先触发 device/material 冲突复核。

## 3. 整理历史实体

**一次只以一个 id 为主任务。**

整理前先列出该目录全部文件，并给每个文件做一个动作：

- KEEP：位置与职责正确；
- MOVE：属于本实体但目录错误；
- REHOME：其实属于另一个实体；
- QUARANTINE：可能有用但身份未闭环；
- DELETE：重复、过时、其它器件、安装器、无规格价值宣传图。

然后依次：
1. 确认 id / variant；
2. 审计所有文件；
3. 归档原件；
4. 修 entity YAML；
5. 建/修 sources/evidence；
6. 修 INDEX；
7. 修 material↔devices 关联；
8. 修 KiCad binding；
9. 运行验证；
10. 更新质量状态；
11. 单独 commit。

不要先批量移动几十个目录再补索引。

## 4. 更新 datasheet / 新 revision

- 新资料先确认 exact match。
- 若新 revision 完全替代旧版，工作树只留新版本；Git history 保留旧版。
- 若旧硬件 revision 仍真实存在且依赖旧手册，则两个版本都保留，并明确 applies_to。
- 不允许“文件名更新了但 YAML 还引用旧文件”。

## 5. 删除资料

删除前确认：
- 是否还有 entity YAML / INDEX / KiCad README 引用；
- 是否是唯一来源；
- 是否仅仅重复或已过时。

删除后必须全库检查引用。

Git history 已提供恢复能力，因此不要为了“怕丢”保留无意义重复副本。

## 6. KiCad 资产新增/修改

新增 private/thirdparty EDA 资产时：
1. 明确来源；
2. 记录许可证；
3. 与精确器件/物料绑定；
4. symbol 核 pin；
5. footprint 核 mechanical；
6. 3D 核 transform；
7. 记录验证范围；
8. 才能标 `ready`。

不得把“能打开”当成“可用”。

## 7. 本地 AI / CLI

Web AI 无法处理的二进制、本地 KiCad/FreeCAD、批量 Git 操作，使用 `docs/local-ai-handoff.md`。

本地 AI 只执行已经确定的结构/manifest，不自行重构信息架构。

本地 AI push 后，Web AI 必须复核 commit 与索引闭环。

## 8. 提交粒度

优先：
- 一个 device 一次规范化提交；
- 一组结构极相同的简单 material 可批量；
- KiCad 资产变更单独提交，便于回退。

不要把“目录大重构 + 电气事实修正 + 库更新 + 删除大量资料”混成一个不可审查提交。
