# 上电、下载与烧录

本目录没有现成 `.bin` 或工程。固件必须按 `esp32s3` 目标构建，Flash 8 MB、PSRAM 2 MB Quad。

## 1. 首次上电

模组本身没有 USB 插座、没有 BOOT/EN 键。这些都要做在底板上。

1. 3.3 V 电源能力 ≥ 0.5 A，先不接天线以外的重负载。
2. EN 经上拉到 3.3 V，不要悬空。
3. 测 3V3 对 GND，确认模组不发烫。
4. 用 `esptool` 认芯片和 Flash，不要凭丝印以外的商品名猜容量。

```powershell
python -m esptool --chip esp32s3 --port COMx chip-id
python -m esptool --chip esp32s3 --port COMx flash-id
```

预期：芯片为 ESP32-S3；Flash 约 8 MB。记下 revision。封装内 PSRAM 是 Quad 2 MB，对应 N8R2 / S3R2 / S3RH2，**不是** Octal 的 R8。

## 2. 进入下载模式

芯片手册 Table 3-3：

| 模式 | GPIO0 | GPIO46 |
|---|---|---|
| SPI Boot（默认） | 1 | 任意 |
| Joint Download Boot | 0 | **0** |

GPIO0 默认内部上拉，GPIO46 默认内部下拉。因此底板典型做法：

1. 把 GPIO0 拉低（BOOT 键对地）；
2. 把 EN 拉低再放开（复位）；
3. 松开 GPIO0。

**GPIO46 在下载时必须为 0。** 若外设把它拉高，即使 GPIO0 为低也不会进 Joint Download。

Joint Download 支持：

- USB Serial/JTAG（GPIO19/20）；
- USB-OTG；
- UART0（TXD0=GPIO43，RXD0=GPIO44）。

USB 下载需要能传数据的线和主机驱动。只有充电功能的线不行。

## 3. ESP-IDF

```powershell
idf.py set-target esp32s3
idf.py menuconfig
idf.py build
idf.py -p COMx flash monitor
```

`menuconfig` 里 Flash 选 8 MB，PSRAM 选 2 MB Quad。不要选 Octal PSRAM。

建议先跑 `get-started/hello_world`，再测 Wi-Fi scan。USB Serial/JTAG 控制台见 ESP-IDF `usb-serial-jtag-console`。项目里固定 IDF 版本，不要用“latest”当基线。

## 4. Arduino

- 官方 `arduino-esp32`；
- 板型用 `ESP32S3 Dev Module`（这是芯片目标，不是说手上是 DevKit 板）；
- Flash Size 8 MB；PSRAM **QSPI 2MB**，不要选 OPI；
- USB CDC On Boot 按是否走 GPIO19/20 原生 USB 决定；
- UART0 引脚是 GPIO43/GPIO44，不是 DevKit 丝印上的别的数字。

## 5. 常见失败

| 现象 | 先查 |
|---|---|
| 上电无反应 | 3.3 V 是否到脚 2；EN 是否被拉高 |
| USB 不枚举 | 是否数据线；GPIO19/20 有没有接反；下载时 GPIO0/GPIO46 |
| 能串口不能 USB | 固件是否把 USB 脚改成普通 GPIO |
| Wi-Fi 差 / 认证不过 | 天线 keepout 是否被铺铜或被壳挡住 |
| GPIO35–37 无输出 | 固件是否按 Octal PSRAM（N8R8）编译，错误占用了这三脚 |
