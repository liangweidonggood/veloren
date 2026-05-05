# Veloren 管理员命令速查（单人 / 自建服）

权限来源：`common/src/cmd.rs` 中 `ServerChatCommand::data()` 对每条命令标注的 `Some(Admin)` 或 `Some(Moderator)`。

## 使用提示

- **列出命令**：`/help`
- **单条命令帮助**：`/help <command>`（例如 `/help give_item`）
- **参数补全**：输入命令后按 **Tab**
- **在线玩家名（alias）**：`/players`（多数管理命令按 **alias** 匹配，需与显示名完全一致）

## `/give_item` 与物品 ID

- 物品参数为 **资源路径**，前缀为 `common.items.`（见 `GiveItem` 的 `AssetPath` 定义）。
- 服务器会把参数里的 `/`、`\` 替换为 `.`。
- 若输入框或输入法把内容改写，可能得到无效 ID；拿钥匙可优先用 **`/kit keys`**。

---

## Admin（管理员）命令

| 命令                      | 说明（简要）                                                  |
| ------------------------- | ------------------------------------------------------------- |
| `adminify`                | 临时授予/移除玩家的临时管理员或版主角色（执行者需永久 Admin） |
| `airship`                 | 生成飞艇                                                      |
| `aura`                    | 创建光环                                                      |
| `buff`                    | 施加 buff                                                     |
| `into_npc`                | 将自己变为 NPC（慎用）                                        |
| `body`                    | 改变身体/种族体型                                             |
| `battlemode_force`        | 强制设置战斗模式                                              |
| `area_add`                | 新增建造区域                                                  |
| `area_list`               | 列出建造区域                                                  |
| `area_remove`             | 移除建造区域                                                  |
| `campfire`                | 生成篝火                                                      |
| `clear_persisted_terrain` | 清除附近持久化地形                                            |
| `death_effect`            | 为目标添加死亡效果（如 transform）                            |
| `debug_column`            | 调试用：列信息                                                |
| `debug_ways`              | 调试用：ways 信息                                             |
| `disconnect_all_players`  | 断开所有玩家（需确认参数）                                    |
| `dummy`                   | 生成训练假人                                                  |
| `explosion`               | 爆炸                                                          |
| `give_item`               | 给自己物品（可带数量）                                        |
| `gizmos`                  | 管理 gizmo 订阅                                               |
| `gizmos_range`            | 调整 gizmo 订阅范围                                           |
| `goto`                    | 传送到坐标                                                    |
| `goto_rand`               | 随机传送                                                      |
| `health`                  | 设置生命值                                                    |
| `jump`                    | 相对当前位置偏移                                              |
| `kill_npcs`               | 杀死范围内 NPC                                                |
| `kit`                     | 将一组物品放入背包（如 `keys`）                               |
| `lantern`                 | 调整灯笼强度与颜色                                            |
| `light`                   | 生成带光实体                                                  |
| `make_block`              | 生成带颜色的方块                                              |
| `make_npc`                | 从实体配置生成 NPC                                            |
| `make_sprite`             | 生成 sprite                                                   |
| `make_volume`             | 生成 volume（调试用）                                         |
| `object`                  | 生成 object                                                   |
| `outcome`                 | 创建 outcome                                                  |
| `permit_build`            | 授予某建造区域权限                                            |
| `poise`                   | 设置韧性                                                      |
| `portal`                  | 生成传送门                                                    |
| `reload_chunks`           | 重载附近区块                                                  |
| `reset_recipes`           | 重置配方书                                                    |
| `remove_lights`           | 移除光源                                                      |
| `revoke_build`            | 撤销某区域建造权限                                            |
| `revoke_build_all`        | 撤销全部建造权限                                              |
| `set_motd`                | 设置 MOTD                                                     |
| `set_body_type`           | 设置体型（可选是否永久）                                      |
| `ship`                    | 生成船                                                        |
| `skill_point`             | 增加技能点                                                    |
| `skill_preset`            | 应用技能预设                                                  |
| `spawn`                   | 生成实体（alignment、entity、数量等）                         |
| `spot`                    | 查找 spot                                                     |
| `time`                    | 设置时间                                                      |
| `time_scale`              | 设置时间流逝倍率                                              |
| `rtsim_tp`                | RTSim：传送 NPC                                               |
| `rtsim_info`              | RTSim：信息                                                   |
| `rtsim_npc`               | RTSim：查询 NPC                                               |
| `rtsim_purge`             | RTSim：清除数据（下次启动）                                   |
| `rtsim_chunk`             | RTSim：chunk 相关                                             |
| `set_waypoint`            | 设置路标                                                      |
| `wiring`                  | 线路/电路相关                                                 |
| `weather_zone`            | 天气区域                                                      |
| `lightning`               | 闪电                                                          |
| `scale`                   | 缩放实体                                                      |
| `repair_equipment`        | 修复装备（可选是否含背包）                                    |
| `tether`                  | 拴绳                                                          |
| `destroy_tethers`         | 摧毁与自己相连的拴绳                                          |
| `mount`                   | 骑乘目标实体                                                  |
| `dismount`                | 让目标下马/卸载                                               |

---

## Moderator（版主）命令

| 命令              | 说明（简要）                     |
| ----------------- | -------------------------------- |
| `alias`           | 修改别名                         |
| `ban`             | 封禁玩家                         |
| `ban_ip`          | IP 关联封禁                      |
| `ban_log`         | 封禁记录                         |
| `dropall`         | 丢弃全部物品                     |
| `respawn`         | 复活相关                         |
| `kick`            | 踢出玩家                         |
| `safezone`        | 安全区                           |
| `server_physics`  | 服务器权威物理相关               |
| `site`            | 传送到站点（名称可含空格）       |
| `sudo`            | 以其他实体身份执行子命令（慎用） |
| `tp`              | 传送                             |
| `unban`           | 解除封禁                         |
| `unban_ip`        | 解除 IP 封禁                     |
| `whitelist`       | 白名单 add/remove                |
| `create_location` | 创建地点                         |
| `delete_location` | 删除地点                         |

---

## 常用示例

```text
/kit keys
```

```text
/give_item common.items.keys.terracotta_key_door 1
```

```text
/spawn wild common.entity.dungeon.terracotta.cursekeeper 1
```

```text
/help spawn
```
