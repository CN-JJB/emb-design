# Normalization Plan

## Phase 0 — Governance

先确定顶层架构、证据规则、维护流程。此阶段不继续做大规模资料移动。

本阶段规则文件即 `docs/governance/`。

说明：在这套规则落地前已经产生过一笔试验性规范化提交 `11a4481f22f744e6c27fa7c71be8a050b1613164`。它不作为未来整理模式的默认范例；后续会按新规则逐项复核其中的移动，确认正确才保留。

## Phase 1 — 补齐“迁移缺口”

先补不会改变信息架构的缺失：
- material 中旧仓库遗留的 INDEX/README 文本；
- devices 中仍缺的驱动/例程文本与 EDA 文本；
- 不做批量目录重命名。

目的：先让每个实体的“原始资料集合”完整，再判断哪些该删/移。

## Phase 2 — Devices 逐个规范化

建议顺序：
1. 与私有 KiCad 强相关：INMP441、ICM45686、ESP32-C3、天空星；
2. 传感器/显示：SSD1306、MMC5983MA、ESP32-S3；
3. 电源/驱动：P03B、SKU-17423、TB6612、YB-MPP01；
4. 机械：MG513。

每个 id 经过：
- 文件审计；
- 来源/证据；
- 关键事实复核；
- material 关联；
- EDA binding；
- 验证；
- quality 标记。

## Phase 3 — Material 逐个规范化

优先处理会进入 PCB 的物料：
1. 已有 private footprint 的：SS12D00、TTC encoder、3528 RGB、MX socket；
2. pending footprint 的：USB-C、XH/PH/XA、KF128、轻触、蜂鸣器、保险丝座；
3. 常规 IC/阻容/保护器件；
4. wiring / mechanical / tools。

## Phase 4 — KiCad 绑定审计

目标不是“库里有多少文件”，而是：
- 当前常用实体是否有正确 symbol；
- footprint 是否与手上实物/精确料号匹配；
- 3D 是否正确；
- 来源/许可证/验证是否闭环。

此阶段再决定是否把过渡期 `index/kicad-assets.yaml` 收敛成正式 `kicad/catalog.yaml`。

## Phase 5 — 自动校验

在结构稳定后增加本地校验脚本，至少检查：
- catalog path 存在；
- id 唯一；
- devices_id 关系有效；
- YAML files 引用存在；
- normalized 实体满足必要字段；
- _inbox 不进入 catalog；
- private KiCad ready 资产实体存在；
- ready footprint/model 有验证证据；
- 不再出现 `wait_organize` 之类正式设计会误检的目录。

之后每次本地 AI/CLI 提交前运行校验。

## Phase 6 — 可选路径标准化

只有在所有交叉引用可机器检查后，再考虑把历史中文 device 文件夹统一改为 `devices/<id>/`。

这是独立迁移，不与内容整理混在一起。
