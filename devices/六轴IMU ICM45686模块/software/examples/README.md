# 多平台移植参考

这里保存从原始完整工程中抽出的传感器相关代码，不包含 MCU SDK、启动文件、IDE 工程文件和构建产物，因此不是开箱即编译的完整工程。

目录内容：

- `arduino/`：商家提供的单文件验证示例，寄存器配置部分需对照官方手册审查。
- `stm32f103/`：I²C、SPI 初始化/读取逻辑及原始端口文件。
- `stm32f407-hal/`：HAL 平台的 I²C/SPI 合并示例及端口文件。
- `mspm0/ccs/`：CCS 的 I²C/SPI 适配和 SysConfig。
- `mspm0/keil/`：Keil 的 I²C/SPI 适配、SysConfig 和生成的引脚配置。

所有平台共用 `../driver/invensense/` 驱动；新建工程时只复制一份公共驱动，不要再次按平台复制。
