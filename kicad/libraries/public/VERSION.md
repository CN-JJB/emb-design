# KiCad 官方库镜像

| 项 | 值 |
|---|---|
| KiCad 版本 | 10.0 |
| 镜像来源 | `C:\Program Files\KiCad\10.0\share\kicad\{symbols,footprints}` |
| 镜像日期 | 2026-08-28 |
| symbols | 223 个 `.kicad_sym` |
| footprints | 155 个 `.pretty`，15447 个 `.kicad_mod` |
| 3dmodels | **未镜像**（3.2 GB，与封装几何无关，仓库放不下） |

## 这是干什么用的

让只能通过 GitHub 看到本仓库的协作 AI（`talk/chatgpt-web` 等）
**看到和本地完全相同的库**，从而能自己验证库键是否存在，而不是猜。

## 怎么更新

```bash
cp -r "/c/Program Files/KiCad/<版本>/share/kicad/symbols"    kicad/libraries/kicad-official/
cp -r "/c/Program Files/KiCad/<版本>/share/kicad/footprints" kicad/libraries/kicad-official/
```

更新后改上表的版本与日期，并在提交信息里写明"重新镜像 KiCad 官方库"。

## 不要在这里改任何文件

这是只读镜像。自建/修改的封装放 `kicad/libraries/footprint/`（已授权）
或 `kicad/libraries/draft/`（未授权）。
