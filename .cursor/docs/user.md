# 管理员常用命令

```text
# 给背包
/give_item armor.misc.bag.soulkeeper_pure 1

# 给钥匙
/give_item keys.terracotta_key_door 1


# 法师装备说明（为何「没有攻击力」）
# - 武器伤害看法杖/权杖 ItemDef 里的 power；护甲没有「+攻击力」这种词条。
# - 护甲侧进攻向主要看 precision_power（暴击/精准伤害相关）等；材料线里 Linen→Sunsilk 只有护甲+能量，没有 precision，所以纯 Sunsilk 看起来像「只有耐力」是设定如此。

# 橙武 + 法杖（伤害在武器上）
/give_item weapons.sceptre.belzeshrub 1              # Artifact 橙；面板 power 最高档权杖
/give_item weapons.staff.laevateinn 1                # Legendary 金法杖

# —— 进攻向法师护甲（FromSet 带 precision_power，见 material_stats_manifest「Cultist」/「Witch」）——
# Cultist 套：Epic，套装数值里 precision 比 Witch 更高一档，邪教法师外观
/give_item armor.cultist.bandana 1
/give_item armor.cultist.chest 1
/give_item armor.cultist.shoulder 1
/give_item armor.cultist.hand 1
/give_item armor.cultist.belt 1
/give_item armor.cultist.pants 1
/give_item armor.cultist.foot 1
/give_item armor.cultist.necklace 1
/give_item armor.cultist.ring 1                     # 戒指槽共 2 个：本枚 + 下面 diamond 1 枚（不要发 3 枚戒指）

# Witch 套：High，布袍风格 + 能量 + precision（不想邪教可改用这套，去掉上面 Cultist 整段即可）
# /give_item armor.witch.hat 1
# /give_item armor.witch.chest 1
# /give_item armor.witch.shoulder 1
# /give_item armor.witch.hand 1
# /give_item armor.witch.belt 1
# /give_item armor.witch.back 1
# /give_item armor.witch.pants 1
# /give_item armor.witch.foot 1

# 首饰堆 precision（项链：邪教 necklace 与 diamond neck 二选一佩戴）
/give_item armor.misc.neck.diamond 1                # Epic，precision_power 高
/give_item armor.misc.ring.diamond 1                # Epic precision；与 cultist.ring 凑满两戒槽
/give_item armor.misc.head.howl_cowl 1             # 与 cultist bandana 头冲突，二选一

# 续航向纯布金套（几乎不加 precision，只有能量+甲）——与上面进攻套二选一
# /give_item armor.cloth.sunsilk.head 1
# /give_item armor.cloth.sunsilk.chest 1
# /give_item armor.cloth.sunsilk.shoulder 1
# /give_item armor.cloth.sunsilk.hand 1
# /give_item armor.cloth.sunsilk.belt 1
# /give_item armor.cloth.sunsilk.back 1
# /give_item armor.cloth.sunsilk.pants 1
# /give_item armor.cloth.sunsilk.foot 1

/give_item armor.misc.bag.mindflayer_spellbag 1     # 传奇背包；要大格用 soulkeeper_pure

# 超模 Boss 套（precision 极高 + 离谱护甲，慎用）
# /give_item armor.cardinal.mitre 1
# /give_item armor.cardinal.chest 1
# …其余 armor.cardinal.*

# 项圈
/give_item utility.collar 1


/give_item lantern.delvers_lamp 1     # 深掘者提灯
/give_item glider.tephras 1           # 火山喷发物滑翔伞


/give_item armor.misc.ring.abyssal_ring 1          # 深渊之戒
/give_item armor.misc.neck.abyssal_gorget 1        # 深渊护喉

# 奥利哈
/give_item armor.mail.orichalcum.head 1
/give_item armor.mail.orichalcum.chest 1
/give_item armor.mail.orichalcum.shoulder 1
/give_item armor.mail.orichalcum.hand 1
/give_item armor.mail.orichalcum.belt 1
/give_item armor.mail.orichalcum.back 1
/give_item armor.mail.orichalcum.pants 1
/give_item armor.mail.orichalcum.foot 1



红色（Debug）物品（可直接 /give_item）
下面这些是仓库里 quality: Debug 的（路径换成 . 就是 /give_item 的 ID）：

/give_item debug.admin 1
/give_item debug.admin_back 1
/give_item debug.admin_stick 1
/give_item debug.admin_sword 1
/give_item debug.admin_black_hole 1
/give_item debug.glider 1
/give_item debug.golden_cheese 1
/give_item debug.velorite_bow_debug 1
/give_item debug.dungeon_purple 1
/give_item debug.cultist_chest_blue 1
/give_item debug.cultist_shoulder_blue 1
/give_item debug.cultist_hands_blue 1
/give_item debug.cultist_legs_blue 1
/give_item debug.cultist_boots 1
/give_item debug.cultist_belt 1



assets\common\items\debug\admin_sword.ron
debug.admin_sword
ItemDef(
    legacy_name: "Admin Greatsword",
    legacy_description: "Shouldn't this be a hammer?",
    kind: Tool((
        kind: Sword,
        hands: Two,
        stats: (
            equip_time_secs: 0.0,
            power: 999.9,
            effect_power: 999.9,
            speed: 1.0,
            range: 1.0,
            energy_efficiency: 1.0,
            buff_strength: 1.0,
        ),
    )),
    quality: Debug,
    tags: [],
    ability_spec: None,
)

自定义装备，法杖
ItemDef(
    legacy_name: "Admin staff",
    legacy_description: "Debug staff — by lwd",
    kind: Tool((
        kind: Staff,
        hands: Two,
        stats: (
            equip_time_secs: 0.0,
            power: 999.9,
            effect_power: 999.9,
            speed: 100.0,
            range: 10.0,
            energy_efficiency: 1.0,
            buff_strength: 10.0,
        ),
    )),
    quality: Debug,
    tags: [],
    ability_spec: None,
)


/give_item debug.admin_staff 1





为显示与模型做的登记
否则容易缺 HUD 图或手上模型：

文件	                                            作用
assets/common/item_i18n_manifest.ron               绑定到 Fluent 键weapon-staff-debug-admin_staff

Simple(
            "common.items.debug.admin_staff",
        ): "weapon-staff-debug-admin_staff",




assets/voxygen/i18n/en/item/admin.ftl              英文名 + 描述
weapon-staff-debug-admin_staff = Admin Staff (Debug)
    .desc = Absurd tool stats for local testing. Not balanced for multiplayer.


assets/voxygen/i18n/zh-Hans/item/admin.ftl         中文名「管理员法杖（调试）」+ 描述
weapon-staff-debug-admin_staff = 管理员法杖（调试）
    .desc = 极限武器数值，仅适合本地测试；联机不平衡。


assets/voxygen/item_image_manifest.ron             背包图标（复用 firestaff_starter 体素）
    Simple("common.items.debug.admin_staff"): VoxTrans(
        "voxel.weapon.staff.firestaff_starter",
        (1.0, 0.0, 0.0), (-130., 90.0, 0.0), 1.2,
    ),



assets/voxygen/voxel/biped_weapon_manifest.ron     手持外观
    Tool("common.items.debug.admin_staff"): (
        vox_spec: ("weapon.staff.firestaff_starter", (-2.5, -3.0, -3.0)),
        color: None
    ),


assets/voxygen/voxel/item_drop_manifest.ron        掉落体模型
    Simple("common.items.debug.admin_staff"): "voxel.weapon.staff.firestaff_starter",







另外还有两件也是 Debug 相关的装备文件（同样可给）：
/give_item armor.misc.tabard.admin 1
/give_item armor.misc.back.admin 1
橙色（Artifact）物品
仓库里 quality: Artifact 的只有 1 件武器：

/give_item weapons.sceptre.belzeshrub 1


/give_item weapons.sword.caladbolg 1
/give_item weapons.axe.parashu 1
/give_item weapons.hammer.mjolnir 1
/give_item weapons.bow.sagitta 1
/give_item weapons.staff.laevateinn 1
/give_item weapons.sceptre.caduceus 1



# 龙鳞套
/give_item armor.hide.dragonscale.head 1
/give_item armor.hide.dragonscale.chest 1
/give_item armor.hide.dragonscale.shoulder 1
/give_item armor.hide.dragonscale.hand 1
/give_item armor.hide.dragonscale.belt 1
/give_item armor.hide.dragonscale.back 1
/give_item armor.hide.dragonscale.pants 1
/give_item armor.hide.dragonscale.foot 1

# 盐晶
/give_item armor.brinestone.crown 1
/give_item armor.brinestone.chest 1
/give_item armor.brinestone.shoulder 1
/give_item armor.brinestone.hand 1
/give_item armor.brinestone.belt 1
/give_item armor.brinestone.back 1
/give_item armor.brinestone.pants 1
/give_item armor.brinestone.foot 1


# 魔像
/give_item armor.golemite.head 1
/give_item armor.golemite.chest 1
/give_item armor.golemite.shoulder 1
/give_item armor.golemite.hand 1
/give_item armor.golemite.belt 1
/give_item armor.golemite.back 1
/give_item armor.golemite.pants 1
/give_item armor.golemite.foot 1



/give_item armor.misc.bag.mindflayer_spellbag 1




# 提灯（Legendary）
/give_item lantern.crux 1               # 源核
/give_item lantern.polaris 1           # 北极星
/give_item lantern.delvers_lamp 1       # 深掘者提灯
# 滑翔翼（Legendary）
/give_item glider.skullgrin 1
/give_item glider.tephras 1
/give_item glider.winter_wings 1

# 钥匙
kit keys

# 加buff
/buff invulnerability 2 9999     # 无敌
/buff protecting_ward 50 9999    # 减伤，不能和狂热同时开
/buff hastened 50 9999            # 加攻，攻速和移速
/buff reckless 100 9999           # 狂热





# jump
/jump 0 0 20                     # 跳




# 传送
/goto 1000.0 2000.0 150.0


# 重载
/reload_chunks

# 清包
/dropall
```
