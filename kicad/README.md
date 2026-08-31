# KiCad 资源库

这里存放方案阶段需要检索和复用的 KiCad 资源，而不是所有项目工程。

## libraries/

- `public/VERSION.md`：KiCad 官方通用库版本清单。当前来源为 KiCad 10.0；完整官方 symbols/footprints 不再在本仓库重复镜像。
- `private/`：单个引入资源 + 自建资源。当前包含 `vibecoder.kicad_sym`、`vibecoder.pretty/` 与来源/许可证说明。
- `thirdparty/`：整套克隆或导入的第三方库。
- `fp-lib-table.sample` / `sym-lib-table.sample`：库表配置样例。

## 资源状态

统一状态见 `../index/kicad-assets.yaml`：

- `ready`：可直接作为方案候选，但仍应按器件 datasheet 做最终核对；
- `verify`：已有资源但尚需尺寸/针序/版本核验；
- `source-only`：旧仓库有实体文件，本仓库因二进制迁移限制暂未复制；
- `missing`：未找到；
- `custom-needed`：建议自行建立。

## 约定

官方库只记录版本与库键，不修改官方镜像。自建/第三方资源必须保存来源、许可证（若适用）、复核结论。
