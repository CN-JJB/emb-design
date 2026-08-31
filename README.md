# emb-design

用于嵌入式硬件方案讨论与器件/库检索的资料仓库。

## 目标

本仓库不是单纯的项目归档，而是面向方案阶段的“可检索硬件知识库”。讨论方案时优先回答：

- 已有器件/模块是否可用、关键电气参数和连接方式是什么；
- KiCad 是否已有可用符号、封装和 3D 模型；
- 资源来自 KiCad 官方库、第三方库、自建库还是器件厂商；
- 若缺少封装/3D 模型/资料，明确标记为待建立或待寻找；
- 保留 datasheet、机械尺寸、引脚图、购买/来源信息，方便复核。

## 目录

```text
devices/        # 器件、模块、开发板及其设计资料
material/       # 已有物料、连接器、线材、工具、采购与库存资料
kicad/          # KiCad 相关库、封装、3D 模型、模板与辅助资料
index/          # 面向检索的统一索引与状态表
docs/           # 维护规范、迁移说明、检索约定
```

## 器件资料建议格式

每个器件目录尽量保留：

- `device.yaml`：机器可读的型号、类别、电气接口、来源、KiCad 资源状态；
- `INDEX.md`：人工快速阅读入口；
- `references/`：datasheet、reference manual、厂商资料；
- `electrical/`：典型连接、电路、引脚；
- `mechanical/`：尺寸、推荐焊盘、安装信息；
- `images/`：器件照片、引脚图；
- `software/`：必要的软件/驱动资料（仅在有价值时保留）。

## KiCad 资源分类

KiCad 资源在检索时区分来源：

1. **official**：KiCad 官方通用库，可引用官方库名/版本，不重复无意义复制；
2. **third-party**：器件厂商、立创/Ultra Librarian/SnapEDA 等第三方资源；
3. **custom**：自行建立或修订的 symbol / footprint / 3D model；
4. **project-derived**：从已有项目中沉淀、仍可复用的库资源。

对每个器件，优先记录：

- symbol：有/无、库名、来源；
- footprint：有/无、库名、封装名称、是否已核对 datasheet；
- 3D model：有/无、路径/来源、STEP/WRL 类型；
- status：ready / verify / missing / custom-needed。

## 来源

第一轮资料主要从原仓库 `CN-JJB/embbed-projects` 的 `devices/`、`material/`、`kicad/` 迁移整理而来。

后续新增器件或资料时，应优先更新对应 YAML/索引，而不是只上传孤立文件。
