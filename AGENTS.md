# emb-design 检索与维护规则

本仓库用于嵌入式硬件方案阶段的器件、物料与 KiCad 资源检索。

## 方案讨论时的检索顺序

1. 先查 `devices/catalog.yaml`：按型号、aliases、roles、interfaces 找候选器件。
2. 再读候选器件目录下的 `device.yaml` 与 `INDEX.md` / `docs/`，确认电压、接口、引脚、尺寸、使用限制与来源。
3. 查 `material/catalog.yaml`：确认用户现有/已购/在途物料，优先复用已有材料。
4. 查具体 `material/**/item.yaml`：确认数量、封装、连接器系列、待核参数等。
5. 查 `index/kicad-assets.yaml` 与 `kicad/libraries/`：
   - KiCad 官方通用 symbol/footprint：按 KiCad 10.0 官方库验证，不在本仓库重复镜像完整 public 库；
   - 单文件引入与自建资源：优先使用 `kicad/libraries/private/`；
   - 整套第三方库：放 `kicad/libraries/thirdparty/`；
   - 3D 模型若状态为 `source-only`，表示模型仍在旧仓库，需要后续二进制迁移。
6. 输出方案时必须明确给出：器件型号、连接关系、symbol、footprint、3D model 的状态与来源。
7. 不得因为“封装名看起来像”就认定可用。封装必须与 datasheet / 实测机械尺寸核对。
8. 缺失资源明确写 `missing` / `verify` / `custom-needed`，不要猜测补齐。

## 新增器件

新增器件时优先：
- 更新 `devices/catalog.yaml`；
- 新建或更新 `devices/<device>/device.yaml`；
- 保存 datasheet / 引脚 / 机械资料的来源信息；
- 记录 KiCad symbol / footprint / 3D model 状态；
- 若用户已购，同时更新 `material/catalog.yaml` 与对应 `item.yaml`。

## 来源优先级

厂家 datasheet / 官方机械图 > KiCad 官方库 > 已核验第三方库 > 实测/用户提供资料 > 未核验社区资源。

任何自建或修改 footprint / 3D model 都要记录来源和复核结论。
