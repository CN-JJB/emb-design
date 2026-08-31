# ICM-45686 公共驱动

`invensense/` 来自资料包随附的 InvenSense 驱动。原始七套 MCU 工程中的这 8 个文件内容完全相同，本目录只保留一份。

平台移植需要给 `inv_imu_device_t.transport` 提供寄存器读、寄存器写、微秒延时和接口类型。板级实现可参考 `../examples/`。

驱动源码保留原有版权和许可头。
