# ESP32-C6 Super Mini 官方与开源资料入口

核对日期：2026-08-09。下列资料优先选择 Espressif 官方站点和官方 GitHub 仓库。

## 芯片与硬件

- [ESP32-C6 产品页](https://www.espressif.com/en/products/socs/esp32-c6)：产品定位、无线能力和入口汇总。
- [ESP32-C6 Series Datasheet（官方最新版地址）](https://documentation.espressif.com/esp32-c6_datasheet_en.pdf)：引脚、电气参数、启动配置和封装。
- [ESP32-C6 Technical Reference Manual（官方最新版地址）](https://documentation.espressif.com/esp32-c6_technical_reference_manual_en.pdf)：寄存器、外设和底层编程。
- [ESP32-C6 Series SoC Errata](https://docs.espressif.com/projects/esp-chip-errata/en/latest/esp32c6/index.html)：芯片版本限制和规避措施；设计与量产前必查。
- [ESP32-C6 Hardware Design Guidelines](https://docs.espressif.com/projects/esp-hardware-design-guidelines/en/latest/esp32c6/index.html)：电源、复位、Flash、USB、射频和 PCB 设计建议。

本目录已离线保存核对日获取的 [数据手册 v1.5](../references/chip/ESP32-C6-Series-Datasheet-v1.5.pdf) 与 [技术参考手册 v1.2](../references/chip/ESP32-C6-Technical-Reference-Manual-v1.2.pdf)。在线固定地址可能在未来更新，版本敏感的设计应重新下载并记录哈希。

## ESP-IDF 与下载

- [ESP-IDF ESP32-C6 Get Started](https://docs.espressif.com/projects/esp-idf/en/stable/esp32c6/get-started/index.html)：官方开发环境和首个工程。
- [USB Serial/JTAG Console](https://docs.espressif.com/projects/esp-idf/en/stable/esp32c6/api-guides/usb-serial-jtag-console.html)：Super Mini 原生 USB 控制台与调试。
- [esptool ESP32-C6 Boot Mode Selection](https://docs.espressif.com/projects/esptool/en/latest/esp32c6/advanced-topics/boot-mode-selection.html)：GPIO8/GPIO9 下载模式与排障。
- [ESP-IDF GitHub](https://github.com/espressif/esp-idf)：官方开源 SDK、示例和问题追踪。

## Arduino

- [Arduino ESP32 安装文档](https://docs.espressif.com/projects/arduino-esp32/en/latest/installing.html)：Boards Manager 安装方式。
- [arduino-esp32 GitHub](https://github.com/espressif/arduino-esp32)：官方 Arduino Core、板定义、库和示例。

该 Super Mini 并非 Espressif 官方开发板。没有专用板定义时通常以 `ESP32C6 Dev Module`、4 MB Flash 和原生 USB CDC 配置起步，再按本目录原理图定义 GPIO8/GPIO15。

## 802.15.4、Zigbee 与 Thread

- [ESP Zigbee SDK Programming Guide - ESP32-C6](https://docs.espressif.com/projects/esp-zigbee-sdk/en/latest/esp32c6/index.html)：官方 Zigbee SDK。
- [ESP Thread Border Router SDK](https://docs.espressif.com/projects/esp-thread-br/en/latest/)：Thread Border Router 方案与示例。

## 采用原则

1. 板级网络以本目录 Super Mini 原理图为准；
2. 芯片电气和复位约束以最新官方数据手册/勘误为准；
3. API 和示例以所选 SDK 版本对应的文档为准；
4. 博客、商品页和其他 Super Mini 仓库只能作辅助，因为同名小板可能更换 LDO、充电 IC、天线或丝印；
5. 引用在线内容时记录访问日期、版本和 URL。
