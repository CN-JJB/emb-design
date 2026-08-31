# 本地 AI / CLI 任务交接规范

本文件用于 Web AI 无法直接操作本地文件、二进制资源或本地软件时，把任务无损交接给本地 AI。

## 原则

Web AI 负责“确定要做什么、为什么、放哪里、如何验收”；本地 AI 负责“在本地文件系统与 Git 工作区真正执行”。

不要让本地 AI 自行重新设计目录结构，除非交接 Prompt 明确允许。

## 标准执行 Prompt

下面模板可直接复制给本地 AI，并按具体任务填充。

~~~text
你现在在处理 GitHub 仓库 CN-JJB/emb-design 的本地工作区。

目标：
<一句话说明本次任务，例如：把旧仓库中的 KiCad 3D 模型迁移到 emb-design，并更新索引状态。>

源仓库：
CN-JJB/embbed-projects

目标仓库：
CN-JJB/emb-design

开始前：
1. 确认两个仓库工作区均无未提交的意外修改。
2. 在 emb-design 执行 git pull --ff-only。
3. 不要覆盖或回退远端现有提交。
4. 如果发现冲突、目标路径已存在但内容不同、Git LFS/文件大小限制等问题，停止对应文件的写入并报告，不要猜。

需要执行：
<列出明确的源路径 -> 目标路径映射。>

文件处理规则：
- 二进制文件按原始字节复制，不做重新编码。
- STEP/STP/STL/PDF/PNG/JPG/WebP/ZIP/XLSX/BIN 等必须保持内容不变，除非任务明确要求转换。
- 不修改 KiCad symbol/footprint 的电气定义、焊盘编号、3D transform，除非任务明确要求。
- 保留现有 LICENSE / 来源说明。
- 不复制无关项目构建产物、缓存、临时文件。

索引更新：
<说明需要更新的 YAML/Markdown，例如 index/kicad-assets.yaml。>
- 实体文件确认进入目标仓库后，把对应 status 从 source-only 改为 ready。
- 路径字段必须改成新仓库真实路径。
- 不删除 old_path / source / provenance 信息，除非它已被更明确的信息替代。

校验：
1. git status --short
2. 对复制的文件比较源/目标 SHA-256（或等价逐字节校验）。
3. 检查目标文件实际存在且大小合理。
4. 对 KiCad 资源，检查 .kicad_mod / .kicad_sym 文本语法未被意外修改。
5. 若安装了 KiCad CLI，可对相关库/封装做只读解析或渲染检查；没有则跳过并报告。
6. 确认 YAML/Markdown 索引引用的路径真实存在。
7. 确认没有把无关的大型官方库镜像或项目目录一起加入。

提交：
- git add 仅加入本次任务相关文件。
- 建议 commit message：
  <给出本次建议 commit message>
- commit 后 push 到当前主分支，除非任务另有指定。

完成后只向我报告：
1. 实际新增/修改/删除的文件清单；
2. 每个二进制文件的源/目标校验是否一致；
3. 执行过的关键校验及结果；
4. commit SHA；
5. push 是否成功；
6. 尚未解决的问题（没有就写“无”）。

不要只告诉我“完成了”，必须给可复核结果。
~~~

## Web AI 生成交接 Prompt 时必须补全的信息

每次交接时应尽量把以下内容直接写死，减少本地 AI 猜测：

- 精确仓库名；
- 精确源路径与目标路径；
- 文件名；
- 当前已知 SHA / size（若有）；
- 要更新的 YAML 条目；
- 状态从什么改为什么；
- 是否允许重命名；
- 是否允许格式转换；
- 是否需要 KiCad/FreeCAD 检查；
- commit message。

## 完成后的闭环

本地 AI push 后，把 commit SHA 返回给 Web AI。

Web AI 随后：
1. 查看该 commit；
2. 核对文件是否进入正确目录；
3. 核对索引状态；
4. 若通过，则后续方案讨论以新仓库资源为准；
5. 若有问题，再生成一份范围更小的修复 Prompt。
