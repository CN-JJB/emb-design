# 官方在线来源与本地归档

在线来源最后核验日期：**2026-08-09**。板级事实优先看立创原理图/引脚图，芯片电气参数和外设行为优先看 ST 当前文档。

## 1. 已归档的当前 ST PDF

| 本地文件 | 文档版本 | SHA-256 |
|---|---|---|
| [STM32F407VE-Datasheet-DS8626-Rev12.pdf](../references/chip/STM32F407VE-Datasheet-DS8626-Rev12.pdf) | DS8626 Rev 12，2026-03，206 页 | `1F41B4FA49A9960215EBB76CD0A75F20241DB82EED60D308345A80463CAEED22` |
| [RM0090-Rev22.pdf](../references/chip/RM0090-Rev22.pdf) | RM0090 Rev 22，2026-05，1741 页 | `F3FF2322EEC21F4E49081F68481B764506EEE81DBA6031BC767E2F8955B27CCA` |
| [ES0182-STM32F405-407-Errata.pdf](../references/chip/ES0182-STM32F405-407-Errata.pdf) | ES0182 Rev 19，2026-07，45 页 | `1D3DA0F99B85013548D17DF07919516BF862F8F38CBD1A61F08E75ABB8E4C54F` |

官方下载入口：

- [STM32F407VE 产品页](https://www.st.com/en/microcontrollers-microprocessors/stm32f407ve.html)
- [DS8626 当前数据手册](https://www.st.com/resource/en/datasheet/stm32f407ve.pdf)
- [RM0090 当前参考手册](https://www.st.com/resource/en/reference_manual/rm0090-stm32f405415-stm32f407417-stm32f427437-and-stm32f429439-advanced-armbased-32bit-mcus-stmicroelectronics.pdf)
- [ES0182 当前勘误表](https://www.st.com/resource/en/errata_sheet/es0182-stm32f405407415417-and-stm32f427437-line-limitations-stmicroelectronics.pdf)

勘误表必须与芯片丝印对应的 silicon revision 一起使用；不能只看“STM32F407”型号名就假定所有勘误都适用。

## 2. 立创官方板级来源

- [青春版项目页](https://lckfb.com/project/detail/lckfb-lspi-skystar-stm32f407vet6-lite?param=baseInfo&collection=1a5d9324392d46b98958a8f223846ab0)：产品身份、青春版资料入口和购买链接。
- [天空星 STM32 开发板介绍](https://wiki.lckfb.com/zh-hans/tkx/tkx-stm32f407vxt6/)：立创开发板技术文档中心的板卡总览。
- [天空星 STM32 模块手册](https://wiki.lckfb.com/zh-hans/tkx/tkx-stm32f407vxt6/module/)：外接模块移植资料；用于补足原始 `模块移植手册网址.txt` 中缺失的网址。
- [嘉立创 EDA 工程打开说明](https://lceda002.feishu.cn/wiki/LCaHwO4YcioEI0kNql6cF5cFnoc)：原始资料自带的在线说明链接。
- [立创商城商品页](https://item.szlcsc.com/23849165.html)：料号 `LCKFB-LSPI-SkyStar-STM32F407VET6-LITE`；部分自动请求可能返回 405，浏览器访问为准。

## 3. 本地板级原始资料

- [引脚分配图](../electrical/pin-assignment.pdf)
- [原理图 2024-01-17](../electrical/schematic-2024-01-17.pdf)
- [嘉立创 EDA 工程](../references/eda/立创梁山派·天空星开发板_2024-01-17.epro)
- [原理图必读](vendor-schematic-note.md)

## 4. 原始旧版芯片文档

商家目录仍保留以下历史版本，便于与旧教程页码对应：

商家第 04 章旧版 DS/RM 已删除，日常查阅用上面的 Rev 12 / Rev 22。

旧版可用于跟随原教程，但进行新设计、检查限制或引用电气参数时，应优先使用本页第 1 节的当前版本。

## 5. 开发与编程官方来源

- [STM32CubeF4 GitHub](https://github.com/STMicroelectronics/STM32CubeF4)：ST 官方 HAL/LL、CMSIS、Middleware 和示例，适合新项目。
- [STM32CubeProgrammer](https://www.st.com/en/development-tools/stm32cubeprog.html)：SWD、UART、USB DFU 等烧录工具。
- [AN2606：系统存储器 Boot 模式](https://www.st.com/resource/en/application_note/an2606-stm32-microcontroller-system-memory-boot-mode-stmicroelectronics.pdf)：确认 STM32F407 系统 Bootloader 支持的接口和引脚。
- [PM0214：Cortex-M4 编程手册](https://www.st.com/resource/en/programming_manual/pm0214-stm32-cortexm4-mcus-and-mpus-programming-manual-stmicroelectronics.pdf)：内核、异常、MPU、FPU 和调试相关说明。

## 6. 资料使用优先级

1. **板子怎么接**：本地原理图、引脚分配图、EDA 工程。
2. **芯片能不能这样用**：ST 当前数据手册、RM0090、ES0182。
3. **Bootloader/内核怎么工作**：AN2606、PM0214。
4. **复现商家教程**：本地 SPL 示例和旧版手册。
5. **新项目架构**：STM32CubeF4 HAL/LL。
6. **宣传图**：仅用于识别套装和 BOM 差异，不覆盖原理图与数据手册。
