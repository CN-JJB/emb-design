# YAHBOOM YB-MPP01 稳压电源板资料索引

> 目录 id：[`yb-mpp01`](../catalog.yaml) · 结构化卡片：[device.yaml](device.yaml)
>
> PCB 丝印 `YAHBOOM YB-MPP01-V1.0`。参数来自商品图，待实物复核。

## 按内容看过之后的结论

| 文件 | 实际是什么 | 归入 |
|---|---|---|
| 功能分布图 | 输入 KF301 / XH2.54 / DC5.5×2.5；输出 Type-C / PH2.0×2 / DC5.5×2.1 / 6P 排针（5 V 出，禁止反灌） | `images/` |
| 尺寸图 | 65×56 mm，高 13.8 mm，4×Ø2.7 mm | `mechanical/` |
| buying.jpg | 购物宣传长图，信息已被功能图覆盖 | **已删除** |

## 优先查阅

- [规格、接口与上电](docs/hardware.md)

## 使用前必看

- 一次只接一路输入。
- Type-C 输出需要支持 PD 的数据线，否则可能无电压。
- 6P 排针是 **5 V 输出**，禁止当输入。
