# 原始资料审计与来源

## 1. 整理结论

2026-08-28 按文件内容归档。用户丢进器件根目录的是 telesky 商品图（淘宝随机文件名）和一张商品页截图。看过每一张后再分目录：丝印/针序进 `images/`，尺寸图进 `mechanical/`，芯片手册进 `references/chip/`。

购物文案、FAQ、语音识别海报、已焊/未焊对比图、以及 SHA-256 完全相同的重复 WEBP 已删除，不留档。没有模块原理图，没有抽出驱动包。

## 2. 原始文件对应关系

| 原始名称 | 实际是什么 | 处理 |
|---|---|---|
| `O1CN01yI2da61…webp` | 正面针序说明图 | `images/module-front-pinout.webp` |
| `O1CN017lMSXa…webp` | 正面丝印照片 | `images/module-front-silkscreen.webp` |
| `O1CN01CLhopo…webp` | 背面 MEMS + Ø14 mm / 12 mm | `mechanical/pcb-14mm-back.webp` |
| `Screenshot 2026-08-28 104229.jpg` | 商品合成图：MK025336、14/12/9/1 mm | `mechanical/merchant-overlay-14mm.jpg` |
| `O1CN01BZKWeZ…webp` 与同名 `(1)` | 接口文字 + ESP32 示例；两份 SHA-256 相同 | **已删除** |
| `O1CN01fSQTQF…webp` 与同名 `(1)` | 接线续页 + FAQ；两份 SHA-256 相同 | **已删除** |
| `O1CN01AmhFHJ` `O1CN01Dmmxyh` `O1CN01Z2zj4Q` | 接口定义 / 芯片简介 / 产品特点，文案已写入 docs | **已删除** |
| `O1CN01FU0BET` `O1CN01BmsL561` `O1CN01yHczKZ` | L/R 说明、已焊对比、问答海报 | **已删除** |
| `O1CN01iENwpa` `O1CN01OmQ6bc` `O1CN01T4iyrZ` | 全向/指向麦科普，与本模块无关 | **已删除** |

两张正面图不是重复：一张带彩色引脚说明，一张丝印更清楚。合成尺寸图是 9 mm 高度和 SKU 的唯一来源，尺寸仍标 `measured: false`。

## 3. 芯片手册

归档文件：[INMP441-Datasheet-DS-INMP441-00-Rev1.1.pdf](../references/chip/INMP441-Datasheet-DS-INMP441-00-Rev1.1.pdf)

| 项目 | 结果 |
|---|---|
| 文档 | InvenSense `DS-INMP441-00` Revision 1.1 |
| Rev Date | 2014-05-21 |
| 页数 | 21 |
| PDF | 1.5，未加密 |
| 大小 | 477,735 B |
| SHA-256 | `DDCC8AC55F9FF15E93760B5D01814BCCBEF0CEBBB3857E0FB3954B2B4973D150` |
| 获取 | 2026-08-28 自 TDK 中国产品中心直接下载 |

已打开封面、电气表、绝对最大额定值、引脚、I²S 时序与双麦接法、封装页，内容完整可读。

`product.tdk.com` 同路径 PDF 返回 403；`invensense.tdk.com/wp-content/uploads/2015/02/INMP441.pdf` 返回 HTML 而不是 PDF。可用的官方副本是：

https://product.tdk.cn/system/files/dam/doc/product/sw_piezo/mic/mems-mic/data_sheet/inmp441.pdf

产品页（NRND、底孔、61 dBA / −26 dBFS）https://invensense.tdk.com/products/digital/inmp441/

没有下载第三方博客、Shopify 镜像或随机 Arduino 例程。

## 4. 从商家图能确认 / 不能确认

能确认：

- 圆形板、两排 1×3、丝印六脚名称。
- 商家尺寸标注 Ø14 mm、排距 12 mm、厚 1 mm、带针高 9 mm。
- SKU `MK025336`，已焊排针，水印 telesky，标题「国产版」。

不能确认：

- MEMS 是否为 TDK INMP441（商家图上看不清 `441` 标记）。
- CHIPEN 接到哪里。
- 板上 VDD 去耦是否满足手册的 0.1 µF。
- 声孔在丝印面还是芯片面。
- 排内 2.54 mm、排距 12 mm 的实测值。

因此电压按手册上限保守处理，机械尺寸全部 `measured: false`。

## 5. SHA-256

| 文件 | SHA-256 |
|---|---|
| `images/module-front-pinout.webp` | `D193C42861B9CFFFE38EA5C778AE4C1D6A6BC29A7AA61266172FE3CA9480DAA9` |
| `images/module-front-silkscreen.webp` | `D0058D8F7459F7B0E4AE4FA6792004EF06CDAD5FA28C908ACE6E170EC43BAE44` |
| `mechanical/pcb-14mm-back.webp` | `BFD7E4AA5DDC4BE2EEB1BA9B6C9CBC078565BAFAC920E7AF1B0968C63B5EE3E8` |
| `mechanical/merchant-overlay-14mm.jpg` | `12655B90D20FD2231647A72C20BFC45B2BCFC6689B41A8B03E328AD9066ED0B0` |
| `references/chip/INMP441-Datasheet-DS-INMP441-00-Rev1.1.pdf` | `DDCC8AC55F9FF15E93760B5D01814BCCBEF0CEBBB3857E0FB3954B2B4973D150` |

## 6. 维护规则

- 新增资料前先确认仍是这款圆形双 1×3、丝印六脚的模块；仅同名「INMP441」不够。
- 到货后的实物照片、卡尺尺寸、芯片丝印特写补进 `images/` / `mechanical/`，并改 `device.yaml` 的 `measured` 与 `chip_identity_verified`。
- 再出现混合资料包：只抽出确定属于本模块的文件，其余删除。
- 官方手册若升版，新文件带版本号文件名放入 `references/chip/`，旧版删除，改卡片路径。
