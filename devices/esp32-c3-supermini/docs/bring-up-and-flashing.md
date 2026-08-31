# ESP32-C3 Super Mini 上电、开发与烧录

## 1. 当前目录没有固件

本目录只有硬件、芯片手册和入门资料，没有 `.bin`、Arduino `.ino`、ESP-IDF 工程或 MicroPython 固件。不要从其他芯片目录复制固件；固件必须明确以 `esp32c3` 为目标构建。

## 2. 首次上电检查

建议按以下顺序：

1. 不接外设，用可传数据的 USB-C 线连接电脑；
2. 测量 3.3V 对 GND，确认电压稳定且芯片不过热；
3. 查看设备管理器或串口列表；
4. 如未枚举，按“按住 BOOT -> 点按 RST -> 松开 BOOT”；
5. 使用 `esptool` 读取芯片和 Flash，而不是先假设容量。

```powershell
python -m esptool --chip esp32c3 --port COMx chip-id
python -m esptool --chip esp32c3 --port COMx flash-id
```

记录芯片 revision、Flash 制造商/容量、USB VID/PID 和空载电流。精确芯片后缀仍需查看实物丝印。

## 3. 下载模式

手动进入：

1. 按住 BOOT（GPIO9 拉低）；
2. 短按并松开 RST；
3. 松开 BOOT；
4. 重新选择枚举出的串口后下载。

若一直连接失败，先断开 GPIO2/GPIO8/GPIO9 上的外设。下载模式需要 GPIO2=1、GPIO8=1、GPIO9=0；本板给 GPIO8/GPIO9 上拉，但没有在图中给 GPIO2 外部上拉。

## 4. ESP-IDF

```powershell
idf.py set-target esp32c3
idf.py menuconfig
idf.py build
idf.py -p COMx flash monitor
```

优先运行：

- `get-started/hello_world`：CPU、Flash、串口；
- GPIO 示例：GPIO8 用户灯（低电平点亮）；
- USB Serial/JTAG console：验证原生 USB；
- Wi-Fi scan：验证射频，但不要在 ADC 噪声测试时同时运行。

使用当前 stable ESP-IDF，并在项目中固定版本；目录里的离线“latest”网页不是版本锁定依赖。

## 5. Arduino

- 安装 Espressif 官方 `arduino-esp32`；
- 没有专用 Super Mini 板定义时选择 `ESP32C3 Dev Module`；
- 启用 USB CDC On Boot 以使用原生 USB 串口；
- Flash Size 按 `esptool flash-id` 的探测结果设置，不凭商品标题猜测；
- 板载用户 LED 定义为 GPIO8，逻辑为 `LOW=亮`、`HIGH=灭`；
- 串口调试默认可保留 GPIO21/GPIO20，或使用 USB CDC。

最小用户灯测试：

```cpp
constexpr int LED_PIN = 8;

void setup() {
  pinMode(LED_PIN, OUTPUT);
  digitalWrite(LED_PIN, HIGH);  // 灭
}

void loop() {
  digitalWrite(LED_PIN, LOW);   // 亮
  delay(500);
  digitalWrite(LED_PIN, HIGH);  // 灭
  delay(500);
}
```

由于 GPIO8 是启动配置脚，程序启动后再设输出；不要加会在复位阶段强拉低 GPIO8 的外部电路。

## 6. MicroPython

使用 MicroPython 官方 `ESP32_GENERIC_C3` 构建，不要用普通 ESP32 或 ESP32-S3 固件。下载页可能同时提供不同格式/构建，按页面说明和 Flash 容量选择。

典型擦写流程：

```powershell
python -m esptool --chip esp32c3 --port COMx erase-flash
python -m esptool --chip esp32c3 --port COMx --baud 460800 write-flash 0x0 firmware.bin
```

最终地址和文件格式必须以所下载固件页面的说明为准。

## 7. 常见问题

| 现象 | 优先检查 |
|---|---|
| 安装 CH340 驱动仍无设备 | 本板没有 CH340；检查数据线、原生 USB、BOOT/RST |
| 下载器一直 Connecting | 进入下载模式、端口占用、GPIO2/8/9 外设电平 |
| 下载后 USB 消失 | 应用关闭 USB CDC、复用 GPIO18/19、程序早期崩溃 |
| 上电偶发不启动 | GPIO2 未可靠上拉、GPIO8 LED/外设影响、供电压降 |
| 用户 LED 逻辑相反 | GPIO8 LED 为低电平点亮 |
| ADC2 读数异常 | GPIO5 对应 ADC2；查芯片 revision 和官方勘误 |
| 串口脚认错 | UART0 是 GPIO21 TX/GPIO20 RX；GPIO0/1 是图中 UART1 复用方案 |

## 8. 安全配置

Secure Boot、Flash Encryption 和 eFuse 都不属于首次点亮流程。eFuse 一次写入不可恢复；没有版本固定、密钥管理、备份、生产验收和返修方案时不要启用。

官方入口见 [official-sources.md](official-sources.md)。
