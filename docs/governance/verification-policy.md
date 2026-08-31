# Verification Policy

## 1. 不使用单一全局“来源排名”

不同事实的权威来源不同。判断时必须先问：**这个来源到底能证明什么范围的事实？**

## 2. 按事实类型选择权威

### 芯片级电气、寄存器、绝对最大值

优先：
1. 芯片厂家 datasheet / reference manual / errata；
2. 厂家 application note；
3. 已核验第三方说明；
4. 社区资料。

模块商家宣传页不得覆盖芯片厂家电气限制。

### 模块级针序、板级连接

优先：
1. 与手上**完全相同 revision** 的模块官方 schematic / drawing；
2. 手上实物的清晰丝印、连通性测试、实测；
3. 精确匹配的厂家/板厂说明；
4. 商家图；
5. 相似模块/社区图。

若商家图与实物丝印冲突，先标 `conflict`，不得静默选一个。

### 机械尺寸、封装

优先：
1. 精确料号 manufacturer mechanical drawing；
2. 手上实物的可靠实测；
3. 已确认同 revision 的板厂/模块机械图；
4. 已核验第三方 EDA；
5. KiCad 官方候选。

KiCad 官方 footprint **从不因为“官方”就自动成为尺寸权威**。

### 库存

唯一权威是用户实际拥有/采购信息及其记录。商品页“10 个装”不等于库存就是 10。

## 3. exact match

任何外部资料都应判断它对当前对象是否：

- `true`：明确同一精确型号/revision/封装；
- `false`：只用于参考，不能证明当前对象；
- `unknown`：尚未确认。

`unknown` 资料可以留作调查证据，但不得支撑 `ready` 状态。

## 4. 冲突处理

遇到冲突：
1. 保留双方来源；
2. 在 entity YAML 的 warnings/open issues 中写明；
3. 只冻结受影响字段，不必让整件资料不可用；
4. 若冲突影响针序、电压、footprint、安装孔等安全/机械关键项，EDA 状态不得为 `ready`；
5. 通过实测/更高权威来源闭环后，记录采用哪个来源及为什么。

## 5. KiCad ready 的最低标准

### Symbol ready

至少：
- pin number/name 与当前实体一致；
- 电源/输入/输出类型不会导致明显 ERC 误导；
- symbol 对应正确 variant；
- 私有/第三方 symbol 有来源。

### Footprint ready

至少：
- pad number 与 symbol 一一对应；
- pitch、pad、孔径、body/courtyard 与精确 mechanical evidence 核过；
- 方向/Pin 1 有可验证依据；
- 若是手焊特殊结构，装配方式已写明；
- 第三方/私有 footprint 有来源和验证记录。

### 3D ready

至少：
- 对应正确外形 variant；
- 位置、旋转、Z 高度经过渲染/机械核对；
- 3D 只作为碰撞/外壳参考，不反向决定电气 pinout；
- 来源明确。

## 6. “已验证”必须带范围

不要只写 `verified: true`。

应能说明验证了什么，例如：
- pinout verified by silkscreen + continuity；
- footprint verified against drawing rev C；
- 3D transform verified by KiCad render；
- voltage only verified at 3.3 V bring-up。

没有验证到的范围继续保留 unknown/verify。
