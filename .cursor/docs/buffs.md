# Veloren Buff（增益/减益）说明（基于源码）

本文档面向想要**理解 Buff 系统行为**（回血/回能/减伤/控制/易伤等）以及进行数据排查的读者。内容以服务端逻辑为准，具体数值与表现请以当前仓库源码为最终依据。

- **核心定义**：`common/src/comp/buff.rs`
- **每 tick 处理逻辑**：`common/systems/src/buff.rs`

---

## 1. Buff 是什么

在 Veloren 中，Buff（含 Debuff）是挂在实体（玩家/NPC）上的一个组件 `Buffs`，里面存放多个 `Buff` 实例。每个 `Buff` 会在系统 tick 中生成/执行一组 `BuffEffect`，从而改变：

- **生命/能量/连击**：按时间周期变化（OverTime）
- **上限修正**：最大生命/最大能量变化
- **Stats 修正**：移速、攻速、收招速度、减伤、能量奖励倍率、控制抗性等
- **事件型效果**：攻击附带效果、受伤/死亡触发效果、被攻击的修正器等

---

## 2. 关键数据结构（你在 RON/逻辑里会看到的字段）

### 2.1 `BuffData`：强度与持续相关的数据

源码：`common/src/comp/buff.rs` 的 `BuffData`

- **`strength: f32`**：强度。每个 `BuffKind` 对强度的含义不同（例如回血每秒、回能每秒、减伤强度、减速强度等），具体在 `BuffKind::effects()` 的分支里写死。
- **`duration: Option<Secs>`**：持续时间。`None` 表示**无限期**（但某些类别来源会强制无限期，见下文）。
- **`delay: Option<Secs>`**：延迟生效。Buff 创建时会把 `start_time` 推迟到 `time + delay`，在此之前不会执行效果。
- **`secondary_duration: Option<Secs>`**：第二持续时间（给“附带 Buff”的 Buff 用，例如 Flame/Frigid 这类骑乘效果）。
- **`misc_data`**：杂项数据（例如 `Polymorphed` 可能需要存 Body）。

### 2.2 `Buff`：实际的 Buff 实例

源码：`common/src/comp/buff.rs` 的 `Buff`

- **`kind: BuffKind`**：Buff 类型（回血、燃烧、冰冻等）。
- **`data: BuffData`**：强度、持续时间等。
- **`cat_ids: Vec<BuffCategory>`**：类别标签（用于“来自光环/链接”“死亡是否保留”“攻击后移除”等规则）。
- **`start_time: Time`**：生效起点（已经包含 `delay`）。
- **`end_time: Option<Time>`**：结束时间。若 `None` 则表示无限期（通常来自光环/链接类 Buff）。
- **`effects: Vec<BuffEffect>`**：由 `kind.effects(&data, source_uid)` 构建出来的效果列表。
- **`source: BuffSource`**：来源（角色/世界/命令/物品/其它 Buff/方块等）。

### 2.3 `Buffs`：挂在实体上的 Buff 容器

源码：`common/src/comp/buff.rs` 的 `Buffs`

`Buffs` 内部维护：

- `buffs: SlotMap<BuffKey, Buff>`：所有 Buff 实例
- `kinds: EnumMap<BuffKind, Option<(Vec<BuffKey>, Time)>>`：按 `BuffKind` 分组，并记录该 kind 的“首次添加时间”

此外，它会按“强度、是否有 delay、结束时间”等规则对同 kind 的 Buff 排序（**强的在前**）。

---

## 3. Buff 的生命周期（服务端每 tick 做了什么）

源码：`common/systems/src/buff.rs`

每次 tick，大致流程是：

1. **处理环境触发**：例如接触燃烧实体传播燃烧、站在藤蔓/蛛网上给予 `Ensnared` 等。
2. **检查并标记过期 Buff**：
   - `end_time < now` 的 Buff 过期
   - 来自 `FromActiveAura` / `FromLink` 的 Buff：离开范围或链接失效，会被替换为普通 buff（去掉这些 category）并移除原实例
3. **重置临时 Stats**：`stat.reset_temp_modifiers()`，然后重新应用当前 Buff 对 Stats 的影响。
4. **按 kind 应用效果**：
   - **默认只处理同 kind 中“最强的一个”**
   - 若 `BuffKind::stacks()` 为真，则会处理该 kind 的**所有实例**
   - 若 `buff.start_time > now`（delay 未到），跳过执行
5. **处理体型变化**：若有 `BodyChange`，发事件切换 body
6. **移除过期 Buff**：统一发 `RemoveByKey(expired)` 事件
7. **死亡清理**：死亡时移除不带 `PersistOnDeath` 的 Buff

---

## 4. 叠加、刷新、排队与延迟（最容易误解的部分）

这些规则主要在 `common/src/comp/buff.rs` 的 `BuffKind::{queues, stacks}` 与 `Buffs::insert()` 里：

### 4.1 是否“可叠加”（`stacks()`）

- `BuffKind::stacks()` 为 **true**：同 kind 的多个实例都会同时生效（当前实现里例如 `PotionSickness`、`Resilience`）。
- `stacks()` 为 **false**（默认）：同 kind 只取“最强的一个”执行效果。

### 4.2 是否“排队”（`queues()`）

- `BuffKind::queues()` 为 **true**：同 kind 的多个实例会被“错开”时间，形成队列依次生效（当前实现里是 `Saturation`）。
- 排队通过 `delay_queueable_buffs()` 调整 `start_time/end_time`，避免多个同 kind 同时作用。

### 4.3 “非叠加” Buff 的刷新行为

当插入一个新的 **非 stacks** Buff 时，系统会尝试找到“字段完全相同且时间重叠”的旧实例（比较 `data`、`cat_ids`、`source`，并要求旧的 `end_time` 不早于新的 `start_time`）。

- 找到了：更新旧实例的 `end_time` 与 `effects`（相当于刷新/覆盖）
- 没找到：插入新实例

### 4.4 delay（延迟生效）

`BuffData.delay` 会让 `Buff.start_time` 推迟；在 `start_time` 之前，该 Buff 存在但不会执行任何效果。

---

## 5. `BuffCategory`（类别标签）常见用途

源码：`common/src/comp/buff.rs` 的 `BuffCategory`

常见类别含义：

- **`PersistOnDeath` / `PersistOnDowned`**：死亡/倒地时是否保留
- **`FromActiveAura(Uid, AuraKey)`**：来自某个实体的光环；离开范围会被替换/移除
- **`FromLink(handle)`**：来自链接（例如某种绑定/连接）；链接不存在则会被替换/移除
- **`RemoveOnAttack`**：攻击后移除（具体何时移除取决于其它系统是否发 RemoveByCategory/RemoveByKind）
- **`RemoveOnLoadoutChange`**：换装时移除（同上）
- **`SelfBuff`**：自施 Buff 的标记（常用于 UI/规则判断）

---

## 6. `BuffEffect`（效果类型）速查

源码：`common/src/comp/buff.rs` 的 `BuffEffect`

常见效果类型与直观解释：

- **`HealthChangeOverTime`**：周期性改变生命（正为回血，负为掉血）
- **`EnergyChangeOverTime`**：周期性改变能量（正为回能，负为耗能）
- **`ComboChangeOverTime`**：周期性改变连击
- **`MaxHealthModifier` / `MaxEnergyModifier`**：改变最大生命/能量（加法或乘法，见 `ModifierKind`）
- **`DamageReduction` / `PoiseReduction`**：减伤/减硬直伤害（通常为“护甲结算之后的额外分数”）
- **`MovementSpeed` / `AttackSpeed` / `RecoverySpeed` / `GroundFriction` / `SwimSpeed`**：移动/攻速/收招/摩擦/游泳速度
- **`EnergyReward`**：改变“命中等行为获得能量奖励”的倍率（不是被动回能）
- **`AttackEffect`**：让你的攻击附带额外效果（燃烧、冻结、吸血等通常靠这里串起来）
- **`BuffImmunity(BuffKind)`**：免疫某种 BuffKind
- **`BodyChange(Body)`**：变形
- **`CrowdControlResistance`**：降低控制类 debuff 的持续时间

---

## 7. `BuffKind` 的“strength”到底怎么读

Buff 的强度没有统一单位：**必须看 `BuffKind::effects()` 分支里怎么把 `data.strength` 转成 `BuffEffect`**。

几个常见例子（来自源码注释与构造）：

- **`Regeneration`**：`strength = 每秒回血量`
- **`EnergyRegen`**：`strength = 每秒回能量`
- **`Burning/Bleeding/Poisoned`**：通常 `strength = DPS`（或每秒能量变化量）
- **`IncreaseMaxEnergy`**：注释写的是 “strength 是最大能量变化的 10 倍”（属于历史约定）
- **控制/减速类（如 `Frozen`、`Ensnared`、`Rooted`）**：常见是非线性缩放（源码里有 `nn_scaling`/`nn_scaling2` 这类归一化函数），强度越大越接近上限效果

---

## 8. Debug 与排查建议

当你想回答“为什么这个 Buff 没生效/叠加不对/持续时间奇怪”时，优先检查：

- **是否有 delay**：`start_time > now` 会直接跳过执行
- **是否被覆盖**：非 stacks 的 kind 只取最强实例；插入时可能刷新旧实例而不是新增
- **是否来自光环/链接**：`FromActiveAura`/`FromLink` 会让 `end_time = None`，并且离开范围会被替换/移除
- **是否死亡清理**：没有 `PersistOnDeath` 会被移除
- **是否受到 ItemEffectReduction**：某些物品来源 Buff 会被 `Stats.item_effect_reduction` 降强度或降持续时间

