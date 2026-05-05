# Veloren 玩家属性说明（基于源码）

本文档根据当前仓库内 `common/src/comp` 等处的实现，概括**玩家角色**在服务端/客户端逻辑中涉及的主要数值与修正项，便于对照代码或排查问题。游戏内显示名称可能经本地化，与下列字段名不完全一致。

---

## 1. 角色由哪些组件拼成

玩家进入世界后，除 `Body`（种族/体型）、`Inventory`、`SkillSet`、`ActiveAbilities` 等外，与「属性条」直接相关的核心组件包括：

| 组件 | 源码位置（约） | 作用简述 |
|------|----------------|----------|
| `Health` | `common/src/comp/health.rs` | 生命：当前值、上限、死亡、`last_change`（最后一次血量变化，用于仇恨/助攻等） |
| `Energy` | `common/src/comp/energy.rs` | 体力：施法/能力消耗、上限与再生 |
| `Poise` | `common/src/comp/poise.rs` | 硬直/韧性：受击积累，决定 Normal / Stunned / KnockedDown 等状态 |
| `Stats` | `common/src/comp/stats.rs` | **战斗与移动相关的修正与附加效果**（多数为倍率或叠加修正） |

此外还有 `Buffs`、`Combo`、`Stance` 等会间接改变上述数值或行为，本文从略。

---

## 2. `Stats`：战斗与移动修正（不是「力量敏捷」那种 RPG 六维）

`Stats` 是挂在实体上的一组**可叠加的修正器与效果列表**，由装备、Buff、能力等共同写入；基础血量/体力上限等通常先来自 `Body`，再乘上这里的修正。

### 2.1 通用修正结构

- **`StatsModifier`**（`add_mod` / `mult_mod`）  
  用于 **最大生命**、**最大体力** 等：`最终 ≈ 基础 * mult_mod + add_mod`（见 `compute_maximum`）。

- **`StatsSplit`**（`pos_mod` / `neg_mod`）  
  用于 **伤害减免**、**硬直减免** 等正负分项，合成时用 `pos_mod + neg_mod`（见 `modifier()`）。

### 2.2 `Stats` 各字段含义

| 字段 | 典型含义（游戏中） |
|------|---------------------|
| `name` | 角色显示名（本地化内容） |
| `original_body` | 创建时的身体模板，用于性别等逻辑 |
| `damage_reduction` | 所受物理/元素等伤害的减免（分项叠加） |
| `poise_reduction` | 所受硬直伤害的减免 |
| `max_health_modifiers` | 最大生命上限修正（`StatsModifier`） |
| `max_energy_modifiers` | 最大体力上限修正（`StatsModifier`） |
| `move_speed_modifier` | 移动速度倍率（默认 `1.0`） |
| `jump_modifier` | 跳跃倍率 |
| `attack_speed_modifier` | 攻击动作速度倍率 |
| `recovery_speed_modifier` | 收招/恢复类速度倍率 |
| `friction_modifier` | 地面摩擦相关倍率 |
| `swim_speed_modifier` | 游泳速度倍率 |
| `poise_damage_modifier` | 造成硬直伤害的倍率 |
| `attack_damage_modifier` | 造成伤害的倍率 |
| `conditional_precision_modifiers` | 在满足某些 `CombatRequirement` 时的精准相关修正 |
| `precision_vulnerability_multiplier_override` | 覆盖受击方精准易伤倍率（可选） |
| `precision_power_mult` | 精准强度倍率 |
| `mitigations_penetration` | 无视对方一部分减伤的比例 |
| `energy_reward_modifier` | 获得体力回复/奖励的倍率 |
| `knockback_mult` | 击退倍率 |
| `crowd_control_resistance` | 对群体控制类效果的抗性 |
| `item_effect_reduction` | 对物品触发效果的衰减系数（默认 `1.0`） |
| `effects_on_attack` | 攻击时附加的 `AttackEffect` |
| `effects_on_damaged` | 受伤时触发的 `StatEffect` |
| `effects_on_death` | 死亡时触发的 `StatEffect` |
| `attacked_modifications` | 针对指向本实体的攻击的修改器 |
| `disable_auxiliary_abilities` | 为 true 时禁用辅助类能力 |

临时 Buff 等造成的改动可通过 `reset_temp_modifiers()` 按「保留名字与身体」重置回默认再重新计算（见 `Stats::reset_temp_modifiers`）。

---

## 3. `Health`（生命）

- 内部用定点数缩放存储，对外接口多为 `f32`。
- **`last_change`**：记录**最后一次**血量变化的数值、来源实体、原因、时间等；互助 AI、助攻统计等会读它（**不是**长期仇恨表，会被后续治疗/伤害覆盖）。

---

## 4. `Energy`（体力）

- 同样有 `current` / `base_max` / `maximum` 与内部缩放。
- 上限会受 `Stats.max_energy_modifiers` 影响（通过 `needs_maximum_update` / `update_internal_integer_maximum` 与 `Stats` 联动）。
- 带有 **再生速率** `regen_rate`（随时间加速恢复）。

---

## 5. `Poise`（硬直 / 韧性）

- 与 `Health` 类似，有当前值、基础上限、受 Buff 影响后的上限。
- **`PoiseState`**：`Normal`、`Interrupted`、`Stunned`、`Dazed`、`KnockedDown` 等，决定硬直表现与动画/控制限制。
- 减免与 `Stats.poise_reduction`、`Stats.poise_damage_modifier` 等配合武器与技能计算。

---

## 6. `SkillSet`（技能与经验）

- 源码：`common/src/comp/skillset/mod.rs` 及子模块 `skills`。
- **技能分组** `SkillGroupKind`：至少包括 **`General`**（通用）与 **`Weapon(ToolKind)`**（按武器类型，如剑、斧、弓等）。
- 每组有独立 **技能点等级** 与 **经验**；升级消耗、武器组经验曲线在 `skill_point_cost` 等函数中定义。
- 技能列表、每组包含哪些技能、技能最高等级、前置关系等来自资产：  
  `assets/common/skill_trees/skills_skill-groups_manifest.ron`、  
  `assets/common/skill_trees/skill_max_levels.ron`、  
  `assets/common/skill_trees/skill_prerequisites.ron`。

---

## 7. 与「阵营 / 是否被 NPC 打」相关的组件（补充）

- 玩家常用 **`Alignment::Owned(自己的 Uid)`**（加载流程里在 `server/src/state_ext.rs` 等处写入），与村民 `Alignment::Npc` 的敌对判断在 `common/src/comp/agent.rs` 的 `Alignment::hostile_towards` / `passive_towards` 等中定义。
- 这与 `Stats` 无关；村民攻击更多由 **AI、装扮标签、`Health.last_change`** 等驱动（参见此前对话中的分析）。

---

## 8. 若需改数值设计

- **单条属性公式**：优先查 `Stats` 与各系统（`common/systems`）里对 `StatsModifier` / `StatsSplit` 的使用。
- **技能树与消耗**：改 `assets/common/skill_trees/` 下 RON，并注意与 `SKILL_GROUP_HASHES` 相关的 **强制洗点** 行为（改组内技能集合可能触发）。

---

## 9. 人物面板上的属性（HUD 里实际显示什么）

游戏里有 **两处**常见「人物属性」界面，数值来源略有不同，以源码为准。

### 9.1 背包侧栏（Bag 展开后的属性条）

- **实现**：`voxygen/src/hud/bag.rs`（常量 `STATS` + 图标旁数值）。
- **共 6 项**（从左到右一列）：生命、体力、防护、战斗评分、硬直抗性、潜行。

| 界面项（代码内标识） | 数值含义（实现摘要） | 悬浮说明文案键（英文在 `assets/voxygen/i18n/en/hud/bag.ftl`） |
|----------------------|----------------------|----------------------------------------------------------------|
| Health | `health.maximum()` 取整 | `hud-bag-health` |
| Energy | `energy.maximum()` 取整 | `hud-bag-energy` |
| Protection | `100 * Damage::compute_damage_reduction(None, inventory, stats, msm)` 取整为 **百分比整数** | `hud-bag-protection` + `hud-bag-protection_desc`（文案：护甲带来的伤害减免） |
| Combat Rating | `combat_rating(...)*10` 取整显示，且 `combat_rating` 上限夹在 `999.9` | `hud-bag-combat_rating` + `hud-bag-combat_rating_desc`（文案：由装备与生命等计算） |
| Stun Resilience | `100 * Poise::compute_poise_damage_reduction(inventory, msm, None, stats)` 取整为 **百分比** | `hud-bag-stun_res` + `hud-bag-stun_res_desc`（连续受击硬直相关） |
| Stealth | `(1 - perception_dist_multiplier_from_stealth(inventory, None, msm)) * 100`，一位小数 + `%` | `hud-bag-stealth` |

**中文界面**：在 `assets/voxygen/i18n/zh-Hans/hud/bag.ftl`（或你当前语言目录下同路径）覆盖上述键即可改玩家看到的说明。

### 9.2 日记「角色」（Diary → Character）

- **实现**：`voxygen/src/hud/diary.rs` 中 `DiarySection::Character` 与枚举 `CharacterStat`（共 **15** 行）。
- **列出的条目**：名称、战斗模式（PvP/PvE）、路点、生命上限、体力上限、**硬直上限（Poise maximum）**、战斗评分、防护、硬直减伤%、暴击强度倍率、体力奖励%、潜行%、主手/副手 **武器 Power / Speed / Effect Power**（双持时两个数并排）。

与背包侧栏的 **主要差异**：

- **Protection**：日记里用 `combat::compute_protection(Some(inventory), msm)`，显示为整数或 `"Invincible"`；背包用 **`compute_damage_reduction` 的百分比**。二者**不是同一函数**，数值含义不要混为一谈。
- **Poise**：日记显示 **最大硬直条**数值；背包显示的是 **硬直伤害减免比例**（`compute_poise_damage_reduction`）。
- **日记**额外有路点、战斗模式、武器三项拆分等。

**列标题文案键**：见 `CharacterStat::localized_str`（如 `hud-bag-health`、`common-stats-precision_power` 等），定义在同一 `diary.rs` 文件末尾附近。

---

## 10. 装备 Tooltip 上的属性（物品说明）

悬浮物品时，**`ItemTooltip`**（`voxygen/src/ui/widgets/item_tooltip.rs`）按物品类型渲染不同行。装备与 **耐久** 会参与 `tool.stats(durability_multiplier)` / `armor.stats(msm, durability_multiplier)` 的缩放（见 `common/src/comp/inventory/item/tool.rs` 的 `with_durability_mult` 等）。

### 10.1 武器（`ItemKind::Tool`）

使用 `Tool::stats` 返回的 **`tool::Stats`** 字段：

| 显示名（Fluent 键） | 含义 | Tooltip 展示方式（约） |
|---------------------|------|-------------------------|
| `common-stats-power` | 威力 `power` | `power * 10.0`，一位小数 |
| `common-stats-speed` | 攻速 | `(speed - 1.0) * 100`，带正负的 `%` |
| `common-stats-effect-power` | 效果强度 | 注意 UI 里写成 **相对 1.0 的百分比** `(effect_power - 1.0) * 100`（与日记里武器 Effect Power 用 `*10` 的展示不一致，属客户端两套用途） |
| `common-stats-range` | 距离/范围倍率 | `(range - 1.0) * 100` → `%` |
| `common-stats-energy_efficiency` | 体力效率 | `(energy_efficiency - 1.0) * 100` → `%` |
| `common-stats-buff_strength` | Buff/Debuff 强度 | `(buff_strength - 1.0) * 100` → `%` |
| `common-stats-durability` | 耐久 | 当前/最大（有耐久时） |

若正在对比 **同槽位已装备** 的另一把武器，右侧会用颜色标 **差值**（同一文件内 `diff` 逻辑）。

### 10.2 护甲（`ItemKind::Armor`）

使用 `armor.stats(msm, durability_multiplier)`，**仅当对应 Option 有值才显示该行**：

| 显示名（Fluent 键） | 含义 |
|---------------------|------|
| `common-stats-armor` | 防护（`Protection` 类型，经 `util::protec2string` 格式化） |
| `common-stats-poise_res` | 硬直抗性（同上） |
| `common-stats-energy_max` | 最大体力加成（浮点） |
| `common-stats-energy_reward` | 体力回复/奖励相关（×100 后以 `%` 显示） |
| `common-stats-precision_power` | 暴击强度（英文键名 Crit Power，×100 后以 `%` 显示） |
| `common-stats-stealth` | 潜行（原始系数，三位小数） |
| `common-stats-slots` | 物品自带栏位数（`item.num_slots()`） |
| `common-stats-durability` | 耐久 |

对比已装备同槽护甲时，同样有 **差值** 列。

### 10.3 消耗品与其它

- **消耗品**：`line_count` 在 `voxygen/src/hud/util.rs` 中走 `consumable_desc`，按效果列出多行说明，**不一定**走「武器/护甲那种 stat 行」。
- **通用词条名**：英文集中在 `assets/voxygen/i18n/en/common.ftl` 的 `common-stats-*`；改说明优先改 FTL，不必改 Rust。

---

*文档生成自仓库源码结构，游戏版本升级后字段可能有增删，请以实际 `common/src/comp/*.rs` 与 `voxygen/src/hud/*.rs` 为准。*
