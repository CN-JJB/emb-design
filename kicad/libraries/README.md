# KiCad 库：公有 / 私有 / 第三方 三分法

> **分类原则（用户 2026-08-29 定）**，封装和 3D 模型一律按这个分：
>
> | 分类 | 定义 | 目录 |
> |---|---|---|
> | **公有库** | KiCad 官方随安装分发的库 | [`public/`](public/) |
> | **私有库** | **单个**引入的东西：从网上下载的、从某个开源项目里挑出来的一两个文件，以及本项目自建的 | [`private/`](private/) |
> | **第三方库** | **整套**引入的库，一次带进来很多元件 | [`thirdparty/`](thirdparty/) |
>
> 判据是**引入方式**，不是来源：从 marbastlib 只挑了一个 `.kicad_mod` → 私有库；
> 若整个 marbastlib 克隆进来 → 第三方库。

## 当前状态

| 目录 | 内容 | 规模 |
|---|---|---|
| [`public/`](public/) | KiCad 10.0 官方符号 + 封装完整镜像 | 223 符号库 / 15447 封装 |
| [`private/`](private/) | 本项目自建 + 单个引入的封装、符号、3D 模型 | 4 封装 / 1 符号 / 3 个 STEP |
| [`thirdparty/`](thirdparty/) | 尚无 | — |
| `footprint/`、`test/` | 更早期的遗留目录 | — |

## 为什么公有库要镜像进仓库

协作的外部 AI（见 [`talk/`](../../talk/)）**只能看到 GitHub 上的仓库**，
本机装了什么它不知道。镜像之后，「这个 `Library:Name` 到底存不存在」它可以自己 grep，
而不是凭印象回答——这正是最容易被带偏的地方。

## 怎么让 KiCad 检索到这些库（一条命令）

```bash
# 先完全退出 KiCad，再跑
python kicad/tools/register_libs_to_kicad.py
```

它做三件事，**幂等，重复跑不会写重复条目**：

1. 在 KiCad 的 `kicad_common.json` 里加环境变量
   `CONTROL_LIB` = 本目录的绝对路径；
2. 在**全局** `fp-lib-table` 登记 `vibecoder`（封装库）；
3. 在**全局** `sym-lib-table` 登记 `vibecoder`（符号库）。

登记之后，在 KiCad 里选封装/符号时直接搜 `vibecoder` 就能搜到，
不需要每个工程单独配。改动前会自动备份成 `*.bak-<日期>`。

`--check` 只检查不写入。

> ⚠ **必须先完全退出 KiCad。** KiCad 退出时会重写 `kicad_common.json`，
> 在它运行期间改会被覆盖。被覆盖了就再跑一次脚本——这也是把它做成脚本而不是手工改的原因。

### 手工配（等价做法）

- **Preferences → Configure Paths** 加一条
  `CONTROL_LIB` = `G:ision_system\control\kicad\libraries`
- **Preferences → Manage Footprint Libraries → 全局** 加一行，
  路径 `${CONTROL_LIB}/private/vibecoder.pretty`
- **Preferences → Manage Symbol Libraries → 全局** 同理，
  路径 `${CONTROL_LIB}/private/vibecoder.kicad_sym`

### 3D 模型路径为什么也用 `CONTROL_LIB`

封装里的 `(model ...)` **不要写 `${KIPRJMOD}`**。
`${KIPRJMOD}` 只有在**打开工程**时才有值——在独立的封装编辑器里打开同一个封装，
路径解析不了，3D 模型就是空白。这是很容易踩的坑。

所以统一写：

```
${CONTROL_LIB}/private/vibecoder.3dshapes/<文件名>
```

工程内、封装编辑器里、3D Viewer 里都能解析。

官方自带的模型继续用官方变量 `${KICAD10_3DMODEL_DIR}`，**不要复制进私有库**。

### 怎么在封装编辑器里看 3D

1. **Footprint Editor** 打开封装
2. `File → Footprint Properties` → 切到 **3D Models** 标签页
3. 右边是实时预览，左边是 `Offset` / `Scale` / `Rotation` 输入框，**改数字预览立刻跟着转**
4. 或者直接 `View → 3D Viewer`（**Alt+3**）看整体

如果预览是空的，多半是路径没解析——回到上面跑一次注册脚本。

### 迁移 / 多机同步

**仓库是唯一真值，KiCad 只是指过来。** 所以：

- 仓库换了位置 → 重跑一次注册脚本，`CONTROL_LIB` 自动指向新位置，
  所有封装和 3D 路径都不用改；
- 换一台机器 → clone 仓库，跑一次注册脚本，完事；
- **不要**把库复制一份到 KiCad 安装目录再手工同步——两份就一定会漂移。
  真要复制（比如给别人一个离线包），复制后必须同时改 `CONTROL_LIB` 指向副本，
  并在本文件记一行"某某副本在哪、谁负责同步"。

## 维护规则

1. **先分类再放。** 新东西进来先问：官方有没有（→ 公有库，直接引用不复制）；
   是单个文件还是整套（→ 私有库 / 第三方库）。
2. **私有库里每个文件都要能说清来历。** 见
   [`private/README.md`](private/README.md) 的来源表：文件、来源、许可证、复核结论，缺一不可。
3. **带许可证的，许可证原文放 [`private/LICENSES/`](private/LICENSES/)**，
   文件名标明是谁的、什么许可证。
4. **公有库镜像是只读的**，不要在里面改任何文件；升级 KiCad 后整体重新镜像，
   并更新 [`public/VERSION.md`](public/VERSION.md)。
5. **引入前必须回读复核**，把上游几何和厂家图/实测值逐项比对，结论写进来源表。
   "社区有就拿来用"不是理由。
6. **3D 模型只服务于 enclosure / collision / actuator 定位**，不是电气真值；
   模型与厂家图冲突时，**以厂家图为准**，并在来源表里写明差异。
