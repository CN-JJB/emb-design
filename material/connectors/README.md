# 接插件

板载排针排母、XH/PH/XA、螺钉端子、保险丝座、USB、XT。

同一系列（只是针数不同、共用尺寸图）放在**一个**文件夹里，变体写在 `item.yaml` 的 `variants`。
互不通用的型号（例如 XT30 线端 vs PCB 焊板）分开建文件夹。

数量为当前库存估计值，用于判断某个设计够不够用，不代表精确账面。

> **2×N 双排排母已补货**（2026-08-21 确认，数量待录，见 `header-2xn-female`）。
> 无双排排母时的退路仍然成立：用两条 1×20P 单排母并排代替，PCB 焊盘照常按 2×20 画。
> **XH 7P 已补货**（数量待录）——接 D153C 的 H1/H2 可 1:1 同序直连。

## PCB 板载排针 / 排母（2.54 mm）

| id | 名称 | 数量 | 状态 | 待补 |
|---|---|---:|---|---|
| [`header-1x40`](header-1x40/INDEX.md) | 1×40P 直插单排针 | 10 | 现有 | — |
| [`header-2x5`](header-2x5/INDEX.md) | 2×5P 直插双排针 | 20 | 现有 | — |
| [`header-2x6`](header-2x6/INDEX.md) | 2×6P 直插双排针 | 20 | 现有 | — |
| [`socket-1x4`](socket-1x4/INDEX.md) | 1×4P 直插单排母 | 20 | 现有 | — |
| [`socket-1x6`](socket-1x6/INDEX.md) | 1×6P 直插单排母 | 20 | 现有 | — |
| [`socket-1x7`](socket-1x7/INDEX.md) | 1×7P 直插单排母 | 20 | 现有 | — |
| [`socket-1x8`](socket-1x8/INDEX.md) | 1×8P 直插单排母 | 20 | 现有 | — |
| [`socket-1x10`](socket-1x10/INDEX.md) | 1×10P 直插单排母 | 10 | 现有 | — |
| [`socket-1x20`](socket-1x20/INDEX.md) | 1×20P 直插单排母 | 10 | 现有 | — |
| [`header-2xn-female`](header-2xn-female/INDEX.md) | 2×N 双排排母 | 已补货，数量待录 | 已购 | qty |
| [`jumper-254`](jumper-254/INDEX.md) | 2.54 mm 跳线帽 | 100 | 现有 | — |

## XH2.54 接插件

| id | 名称 | 数量 | 状态 | 待补 |
|---|---|---:|---|---|
| [`xh254`](xh254/INDEX.md) | XH2.54 接插件 | 见 variants | 现有 | footprint,qty |

## PH2.0 接插件

| id | 名称 | 数量 | 状态 | 待补 |
|---|---|---:|---|---|
| [`ph20`](ph20/INDEX.md) | PH2.0 接插件 | 见 variants | 现有 | footprint |

## XA2.5 接插件

| id | 名称 | 数量 | 状态 | 待补 |
|---|---|---:|---|---|
| [`xa25`](xa25/INDEX.md) | XA2.5 接插件 | 见 variants | 现有 | footprint |

## 螺钉端子

| id | 名称 | 数量 | 状态 | 待补 |
|---|---|---:|---|---|
| [`kf128-2p`](kf128-2p/INDEX.md) | KF128-2P 螺钉端子 | 5 | 现有 | footprint |

## 保险丝座

| id | 名称 | 数量 | 状态 | 待补 |
|---|---|---:|---|---|
| [`fuse-holder-5x20`](fuse-holder-5x20/INDEX.md) | 5×20 mm PCB 保险丝座 | 10 | 现有 | footprint |

## USB

| id | 名称 | 数量 | 状态 | 待补 |
|---|---|---:|---|---|
| [`usb-a-male-smd`](usb-a-male-smd/INDEX.md) | USB-A 贴片焊板公头 | 商品页 10 个装，数量待录 | 现有 | qty |
| [`usb-c-16p`](usb-c-16p/INDEX.md) | USB-C 16P 插板母座 | 数量待录 | 现有 | footprint,qty |

## XT 电源接插件

| id | 名称 | 数量 | 状态 | 待补 |
|---|---|---:|---|---|
| [`xt30u-m`](xt30u-m/INDEX.md) | XT30U-M 公头（线端） | 5 | 现有 | — |
| [`xt30u-f`](xt30u-f/INDEX.md) | XT30U-F 母头（线端） | 5 | 现有 | — |
| [`xt30upb-m`](xt30upb-m/INDEX.md) | XT30UPB-M 公头（PCB 焊板） | 5 | 现有 | — |
| [`xt30pw-m`](xt30pw-m/INDEX.md) | XT30PW-M 公头（弯脚） | 5 | 现有 | — |
| [`xt60pt-m`](xt60pt-m/INDEX.md) | XT60PT-M 公头（卧式带护套） | 1 | 现有 | — |
| [`xt60pm-m`](xt60pm-m/INDEX.md) | XT60PM-M 公头（PCB 骑板） | 1 | 现有 | — |
