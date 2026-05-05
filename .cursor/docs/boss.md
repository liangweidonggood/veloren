# dungeon

assets/common/entity/dungeon

| 英文名         | 中文名           | 难度 | boss列表                                 |
| -------------- | ---------------- | ---- | ---------------------------------------- |
| adlet          | 狼人要塞         | T2   | [elder,yeti]                             |
| cultist        | 邪教徒地牢       | T5   | [mindflayer,warlord,warlock,beastmaster] |
| dwarven_quarry | 矿洞             | T5   | [forgemaster,irongolem]                  |
| fallback       | 占位/测试地牢    | T0   | [boss,miniboss]                          |
| gnarling       | 哥纳林要塞       | T1   | [chieftain,woodgolem]                    |
| haniwa         | 陶俑勇士地下墓穴 | T4   | [gravewarden,general]                    |
| myrmidon       | 迈密敦地牢       | T5   | [minotaur,cyclops]                       |
| sahagin        | 萨哈金岛屿       | T3   | [karkatha,hakulaq]                       |
| sea_chapel     | 海上教堂         | T5   | [dagon,cardinal]                         |
| terracotta     | 陶俑遗迹         | T5   | [cursekeeper,mogwai]                     |
| vampire        | 吸血鬼城堡       | T4   | [bloodmoon_heiress,strigoi,executioner]  |

矿洞t5

召唤

```text

# 狼人要塞t2
/make_npc dungeon.adlet.elder 49                                # 阿德莱特长老 2k,绿
/make_npc dungeon.adlet.hunter 49                               # 狼人猎手 65,灰
/make_npc dungeon.adlet.icepicker 49                            # 冰镐狼人 65,灰
/make_npc dungeon.adlet.tracker 49                              # 狼人追踪者 65,灰
/make_npc dungeon.adlet.yeti 49                                 # 雪人 2k,黄


# 邪教徒地牢t5
/make_npc dungeon.cultist.beastmaster 49                         # 忠诚黑暗猎犬 80 灰
/make_npc dungeon.cultist.cultist 49                             # 邪教徒 100,白
/make_npc dungeon.cultist.hound 49                              # 忠诚黑暗猎犬
/make_npc dungeon.cultist.husk 49                               # 邪教徒躯壳 50 灰
/make_npc dungeon.cultist.husk_brute 49                             # 枯壳蛮兵 800 白
/make_npc dungeon.cultist.mindflayer 49                             # 夺心魔 2k 黄
/make_npc dungeon.cultist.turret 49                                 # 魔能炮塔 80 灰
/make_npc dungeon.cultist.warlock 49                                # 邪教术士 200 绿
/make_npc dungeon.cultist.warlord 49                                # 邪教军阀 200 绿

# 矿洞t5
/make_npc dungeon.dwarven_quarry.flamekeeper 49                    # 火焰守护者 2k, 黄
/make_npc dungeon.dwarven_quarry.forgemaster 49                    # 锻造大师 10k,黄
/make_npc dungeon.dwarven_quarry.iron_dwarf 49                     # 铁矮人，锻造大师召唤物
/make_npc dungeon.dwarven_quarry.irongolem 49                      # 铁魔像 2k,黄
/make_npc dungeon.dwarven_quarry.irongolem_key 49                  # 铁魔像 2k,黄
/make_npc dungeon.dwarven_quarry.lavathrower 49                    # 熔岩抛射者 80 灰
/make_npc dungeon.dwarven_quarry.mine_guard 49                     # 地雷守卫 100 白
/make_npc dungeon.dwarven_quarry.miner 49                          # 贪婪的矿工 100 白
/make_npc dungeon.dwarven_quarry.snaretongue_forge_key 49          # 陷舌兽 2k,黄
/make_npc dungeon.dwarven_quarry.snaretongue_miner_key 49          # 陷舌兽 2k,黄
/make_npc dungeon.dwarven_quarry.turret 49                         # 机械炮塔 80 灰


# 测试用的t0
/make_npc dungeon.fallback.boss 49          # 疯狂绵羊 30 灰
/make_npc dungeon.fallback.enemy 49         # 严虎 100 灰
/make_npc dungeon.fallback.miniboss 49      # 大鹅 25 灰


# 哥纳林要塞t1
/make_npc dungeon.gnarling.chieftain 49     # 狡灵酋长 150 白
/make_npc dungeon.gnarling.harvester 49     # 收割者 1k 绿
/make_npc dungeon.gnarling.logger 49        # 粗犷的伐木工 50 灰
/make_npc dungeon.gnarling.mandragora 49    # 曼德拉草 65 灰
/make_npc dungeon.gnarling.mugger 49        # 凶悍的劫匪 50 灰
/make_npc dungeon.gnarling.stalker 49       # 咆哮追猎者 50 灰
/make_npc dungeon.gnarling.woodgolem 49     # 木魔像 120 白

# 陶俑勇士地下墓穴t4
/make_npc dungeon.haniwa.ancienteffigy 49    # 古代肖像 250 白
/make_npc dungeon.haniwa.archer 49           # 陶俑弓箭手 100 灰
/make_npc dungeon.haniwa.claygolem 49        # 黏土魔像 350 绿
/make_npc dungeon.haniwa.claysteed 49        # 泥土骏马 400 白
/make_npc dungeon.haniwa.general 49          # 填轮将军 600 绿
/make_npc dungeon.haniwa.gravewarden 49      # 坟墓守护者 1k 黄
/make_npc dungeon.haniwa.guard 49            # 陶俑护卫 100 灰
/make_npc dungeon.haniwa.sentry 49           # 陶俑哨兵 60 灰
/make_npc dungeon.haniwa.soldier 49          # 陶俑士兵 100 灰

# 迈密敦地牢t5
/make_npc dungeon.myrmidon.cyclops 49           # 独眼巨人 1k 紫
/make_npc dungeon.myrmidon.cyclops_key 49       # 独眼巨人 1k 紫
/make_npc dungeon.myrmidon.hoplite 49           # 迈密敦重装步兵 100 白
/make_npc dungeon.myrmidon.marksman 49          # 迈密敦精英射手 100 灰
/make_npc dungeon.myrmidon.minotaur 49          # 米诺陶 3k 黄
/make_npc dungeon.myrmidon.strategian 49        # 迈密敦战略家 100 白

# 萨哈金岛屿t3
/make_npc dungeon.sahagin.hakulaq 49            # 哈库拉克 155 灰
/make_npc dungeon.sahagin.karkatha 49           # 卡尔卡萨 2k 黄
/make_npc dungeon.sahagin.sniper 49             # 萨哈金狙击手 85 灰
/make_npc dungeon.sahagin.soldier_crab 49       # 士兵蟹 50 灰
/make_npc dungeon.sahagin.sorcerer 49           # 萨哈金巫师 85 灰
/make_npc dungeon.sahagin.spearman 49           # 萨哈长矛手 85 灰
/make_npc dungeon.sahagin.tidalwarrior 49       # 潮汐战士 2k 黄

# 海上教堂t5
/make_npc dungeon.sea_chapel.cardinal 49            # 红衣主教 100 绿
/make_npc dungeon.sea_chapel.coralgolem 49          # 珊瑚魔像 550 白
/make_npc dungeon.sea_chapel.dagon 49               # 达贡 1k 黄
/make_npc dungeon.sea_chapel.dagonite 49            # 达贡兽 70 灰
/make_npc dungeon.sea_chapel.organ 49               # 风琴 500 灰
/make_npc dungeon.sea_chapel.prisoner 49            # 囚犯 100 灰
/make_npc dungeon.sea_chapel.sea_bishop 49          # 海洋主教 550 白
/make_npc dungeon.sea_chapel.sea_cleric 49          # 海渊牧师 100 灰


# 陶俑遗迹t5
/make_npc dungeon.terracotta.besieger 49                            # 陶俑攻城者 300 白
/make_npc dungeon.terracotta.cursekeeper 49                         # 诅咒守护者 3k 黄
/make_npc dungeon.terracotta.cursekeeper_fake 49                    # 诅咒守护者 3k 黄
/make_npc dungeon.terracotta.demolisher 49                          # 陶俑破坏者 300 白
/make_npc dungeon.terracotta.jiangshi 49                            # 僵尸 250 灰
/make_npc dungeon.terracotta.mogwai 49                              # 魔怪 500 绿
/make_npc dungeon.terracotta.punisher 49                            # 陶俑惩罚者 300 白
/make_npc dungeon.terracotta.pursuer 49                             # 陶俑追击者 300 白
/make_npc dungeon.terracotta.shamanic_spirit 49                     # 萨满之灵 240 灰
/make_npc dungeon.terracotta.shamanic_spirit_key 49                 # 萨满之灵 240 灰
/make_npc dungeon.terracotta.terracotta_statue 49                   # 陶俑雕像 600 绿
/make_npc dungeon.terracotta.terracotta_statue_key 49               # 陶俑雕像 600 绿
/make_npc dungeon.terracotta.terracotta_statue_key_chance 49        # 陶俑雕像 600 绿

# 吸血鬼城堡t4
/make_npc dungeon.vampire.bloodmoon_bat 49              # 血月继承者 1k 蓝
/make_npc dungeon.vampire.bloodmoon_heiress 49          # 血月继承者 2k 黄
/make_npc dungeon.vampire.bloodservant 49               # 血仆 300 白
/make_npc dungeon.vampire.executioner 49                # 刽子手 800 白
/make_npc dungeon.vampire.harlequin 49                  # 小丑 500 白
/make_npc dungeon.vampire.strigoi 49                    # 斯特里戈伊 800 白
/make_npc dungeon.vampire.vampire_bat 49                # 吸血蝙蝠 100 灰
```

boss

```text
/make_npc dungeon.adlet.elder 49                                # 阿德莱特长老 2k,绿
/make_npc dungeon.adlet.yeti 49                                 # 雪人 2k,黄
/make_npc dungeon.cultist.mindflayer 49                         # 夺心魔 2k 黄
/make_npc dungeon.dwarven_quarry.flamekeeper 49                 # 火焰守护者 2k, 黄
/make_npc dungeon.dwarven_quarry.forgemaster 49                 # 锻造大师 10k,黄
/make_npc dungeon.dwarven_quarry.irongolem 49                   # 铁魔像 2k,黄
/make_npc dungeon.dwarven_quarry.snaretongue_forge_key 49       # 陷舌兽 2k,黄
/make_npc dungeon.gnarling.harvester 49                         # 收割者 1k 绿
/make_npc dungeon.haniwa.gravewarden 49                         # 坟墓守护者 1k 黄
/make_npc dungeon.myrmidon.cyclops 49                           # 独眼巨人 1k 紫
/make_npc dungeon.myrmidon.minotaur 49                          # 米诺陶 3k 黄
/make_npc dungeon.sahagin.karkatha 49                           # 卡尔卡萨 2k 黄
/make_npc dungeon.sahagin.tidalwarrior 49                       # 潮汐战士 2k 黄
/make_npc dungeon.sea_chapel.dagon 49                           # 达贡 1k 黄
/make_npc dungeon.terracotta.cursekeeper 49                     # 诅咒守护者 3k 黄
/make_npc dungeon.vampire.bloodmoon_bat 49                      # 血月继承者 1k 蓝
/make_npc dungeon.vampire.bloodmoon_heiress 49                  # 血月继承者 2k 黄
/make_npc wild.aggressive.hydra 49                              # 九头蛇 1k 蓝
/make_npc wild.aggressive.seawyvern 49                          # 海霄双足飞龙 1k 黄
/make_npc world.world_bosses.gigas_fire 49                      # 火焰巨神 25k 骷髅
/make_npc world.world_bosses.gigas_frost 49                     # 冰霜巨神 30k 骷髅
```

npc

```text
/make_npc village.alchemist 49                                  # 炼金术士
/make_npc village.blacksmith 49                                 # 铁匠
/make_npc village.bowman 49                                     # 牛人弓箭手
/make_npc village.captain 49                                    # 船长自带安全区光环，无法被攻击
/make_npc village.chef 49                                       # 厨师
/make_npc village.dummy 49                                      # 训练假人
/make_npc village.farmer 49                                     # 农夫
/make_npc village.guard 49                                      # 守卫
/make_npc village.herbalist 49                                  # 草药师
/make_npc village.hunter 49                                     # 猎人
/make_npc village.merchant 49                                   # 商人
/make_npc village.mountaineer 49                                # 登山者
/make_npc village.skinner 49                                    # 剥皮匠
/make_npc village.villager 49                                   # 村民




/make_npc world.traveler0 49                                    # 新手冒险者
/make_npc world.traveler1 49                                    # 旅人
/make_npc world.traveler2 49                                    # 见多识广的冒险者
/make_npc world.traveler3 49                                    # 经验丰富的冒险者
/kill_npcs                                                      # 清理所有npc
```
