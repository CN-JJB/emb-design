# Repository Architecture

## 1. 目标

本仓库不是“把文件都存进来”的归档盘，而是用于后续嵌入式方案选择的**可验证硬件知识库**。

任何设计问题最终都应能回答：

- 这个器件/模块到底是哪一个具体型号或变体；
- 我是否已经拥有/已购它；
- 关键电气、针序、机械尺寸由什么证据支持；
- KiCad symbol / footprint / 3D 是否存在、来源是什么、验证到了什么程度；
- 如果有冲突或缺资料，哪里还不能定稿。

## 2. 顶层职责

| 路径 | 职责 | 不应该承担的职责 |
|---|---|---|
| `devices/` | 模块、开发板、传感器板“怎么用” | 库存数量、采购批次 |
| `material/` | “手上有什么”：库存、已购、线材、连接器、分立件、工具 | 重复保存已有 devices 模块手册 |
| `kicad/` | EDA 资产及其来源：symbol / footprint / 3D | 把 KiCad 官方完整库镜像进来 |
| `index/` | 跨域检索入口/派生索引 | 成为唯一事实来源 |
| `docs/` | 治理、迁移、审计、维护记录 | 器件具体规格事实 |
| `material/_inbox/` | 未确认身份/归属的临时隔离区 | 设计依据、正式库存 |

## 3. 核心实体

### Device

一块“可作为功能模块使用”的具体硬件，例如开发板、IMU 模块、麦克风模块。

权威机器记录：`devices/<path>/device.yaml`

记录：
- 身份、别名、芯片/型号；
- 电气接口、关键针序、电压；
- 模块级机械尺寸；
- 风险/警告；
- 文件引用；
- 后续应增加的 quality / provenance / EDA binding。

不记录库存数量。

### Material Item

用户实际拥有/购买的物料。

权威机器记录：`material/<category>/<id>/item.yaml`

记录：
- 数量与库存状态；
- 具体封装/规格；
- 采购/实物状态；
- pending 项；
- 若是模块，用 `devices_id` 指到 Device，不复制模块使用资料。

### KiCad Asset

用于 EDA 的 symbol / footprint / 3D。

长期目标：
- KiCad 官方资源只记录 KiCad 版本 + 库键；
- 单文件引入/自建资源保存在 `kicad/libraries/private/`；
- 整套外部库保存在 `kicad/libraries/thirdparty/`；
- 每个私有/第三方资产必须有来源、许可证（若适用）、验证结论；
- 物理器件通过 EDA binding 指向具体资产。

当前 `index/kicad-assets.yaml` 继续作为过渡期资产登记表；后续在专门迁移中决定是否收敛为 `kicad/catalog.yaml`，不要在日常整理中顺手改路径。

### Source / Evidence

文件“存在”不代表它能证明某个事实。

长期每个已规范化实体应有证据登记（可采用 `sources.yaml`），至少记录：

- source id；
- path / URL / repo；
- source kind：manufacturer / vendor / user-photo / user-measurement / community / upstream-eda；
- applies_to：chip / exact-module / package / mechanical / inventory；
- exact_match：true / false / unknown；
- acquired/reviewed date；
- license（适用时）；
- notes。

关键事实应能追溯到一个或多个 source id。

## 4. 单一事实来源

| 信息 | 权威位置 |
|---|---|
| 模块针序、电压、模块尺寸 | `device.yaml` |
| 库存数量、在途、已购 | `item.yaml` |
| 原始证据 | 实体目录中的原件 + evidence registry |
| 私有 KiCad 文件本身 | `kicad/libraries/private/` |
| KiCad 资产来源/状态 | KiCad 资产登记 |
| 人工阅读说明 | `INDEX.md` |
| 快速搜索入口 | `catalog.yaml` / `index/` |

`INDEX.md` 和 catalog 的 summary 可以帮助搜索，但**不得成为唯一保存关键参数的地方**。

长期应减少 catalog 与 README 中重复维护的电气/机械事实；关键值只保留在实体 YAML，索引只保留身份、标签和路径。

## 5. 身份规则

- `id` 是稳定主键，建议小写 ASCII slug。
- 同一个实体在 devices 与 material 间关联时使用同一 id。
- aliases 只用于搜索，不作为主键。
- 当变体改变**针序、封装、机械互换性、关键电气规格**时，必须拆成不同 id。
- 仅颜色、数量、柄高等且不影响 EDA/接口互换性的差异，可放 variants。
- 新目录优先使用 id 作为文件夹名。
- 现有中文/历史目录先 grandfather；只有在专门“路径标准化迁移”中统一改名，禁止日常顺手重命名导致链接漂移。

## 6. 文件分层

### devices

建议目标结构：

```text
devices/<device>/
  device.yaml
  INDEX.md
  sources.yaml          # 有外部资料时
  docs/
  images/               # 实物/识别照片
  electrical/           # 模块原理图、连接图
  mechanical/           # 外形/孔位/板框
  model/                # STEP/STL 等
  software/             # 与该具体器件直接相关
  references/
    chip/                # 芯片厂家资料
    panel/               # 面板/附属器件资料
    vendor/              # 商家/板厂资料，低于厂家/实测
    community/           # 社区资料，必须标来源
```

商家宣传图不得混入 `images/` 充当实物证据；有识别价值才保留到 `references/vendor/`。

### material

```text
material/<category>/<id>/
  item.yaml
  INDEX.md
  sources.yaml          # 有原始规格/图片时
  datasheets/
  images/               # 实物照片
  mechanical/
  docs/
```

模块类 material 原则上只记库存并链接 devices；不要复制 devices 已保存的 datasheet。

## 7. 质量状态

实体的“有文件”与“可用于设计”必须分开。

建议逐步加入：

- `draft`：刚录入，信息不完整；
- `review`：资料已迁入，但尚未逐文件审计；
- `normalized`：身份、文件归属、关键事实、证据、链接已完成一轮审计；
- `conflict`：存在影响设计的来源冲突，未解决前不能作为定稿依据。

`material/_inbox` 中的对象视为 `quarantined`，且不进入 catalog。

## 8. EDA Binding

为了让未来方案选择直接落到 KiCad，每个可上 PCB 的器件/物料最终应有 EDA binding：

- symbol：库、名称、来源、状态；
- footprint：库、名称、来源、状态、验证证据；
- 3D：路径/库、来源、状态、机械验证；
- mount：on-board / off-board / header/socket / not-applicable。

推荐状态：
- `ready`：已完成适用范围内验证；
- `verify`：有候选但关键尺寸/针序仍需核；
- `missing`；
- `custom-needed`；
- `not-applicable`。

**KiCad 官方库不是物理真相来源。** 官方 footprint 也必须与实际 datasheet/实测机械尺寸核对。

## 9. 设计选择算法

后续方案讨论固定按：

1. 搜 `devices/catalog.yaml` 与 `material/catalog.yaml`；
2. 确定具体 id / variant；
3. 读取 entity YAML；
4. 检查 quality / pending / warnings；
5. 读取关键证据；
6. 优先复用现有/已购物料，但只有在规格满足时才复用；
7. 核对 KiCad binding；
8. footprint/model 只有到 `ready` 才可称为“可直接用于定稿”；
9. 输出时明确：器件、库存、接线、symbol、footprint、3D、来源、未决项。

库存存在不能覆盖技术不适配；私人库存在也不能覆盖机械/针序不匹配。

## 10. Git 是历史，不是工作区垃圾桶

旧版本、误放文件、过时资料无需长期留在当前树中“以防万一”；Git history 已经承担历史追踪。

工作树只保留当前仍有：
- 规格证明价值；
- 身份证明价值；
- 机械/电气复核价值；
- 软件复现价值

的文件。

旧 datasheet 只有在特定硬件 revision 仍依赖它时才保留；否则保留当前有效版本并依靠 Git 历史。
