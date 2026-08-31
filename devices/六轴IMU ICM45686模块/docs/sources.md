# 资料来源与整理范围

## 保留的来源

- [TDK/InvenSense ICM-45686 Datasheet，DS-000577，Revision 1.0](../references/chip/ICM-45686-datasheet-rev1.0.pdf)
  - 芯片功能、电气参数、量程、ODR、接口、寄存器和典型应用电路的权威来源。
- 原网盘中的模块规格书
  - 已转换为硬件、地址和接线说明，不再保留 DOCX。
- 原网盘中的模块原理图
  - 已截取为[模块原理图](../electrical/module-schematic.png)，删除边框、标题栏和错误模板元数据。
- 原网盘中的模块尺寸图
  - 已整理为[模块尺寸图](../mechanical/module-15x18mm.png)。
- 原网盘中的 Arduino、STM32、MSPM0 工程
  - 只保留传感器驱动、传输适配和引脚配置，集中放入 `driver/` 与 `examples/`。

## 已删除的内容

- 七套工程内完全重复的 InvenSense 驱动副本，只保留一份公共驱动。
- MSPM0 Keil 工程内重复携带的完整 TI SDK。
- `Debug`、`Objects`、`OBJ` 等构建输出。
- `.o`、`.d`、`.crf`、`.axf`、`.hex` 等生成文件。
- JLink 日志、IDE 索引、缓存和临时配置。
- 与实际 IMU 工程无关的 TI 示例、库和模板 README。
- 商家性能宣传图以及已被 Markdown 文字完整覆盖的文档图片。
- 原始 DOCX、原理图 PDF、尺寸 PDF和中间 QA 文件。

## 来源优先级

1. TDK/InvenSense 官方数据手册。
2. 模块原理图、尺寸图和实物测量。
3. 商家模块规格书。
4. 随附例程。

例程只用于了解移植方式，不覆盖官方手册中的寄存器定义和时序要求。
