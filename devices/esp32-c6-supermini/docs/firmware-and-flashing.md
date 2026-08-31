# ESP32-C6 Super Mini 固件与烧录说明

## 1. 现有固件是什么

`software/` 不是 Arduino 工程，而是一组已经编译好的商家测试产物：

| 文件 | 用途 |
|---|---|
| `superminiC6TEST.ino.bootloader.bin` | ESP32-C6 二级 Bootloader |
| `superminiC6TEST.ino.partitions.bin` | 4 MB Flash 分区表 |
| `boot_app0.bin` | OTA 数据分区的初始内容 |
| `superminiC6TEST.ino.bin` | 测试应用 |
| `superminiC6TEST.ino.elf` | 带符号的链接结果，供调试/解析 |
| `superminiC6TEST.ino.map` | 链接映射和依赖清单 |

缺少 `.ino`、`.cpp` 和项目配置，不能把这组文件当作可维护源码。

## 2. 二进制解析结果

使用 Espressif `esptool` 5.3.1 离线解析得到：

- 目标芯片：ESP32-C6（Chip ID 13）；
- 应用大小：842,176 字节；
- Flash：4 MB、80 MHz；
- 镜像头实际模式：DIO；
- 入口地址：`0x40801c28`；
- 应用和 Bootloader 校验和、SHA-256 校验均有效；
- ESP-IDF 基线：`v5.1.2-185-g3662303f31-dirty`；
- Arduino 构建信息字符串：Arduino ESP32 `3.0.0`，板型 `ESP32C6_DEV`，variant `esp32c6`。

FQBN 字符串保留了 `FlashMode=qio`，但最终应用镜像头标记为 DIO。恢复商家固件时应以实际镜像头和成功实测为准，不要仅照抄 FQBN 字符串。

从 MAP 与应用字符串可确认测试程序包含：

- Arduino USB CDC/串口诊断；
- 芯片、内存、Flash、分区和 GPIO 信息打印；
- Wi-Fi 连接/扫描相关代码；
- Adafruit NeoPixel / RMT，用于 GPIO8 的板载 RGB LED。

固件内还残留固定 Wi-Fi SSID 和明文密码字符串。它只适合作为商家验板固件，不应直接用于正式设备或分发；正式项目应重新构建并擦除旧 Flash/NVS。

## 3. 分区表

| 标签 | 类型 | 偏移 | 大小 | 说明 |
|---|---:|---:|---:|---|
| nvs | data/nvs | `0x009000` | `0x005000` | NVS 参数 |
| otadata | data/ota | `0x00E000` | `0x002000` | OTA 状态 |
| app0 | app/ota_0 | `0x010000` | `0x140000` | OTA 应用槽 0 |
| app1 | app/ota_1 | `0x150000` | `0x140000` | OTA 应用槽 1 |
| spiffs | data/spiffs | `0x290000` | `0x160000` | SPIFFS |
| coredump | data/coredump | `0x3F0000` | `0x010000` | 崩溃转储 |

这是 4 MB、双 OTA 分区布局。

## 4. 恢复商家测试固件

只有在确实要恢复原始测试状态时才使用以下地址：

| 地址 | 文件 |
|---:|---|
| `0x0000` | `superminiC6TEST.ino.bootloader.bin` |
| `0x8000` | `superminiC6TEST.ino.partitions.bin` |
| `0xE000` | `boot_app0.bin` |
| `0x10000` | `superminiC6TEST.ino.bin` |

推荐使用 `esptool`，因为命令可记录、可复现：

```powershell
python -m esptool --chip esp32c6 --port COMx erase-flash
python -m esptool --chip esp32c6 --port COMx --baud 460800 write-flash `
  0x0000  superminiC6TEST.ino.bootloader.bin `
  0x8000  superminiC6TEST.ino.partitions.bin `
  0xE000  boot_app0.bin `
  0x10000 superminiC6TEST.ino.bin
```

Windows GUI 也可使用共享目录的 [Flash Download Tool 3.9.6](../../../tools/espressif/flash-download-tool-3.9.6/)。选择 ESP32-C6、正确 COM 口和上述四组文件/地址。它是 Espressif 多芯片通用下载器，因此不放在单板资料目录内；3.9.4 起已支持 ESP32-C6。

## 5. 手动进入下载模式

1. 按住 BOOT；
2. 短按并松开 RST；
3. 松开 BOOT；
4. 重新识别串口/USB 设备后开始烧录。

原理是复位采样时 GPIO8=1、GPIO9=0。若刷写后不运行，松开 BOOT 再按一次 RST。

该板使用 GPIO12/GPIO13 的原生 USB Serial/JTAG，不依赖 CH340。数据线必须是真正的 USB 数据线；只有充电线时设备不会枚举。

## 6. 新项目建议

Arduino：

- 安装官方 `arduino-esp32`；
- 没有专用 Super Mini 板定义时选择 `ESP32C6 Dev Module`；
- Flash Size 设为 4 MB；
- 需要原生串口时启用 USB CDC On Boot；
- 从 DIO/80 MHz 开始验证，再决定是否使用更激进的 Flash 模式；
- 板载 RGB 使用 GPIO8，蓝灯使用 GPIO15。

ESP-IDF：

- 目标设为 `esp32c6`；
- 优先使用当前 stable ESP-IDF，而不是商家固件的旧 v5.1.2 基线；
- 先用 `hello_world`、USB Serial/JTAG console 和 GPIO 示例验证板卡，再加入 Wi-Fi、Thread/Zigbee。

## 7. 不可逆操作

Flash Download Tool 文档包含安全启动和 Flash 加密功能。这些流程会写 eFuse；eFuse 是一次性配置。没有量产密钥方案、备份、验收步骤和废板预案时，不要启用 Secure Boot、Flash Encryption 或任意 eFuse 烧写。

## 8. 常见故障定位

| 现象 | 优先检查 |
|---|---|
| 没有 COM/USB 设备 | 数据线、USB 端口、BOOT+RST 手势、设备管理器 |
| 一直 Connecting | GPIO9 未拉低、GPIO8 被外设拉低、端口被占用 |
| 下载成功但不启动 | 地址错误、分区不匹配、BOOT 未松开、Flash 模式不兼容 |
| USB 刷写后消失 | 应用复用了 GPIO12/13、关闭 USB CDC、程序早期崩溃 |
| RGB 异常或无法下载 | GPIO8 外设冲突或外部电路影响启动采样 |
| 蓝灯影响调试 | GPIO15 同时涉及板载 LED 和 JTAG 启动配置 |

官方操作入口见 [official-sources.md](official-sources.md)。
