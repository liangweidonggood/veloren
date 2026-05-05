# dungeon

assets/common/entity/dungeon

| 英文名         | 中文名         | 难度 | boss列表                                 |
| -------------- | -------------- | ---- | ---------------------------------------- |
| adlet          | 阿德雷特地牢   | T1   | [elder,yeti]                             |
| cultist        | 邪教徒地牢     | T5   | [mindflayer,warlord,warlock,beastmaster] |
| dwarven_quarry | 矮人采石场地牢 | T4   | [forgemaster,irongolem]                  |
| fallback       | 占位/测试地牢  | T0   | [boss,miniboss]                          |
| gnarling       | 格纳林地牢     | T1   | [chieftain,woodgolem]                    |
| haniwa         | 埴轮地牢       | T2   | [gravewarden,general]                    |
| myrmidon       | 蚁兵地牢       | T3   | [minotaur,cyclops]                       |
| sahagin        | 萨胡阿金地牢   | T2   | [karkatha,hakulaq]                       |
| sea_chapel     | 海之教堂地牢   | T3   | [dagon,cardinal]                         |
| terracotta     | 陶俑/陶土地牢  | T4   | [cursekeeper,mogwai]                     |
| vampire        | 吸血鬼地牢     | T4   | [bloodmoon_heiress,strigoi,executioner]  |

召唤

```text
/make_npc dungeon.adlet.elder 49
/make_npc dungeon.adlet.hunter 49
/make_npc dungeon.adlet.icepicker 49
/make_npc dungeon.adlet.tracker 49
/make_npc dungeon.adlet.yeti 49

/make_npc dungeon.cultist.beastmaster 49
/make_npc dungeon.cultist.cultist 49
/make_npc dungeon.cultist.hound 49
/make_npc dungeon.cultist.husk 49
/make_npc dungeon.cultist.husk_brute 49
/make_npc dungeon.cultist.mindflayer 49
/make_npc dungeon.cultist.turret 49
/make_npc dungeon.cultist.warlock 49
/make_npc dungeon.cultist.warlord 49

/make_npc dungeon.dwarven_quarry.flamekeeper 49
/make_npc dungeon.dwarven_quarry.forgemaster 49
/make_npc dungeon.dwarven_quarry.iron_dwarf 49
/make_npc dungeon.dwarven_quarry.irongolem 49
/make_npc dungeon.dwarven_quarry.irongolem_key 49
/make_npc dungeon.dwarven_quarry.lavathrower 49
/make_npc dungeon.dwarven_quarry.mine_guard 49
/make_npc dungeon.dwarven_quarry.miner 49
/make_npc dungeon.dwarven_quarry.snaretongue_forge_key 49
/make_npc dungeon.dwarven_quarry.snaretongue_miner_key 49
/make_npc dungeon.dwarven_quarry.turret 49

/make_npc dungeon.fallback.boss 49
/make_npc dungeon.fallback.enemy 49
/make_npc dungeon.fallback.miniboss 49

/make_npc dungeon.gnarling.chieftain 49
/make_npc dungeon.gnarling.harvester 49
/make_npc dungeon.gnarling.logger 49
/make_npc dungeon.gnarling.mandragora 49
/make_npc dungeon.gnarling.mugger 49
/make_npc dungeon.gnarling.stalker 49
/make_npc dungeon.gnarling.woodgolem 49

/make_npc dungeon.haniwa.ancienteffigy 49
/make_npc dungeon.haniwa.archer 49
/make_npc dungeon.haniwa.claygolem 49
/make_npc dungeon.haniwa.claysteed 49
/make_npc dungeon.haniwa.general 49
/make_npc dungeon.haniwa.gravewarden 49
/make_npc dungeon.haniwa.guard 49
/make_npc dungeon.haniwa.sentry 49
/make_npc dungeon.haniwa.soldier 49

/make_npc dungeon.myrmidon.cyclops 49
/make_npc dungeon.myrmidon.cyclops_key 49
/make_npc dungeon.myrmidon.hoplite 49
/make_npc dungeon.myrmidon.marksman 49
/make_npc dungeon.myrmidon.minotaur 49
/make_npc dungeon.myrmidon.strategian 49

/make_npc dungeon.sahagin.hakulaq 49
/make_npc dungeon.sahagin.karkatha 49
/make_npc dungeon.sahagin.sniper 49
/make_npc dungeon.sahagin.soldier_crab 49
/make_npc dungeon.sahagin.sorcerer 49
/make_npc dungeon.sahagin.spearman 49
/make_npc dungeon.sahagin.tidalwarrior 49

/make_npc dungeon.sea_chapel.cardinal 49
/make_npc dungeon.sea_chapel.coralgolem 49
/make_npc dungeon.sea_chapel.dagon 49
/make_npc dungeon.sea_chapel.dagonite 49
/make_npc dungeon.sea_chapel.organ 49
/make_npc dungeon.sea_chapel.prisoner 49
/make_npc dungeon.sea_chapel.sea_bishop 49
/make_npc dungeon.sea_chapel.sea_cleric 49

/make_npc dungeon.terracotta.besieger 49
/make_npc dungeon.terracotta.cursekeeper 49
/make_npc dungeon.terracotta.cursekeeper_fake 49
/make_npc dungeon.terracotta.demolisher 49
/make_npc dungeon.terracotta.jiangshi 49
/make_npc dungeon.terracotta.mogwai 49
/make_npc dungeon.terracotta.punisher 49
/make_npc dungeon.terracotta.pursuer 49
/make_npc dungeon.terracotta.shamanic_spirit 49
/make_npc dungeon.terracotta.shamanic_spirit_key 49
/make_npc dungeon.terracotta.terracotta_statue 49
/make_npc dungeon.terracotta.terracotta_statue_key 49
/make_npc dungeon.terracotta.terracotta_statue_key_chance 49

/make_npc dungeon.vampire.bloodmoon_bat 49
/make_npc dungeon.vampire.bloodmoon_heiress 49
/make_npc dungeon.vampire.bloodservant 49
/make_npc dungeon.vampire.executioner 49
/make_npc dungeon.vampire.harlequin 49
/make_npc dungeon.vampire.strigoi 49
/make_npc dungeon.vampire.vampire_bat 49
```

血条 1k 及以上（以游戏内血条显示为准）

```text
/make_npc dungeon.cultist.mindflayer 49                     # 2k    黄
/make_npc dungeon.dwarven_quarry.forgemaster 49             # 10k   黄
/make_npc dungeon.dwarven_quarry.irongolem 49               # 2k    黄
/make_npc dungeon.haniwa.gravewarden 49                     # 1k    黄
/make_npc dungeon.myrmidon.minotaur 49                      # 3k    黄
/make_npc dungeon.myrmidon.cyclops 49                       # 1k    紫
/make_npc dungeon.sahagin.karkatha 49                       # 2k    黄
/make_npc dungeon.sea_chapel.dagon 49                       # 1k    黄
/make_npc dungeon.terracotta.cursekeeper 49                 # 3k    黄
/make_npc dungeon.vampire.bloodmoon_heiress 49              # 2k    黄
```

npc

/make_npc village.villager 49
/make_npc village.merchant 49
/make_npc village.guard 49
/make_npc village.captain 49 # 船长自带安全区光环，无法被攻击
/make_npc village.bowman 49

/make_npc wild.aggressive.hydra 49
/make_npc wild.aggressive.seawyvern 49
/make_npc wild.aggressive.wendigo 49

/make_npc world.world_bosses.gigas_fire 49
/make_npc world.world_bosses.gigas_frost 49

/make_npc world.traveler0 49
/make_npc world.traveler1 49
/make_npc world.traveler2 49
/make_npc world.traveler3 49

/kill_npcs
