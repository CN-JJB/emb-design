# ESP32-C3 Super Mini 官方与开源资料入口

核对日期：2026-08-09。优先采用 Espressif 官方站点、MicroPython 官方站点和官方 GitHub 仓库。

## 芯片与硬件

- [ESP32-C3 产品页](https://www.espressif.com/en/products/socs/esp32-c3)：产品能力和文档入口。
- [ESP32-C3 Series Datasheet（官方最新版地址）](https://documentation.espressif.com/esp32-c3_datasheet_en.pdf)：引脚、电气参数、芯片后缀和启动配置。
- [ESP32-C3 Technical Reference Manual（官方最新版地址）](https://documentation.espressif.com/esp32-c3_technical_reference_manual_en.pdf)：寄存器、外设和底层编程。
- [ESP32-C3 Series SoC Errata](https://docs.espressif.com/projects/esp-chip-errata/en/latest/esp32c3/index.html)：ADC2 等芯片修订问题；设计前必查。
- [ESP32-C3 Hardware Design Guidelines](https://docs.espressif.com/projects/esp-hardware-design-guidelines/en/latest/esp32c3/index.html)：电源、复位、USB、Flash、射频和 PCB 建议。

本目录已保存核对日获取的 [数据手册 v2.4](../references/chip/ESP32-C3-Series-Datasheet-v2.4.pdf) 与 [技术参考手册 v1.4](../references/chip/ESP32-C3-Technical-Reference-Manual-v1.4.pdf)。固定在线地址以后可能更新，版本敏感的设计应重新下载并记录哈希。

## ESP-IDF 与烧录

- [ESP-IDF ESP32-C3 Get Started](https://docs.espressif.com/projects/esp-idf/en/stable/esp32c3/get-started/index.html)：正确的 C3 入门入口。
- [ESP32-C3 USB Serial/JTAG Console](https://docs.espressif.com/projects/esp-idf/en/stable/esp32c3/api-guides/usb-serial-jtag-console.html)：本板原生 USB 串口/调试。
- [esptool ESP32-C3 Boot Mode Selection](https://docs.espressif.com/projects/esptool/en/latest/esp32c3/advanced-topics/boot-mode-selection.html)：GPIO2/GPIO8/GPIO9 下载模式和排障。
- [ESP-IDF GitHub](https://github.com/espressif/esp-idf)：官方 SDK、示例和问题追踪。
- [esptool GitHub](https://github.com/espressif/esptool)：官方烧录与芯片探测工具。

## Arduino

- [Arduino ESP32 安装文档](https://docs.espressif.com/projects/arduino-esp32/en/latest/installing.html)：Boards Manager 安装。
- [arduino-esp32 GitHub](https://github.com/espressif/arduino-esp32)：官方 Arduino Core、板定义与示例。

该 Super Mini 不是 Espressif 官方开发板。没有专用定义时从 `ESP32C3 Dev Module` 起步，Flash Size 必须以实测为准。

## MicroPython

- [MicroPython ESP32 Quick Reference](https://docs.micropython.org/en/latest/esp32/quickref.html)：ESP32 端口用法。
- [MicroPython ESP32_GENERIC_C3 下载页](https://micropython.org/download/ESP32_GENERIC_C3/)：C3 对应固件，不要误用普通 ESP32/S3 构建。
- [MicroPython GitHub](https://github.com/micropython/micropython)：官方源码和发布记录。

## 使用原则

1. 板级网络以本目录 Super Mini 电路图为准；
2. 芯片限制以最新官方数据手册和勘误为准；
3. SDK API 必须匹配项目固定的版本，不使用模糊的“latest”作为构建基线；
4. 同名 Super Mini 可能换芯片后缀、LDO 或 PCB 修订，商品页/博客只能辅助；
5. 引用在线内容时记录访问日期、版本和 URL。
