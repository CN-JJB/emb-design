# 待补资料

**本表是「不许猜封装 / 不许装不知道的数」清单。** 表内对象在拿到实物尺寸图或实测数据之前，
**原理图可以画（符号不依赖尺寸），PCB 封装不可定稿。**

来源是各物料 `item.yaml` 的 `pending:` 字段。闭环后：把关键尺寸写进该物料卡片，删掉对应 pending，并改本表。

## 仍阻塞 PCB 封装

| id | 对象 | 缺什么 |
|---|---|---|
| [`tact-6x6`](pcb/tact-6x6/INDEX.md) | 6×6 轻触开关 | 底面尺寸 / 脚距，封装不可定稿 |
| [`tmb12a05`](pcb/tmb12a05/INDEX.md) | TMB12A05 有源蜂鸣器 | 底面尺寸 / 脚距，封装不可定稿 |
| [`xh254`](connectors/xh254/INDEX.md) | XH2.54 接插件 | 底面尺寸 / 脚距，封装不可定稿；数量待录 |
| [`ph20`](connectors/ph20/INDEX.md) | PH2.0 接插件 | 底面尺寸 / 脚距，封装不可定稿 |
| [`xa25`](connectors/xa25/INDEX.md) | XA2.5 接插件 | 底面尺寸 / 脚距，封装不可定稿 |
| [`kf128-2p`](connectors/kf128-2p/INDEX.md) | KF128-2P 螺钉端子 | 底面尺寸 / 脚距，封装不可定稿 |
| [`fuse-holder-5x20`](connectors/fuse-holder-5x20/INDEX.md) | 5×20 mm PCB 保险丝座 | 底面尺寸 / 脚距，封装不可定稿 |
| [`p03b`](modules/p03b/INDEX.md) | 轮趣 P03B 5 V / 3.3 V 稳压模块 | 安装孔距实物复核（宣传图约 0.165 mm 差异） |
| [`usb-c-16p`](connectors/usb-c-16p/INDEX.md) | USB-C 16P 插板母座 | 16P 插板尺寸图未到，封装不可定稿；数量待录 |

## 不阻塞封装，但还没记完

| id | 对象 | 缺什么 |
|---|---|---|
| [`pptc-1812-0a5`](pcb/pptc-1812-0a5/INDEX.md) | PPTC 0.5 A / 30 V 1812 | 商品页 Vmax / 保持电流 / 动作电流 |
| [`pptc-1812-1a1`](pcb/pptc-1812-1a1/INDEX.md) | PPTC 1.1 A / 33 V 1812 | 商品页 Vmax / 保持电流 / 动作电流 |
| [`header-2xn-female`](connectors/header-2xn-female/INDEX.md) | 2×N 双排排母 | 数量待录 |
| [`heatshrink`](wiring/heatshrink/INDEX.md) | 彩色加厚热缩管 | 数量待录 |
| [`daplink`](tools/daplink/INDEX.md) | DAPLink 仿真器 | 固件与协议支持 |
| [`ch340c`](tools/ch340c/INDEX.md) | CH340C USB 转 TTL | 接口电平与引脚定义 |
| [`led-3528-rgb`](pcb/led-3528-rgb/INDEX.md) | 3528 RGB 雾状共阳 | 数量待录 |
| [`tact-3x4-2p`](pcb/tact-3x4-2p/INDEX.md) | 3×4 小龟轻触（贴片 2 脚） | 份数待录 |
| [`ttc-5p5`](pcb/ttc-5p5/INDEX.md) | TTC 5.5 mm 鼠标编码器 | 数量待录；安装高度 H 未核 |
| [`usb-a-male-smd`](connectors/usb-a-male-smd/INDEX.md) | USB-A 贴片焊板公头 | 数量待录 |
| [`lipo-523450-1000`](modules/lipo-523450-1000/INDEX.md) | 523450 1000 mAh 锂聚合物 | 数量待录；引出插头未核 |

## 已从本表拿掉（资料在 devices）

这些曾经写在旧 `pending-specs.md` 里，器件档案整理后机械/针序已经有主记录，不再当作物料库的封装阻塞项：

- `stm32f407-skystar`：P1/P2 间距 **35.56 mm**（不是 33.02 mm），见 devices
- `esp32-c3-supermini` / `esp32-c6-supermini`：两排针中心距 15.24 mm，见 devices
- `icm45686`：板 15×18 mm、针序已闭环
- `mmc5983ma`：15×10 mm 四针 VIN/GND/SDA/SCL
- `tb6612-d153c`：针序已对照实物；离板走线束，无需本板封装

本表清空之前，涉及表内机械件的 PCB 不能进入投板环节。
