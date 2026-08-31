# 官方资料入口

核对日期：2026-08-28。只采用 Espressif 官方站点和官方 GitHub。

## 模组与芯片

- [ESP32-S3-WROOM-1 产品页](https://www.espressif.com/en/products/modules/esp32-s3)
- [模组规格书（官方固定地址）](https://www.espressif.com/documentation/esp32-s3-wroom-1_wroom-1u_datasheet_en.pdf) — 本地 [v1.8](../references/chip/ESP32-S3-WROOM-1-WROOM-1U-Datasheet-v1.8.pdf)
- [芯片规格书](https://www.espressif.com/documentation/esp32-s3_datasheet_en.pdf) — 本地 [v2.2](../references/chip/ESP32-S3-Series-Datasheet-v2.2.pdf)
- [TRM](https://www.espressif.com/documentation/esp32-s3_technical_reference_manual_en.pdf) — 本地 [v1.8](../references/chip/ESP32-S3-Technical-Reference-Manual-v1.8.pdf)
- [ESP32-S3 Series SoC Errata](https://docs.espressif.com/projects/esp-chip-errata/en/latest/esp32s3/index.html) — 芯片修订与已知问题
- [ESP32-S3 Hardware Design Guidelines](https://docs.espressif.com/projects/esp-hardware-design-guidelines/en/latest/esp32s3/index.html) — 电源、EN、USB、天线净空；当前是 HTML，不存未钉版本 PDF

固定 URL 以后可能换成更新版本。版本敏感的设计应重新下载并记哈希。

## ESP-IDF 与烧录

- [ESP-IDF ESP32-S3 Get Started](https://docs.espressif.com/projects/esp-idf/en/stable/esp32s3/get-started/index.html)
- [USB Serial/JTAG Console](https://docs.espressif.com/projects/esp-idf/en/stable/esp32s3/api-guides/usb-serial-jtag-console.html)
- [esptool boot mode](https://docs.espressif.com/projects/esptool/en/latest/esp32s3/advanced-topics/boot-mode-selection.html)
- [esp-idf GitHub](https://github.com/espressif/esp-idf)
- [esptool GitHub](https://github.com/espressif/esptool)

## Arduino / MicroPython

- [arduino-esp32 安装](https://docs.espressif.com/projects/arduino-esp32/en/latest/installing.html)
- [arduino-esp32 GitHub](https://github.com/espressif/arduino-esp32)
- [MicroPython ESP32_GENERIC_S3](https://micropython.org/download/ESP32_GENERIC_S3/) — 不要用普通 ESP32 / C3 构建

## 使用原则

1. 模组焊盘、针序、外形以模组规格书 v1.8 为准；
2. strapping、IO 耐压、芯片 EOL 以芯片规格书 v2.2 和勘误为准；
3. 寄存器以 TRM v1.8 为准，日常接线不必先读；
4. SDK 必须钉版本；
5. 第三方 DevKit 引脚图、博客封装不能替代本模组 41 脚。
