# 八职技能用途 Brief（开团前设计资料）

- 时间：2026-11-01 16:39:34 Asia/Shanghai
- 来源：`SKILL-DENOM.json` tip `425afa9f`；`LegacyQuickMagicRoleProfile.h`；`magic-role-lookup.jsonl` currentSnapshot；`RECOMMEND-AND-TEST-OPT-20261101.md`
- **间隔/读条**：`castUnits`/`cooldownUnits`/`durationUnits` 为 Object.dbf 快照引擎单位，**未证明=秒**；文案里写的「秒/分钟」以 historicalDescription 为准并另列。
- **命中**：画像不存「法术命中率%」。物理侧见 NPC/角色 `runtime_2f34`（命中阈值参与）与 `runtime_2f36`（回避）；刀客有「命中形态」自 buff（蛇迹难辨等）。法术是否命中以施法 RESOLVE/拒因为准。
- 测号：{"1": "sxsxzzz5", "2": "12345", "3": "jiuyue", "4": "A88888", "5": "helen1", "6": "helen", "7": "nana", "8": "nana1"}

## 开团组合怎么用这八职（摘要）

| 职 | 开团角色 | 何时上场 |
|---|---|---|
| 武术家 | 嘲讽拉怪（需药师甲/HOT；可选神药无敌窗） | **最先**接触怪（无敌/厚甲后），把怪拉回远程圈 |
| 药师 | 甲/HOT/应急奶；神药无敌/天灵神药开无敌窗；复活另册 | 开战前给坦，战中保血；**不贴脸** |
| 阴阳师 | 自buff→**定身**→站定倒转→四系 | 定身在怪被拉稳后立刻；倒转禁移动 |
| 修罗医师 | 减速/毒/破防/增伤；护驾给脆皮 | 定身后立刻上负面；远程位 |
| 刺客 | 远程射击/连击爆发 | **定住后**全力；未定身不贴脸 |
| 魔刀客 | 自 buff 形态→刀技物伤 | **定住后**再近战切入；开怪瞬间禁止冲脸 |
| 剑客 | Special7 剑技爆发 | 同魔刀：定住后再切入 |
| 祭符师 | 先召宠→宠奶/魔化→宠输出；制符另页 | 开战前召宠；宠可顶前但人站远程；制符不进开团循环 |


## 1. 剑客
- 测号：`sxsxzzz5` lv40 learned≈17
- **用途**：近战剑技爆发+少量控/自 buff；装剑 Special7
- **开团/上场**：定身后切入，不抢拉
- **命中/条件**：剑技靠选中合法敌+武器；控技雷霆光铧等看 RESOLVE
- **间隔注**：六脉心法 cdU=9000 极长；铁布杉 cdU=150
- 画像职责（节选）：
  - family1 pri≈0 `single/source-single-attack` → 狂殇水击, 神焰九转, 极光剑式
  - family2 pri≈1 `control` → 雷霆光铧, 怒涛排壑
  - family3 pri≈2 `attack-buff` → 舒筋活脉, 剑疾驶还, 九转乾坤
  - family4 pri≈3 `defence-buff` → 风伯见日, 铁布杉, 御气入神
  - family5 pri≈4 `self-recovery` → 六脉心法, 一气化三清
  - family6 pri≈5 `cleanse` → 仙音净化
  - family7 pri≈6 `area` → 无日剑式, 风卷残云, 烈焰焚日
  - family90 pri≈7 `source-area-attack` → 迅雷裂地, 幽冥闪, 舞水莲华
- 关键技快照（cast/cd 引擎单位）：
  - 11117 杳冥降势 | castU=10 cdU=90 durU=0 mp=200 | 以剑瞬间制造真空攻击，不露痕迹地灭杀对方
  - 10203 极光剑式 | castU=10 cdU=50 durU=0 mp=200 | 以剑瞬间制造光的攻击并使对方失明
  - 10118 雷霆光铧 | castU=10 cdU=20 durU=0 mp=150 | 以剑瞬间制造雷电的攻击并使对方淋痹
  - 10101 舒筋活脉 | castU=30 cdU=30 durU=6000 mp=26 | 提高力量并可增加ＨＰ，但会降低敏捷[限自己使用]
  - 10852 铁布杉 | castU=0 cdU=150 durU=100 mp=0 | 防御技法[限自己使用]
  - 11116 剑疾驶还 | castU=20 cdU=4800 durU=3000 mp=175 | 提高四成攻击力，但会降低敏捷[限自己使用]

## 2. 魔刀客
- 测号：`12345` lv50 learned≈23
- **用途**：先叠进攻/命中/防御/生命形态，再打刀技；多数形态 manual
- **开团/上场**：**刀客何时上场**：怪已被嘲讽拉回且阴阳定住后，再贴脸输出；开怪/拉怪阶段禁止冲脸
- **命中/条件**：蛇迹难辨等提升命中率（自）；输出仍要合法敌选中
- **间隔注**：霸绝天地 cdU=1800；怒火中烧自残换爆发 cdU=2400
- 画像职责（节选）：
  - family1 pri≈0 `source-single-attack/strike` → 百器王刀, 狂横长沙, 霸绝天地
  - family2 pri≈1 `offence-form-manual-choice` → 虎止咆野, 杀龙求道, 万魔噬魂
  - family3 pri≈2 `shield` → 灵力护盾
  - family4 pri≈3 `defenceform` → 天怒难犯, 熊拙于行
  - family5 pri≈4 `hp-form-manual-choice` → 百识象奔, 蛮牛强袭
  - family6 pri≈5 `accuracy-form-manual-choice` → 蛇迹难辨, 鬼影重重
  - family7 pri≈6 `power-form-manual-choice` → 巨魔再世, 躯灵复活
  - family9 pri≈8 `self-cost-area-manual` → 以血洗血
  - family16 pri≈15 `self-cost-burst-manual` → 怒火中烧
  - family17 pri≈16 `evasion-form-manual` → 妖精转生, 兔朔迷离
  - family18 pri≈17 `attribute-cost-offence-manual` → 龟回奘藏, 狼嗥血奔
- 关键技快照（cast/cd 引擎单位）：
  - 10516 万魔噬魂 | castU=40 cdU=10 durU=12000 mp=350 | 降低所有属性提升生命力，攻击，防御，命中、回避生命回复[限自己]
  - 10504 蛇迹难辨 | castU=20 cdU=10 durU=6000 mp=74 | 降低魅力提升命中率[限自己使用]
  - 10506 天怒难犯 | castU=30 cdU=10 durU=12000 mp=150 | 降低灵巧与敏捷提升二倍防御力与快速回复生命力[限自己使用]
  - 10534 灵力护盾 | castU=30 cdU=30 durU=36000 mp=75 | 将灵力笼罩全身并形成一道护盾[限自己使用]
  - 10525 霸绝天地 | castU=0 cdU=1800 durU=0 mp=0 | 霸绝天地[攻击技法]
  - 10508 百器王刀 | castU=0 cdU=600 durU=0 mp=0 | 百器王刀[攻击技法]
  - 10871 怒火中烧 | castU=0 cdU=2400 durU=150 mp=0 | 怒火中烧[损失自己的一百点生命力换来十五秒的三倍攻击力]

## 3. 武术家
- 测号：`jiuyue` lv60 learned≈19
- **用途**：嘲讽拉仇恨+减伤/铁布+踢技；团战坦克
- **开团/上场**：甲/HOT/无敌后嘲讽挑衅或狂舞→拉回集合点
- **命中/条件**：嘲讽 Special16 需选中；踢技同近战
- **间隔注**：嘲讽挑衅 cdU=40；狂舞 80；力抗山河 cdU=18000 极长
- 画像职责（节选）：
  - family1 pri≈0 `taunt` → 嘲讽挑衅
  - family2 pri≈1 `guard` → 铁布杉
  - family3 pri≈2 `physical-damage-reduction` → 黄龙浩气, 天露赋畀, 力抗山河
  - family4 pri≈3 `stun` → 伏虎冲天
  - family5 pri≈4 `source-single-attack/strike` → 撼天狂掌, 霸拳盖天, 卷云腿
  - family6 pri≈5 `self-recovery` → 乾坤气疗, 回气凝血
  - family7 pri≈6 `areaTaunt` → 嘲讽狂舞
  - family8 pri≈7 `drain-hp-strike` → 五行太极
  - family9 pri≈8 `defence-form-manual` → 龙战于野
  - family16 pri≈15 `weakening-area` → 如丧神守
  - family17 pri≈16 `throw-ammunition` → 投掷
- 关键技快照（cast/cd 引擎单位）：
  - 10513 嘲讽挑衅 | castU=0 cdU=40 durU=0 mp=0 | 嘲讽挑衅对方
  - 10954 嘲讽狂舞 | castU=5 cdU=80 durU=0 mp=0 | 疯狂嘲讽对方[群体]
  - 10852 铁布杉 | castU=0 cdU=150 durU=100 mp=0 | 防御技法[限自己使用]
  - 11078 力抗山河 | castU=0 cdU=18000 durU=3000 mp=0 | 所有物理伤害减低一半[限自己使用]
  - 10853 伏虎冲天 | castU=5 cdU=300 durU=30 mp=0 | 攻击技法[短时间震晕效果]
  - 11306 疯魔踢 | castU=0 cdU=1200 durU=0 mp=0 | 攻击技法

## 4. 刺客
- 测号：`A88888` lv53 learned≈23
- **用途**：远程枪/暗器+隐匿/闪避/爆发连段
- **开团/上场**：定身后射击/连击；可用惊骇术群体晕辅助
- **命中/条件**：射击需远程主手+弹药；未备装具易拒
- **间隔注**：五段击杀 cdU=9600；瞄准/连环射击 snapshot cdU=0（仍受攻速/动画）
- 画像职责（节选）：
  - family1 pri≈0 `learned-burst-combo/source-single-attack` → 突击, 一段击杀, 二段击杀
  - family2 pri≈1 `area-stun` → 惊骇术, 魄散九霄
  - family3 pri≈2 `attackSpeed` → 疾风之术
  - family4 pri≈3 `evasion` → 闪躲, 闪身一形
  - family5 pri≈4 `escape` → 离影秘技
  - family6 pri≈5 `invisible` → 隐形之术, 匿影藏形
  - family7 pri≈6 `restore` → 幽冥调息
  - family8 pri≈7 `mana` → 仙灵气息
  - family9 pri≈8 `gun-mana-drain` → 瞄准射击, 连环射击, 噬灵连射
  - family10 pri≈9 `throw-ammunition` → 投掷, 疾影千手, 鬼影错手
  - family11 pri≈10 `dexterity-buff` → 风灵圣手
  - family12 pri≈11 `deaggro` → 移心忘恨
- 关键技快照（cast/cd 引擎单位）：
  - 10703 瞄准射击 | castU=0 cdU=0 durU=0 mp=0 | 使用火枪瞄准射击
  - 10848 连环射击 | castU=0 cdU=0 durU=0 mp=0 | 使用火枪连续射击二次
  - 11279 五段击杀 | castU=0 cdU=9600 durU=0 mp=0 | 刺客十五段连续技的最後一式，最具爆发力的技巧
  - 11307 伤痕措手 | castU=0 cdU=4800 durU=0 mp=0 | 神秘的二段刺杀，只要一击便可重创敌人！
  - 10719 疾风之术 | castU=10 cdU=6000 durU=600 mp=0 | 加快攻击速度[限自己使用]
  - 10631 隐形之术 | castU=20 cdU=50 durU=12000 mp=0 | 这法术可以让你隐身
  - 10930 惊骇术 | castU=5 cdU=300 durU=20 mp=0 | 瞬间将对手暂时吓晕[群体]

## 5. 药师
- 测号：`helen1` lv50 learned≈41
- **用途**：奶/甲/HOT/群疗/无敌/定身针/复活
- **开团/上场**：战前给坦甲+HOT；拉怪前神药无敌或协调天灵神药；人站远程
- **命中/条件**：友方技空选或选队友；无敌限己或全队看技能
- **间隔注**：神药无敌/天灵神药 cdU=18000；苍冥沛血 castU=60
- 画像职责（节选）：
  - family1 pri≈0 `single-heal` → 猿骨蛇胶, 虎朴龙血, 药石归一
  - family2 pri≈1 `physical-defence` → 凄魂鬼甲, 铁狱镇甲, 龙骨玄甲
  - family3 pri≈2 `hpBuff` → 阴阳龙合, 急救药和, 沌古上汤
  - family4 pri≈3 `root` → 定影灵针
  - family5 pri≈4 `hot` → 续命汤
  - family6 pri≈5 `groupHeal` → 植衣披, 神恩广泽
  - family7 pri≈6 `magicDebuff` → 普济消魔, 腾蛇弃鳞
  - family8 pri≈7 `resurrect` → 浴火重生, 九天降灵, 七魄归窍
  - family9 pri≈8 `self-invulnerability-manual` → 神药无敌, 不死药方, 天灵神药
  - family10 pri≈9 `single-light-attack` → 破光神雷, 聚顶曙光, 天罚幻光
  - family11 pri≈10 `area-light-attack` → 皓然之炫
  - family16 pri≈15 `fear-control` → 追月之惧
- 关键技快照（cast/cd 引擎单位）：
  - 11133 苍冥沛血 | castU=60 cdU=120 durU=0 mp=250 | 连续二次间隔20秒将血补满！
  - 11521 女娲战甲 | castU=50 cdU=50 durU=18000 mp=300 | 创世女神“女娲”，增加防御同时增加化元
  - 10884 续命汤 | castU=30 cdU=20 durU=80 mp=30 | 每秒恢复一次生命力[四次]
  - 10523 神药无敌 | castU=10 cdU=18000 durU=200 mp=86 | 短时间暂时刀枪不入[限自己使用]
  - 11135 天灵神药 | castU=20 cdU=18000 durU=200 mp=250 | 短时间暂时刀枪不入[队伍全员]
  - 10532 神恩广泽 | castU=120 cdU=120 durU=0 mp=250 | 一次将生命力恢复到满[队伍全员]
  - 10859 定影灵针 | castU=20 cdU=20 durU=200 mp=14 | 固定敌人使他无法移动。
  - 10055 浴火重生 | castU=80 cdU=80 durU=0 mp=60 | 复活指定尸体并恢复一成经验值

## 6. 修罗医师（巫医）
- 测号：`helen` lv52 learned≈47
- **用途**：减速缠身、持续毒、破防、增伤溃体、护驾；不是主奶
- **开团/上场**：定身后立刻上减速+破防+DOT；护驾给主C/奶
- **命中/条件**：负面打在敌目标；护驾打友方槽
- **间隔注**：缠身类 cdU≈10；护驾 cdU=900
- 画像职责（节选）：
  - family1 pri≈0 `slow` → 毒蛊缠身, 万蛊缠身
  - family2 pri≈1 `continuous-hp-mp-poison` → 尸蛊毒, 咒蛊毒, 魂蛊毒
  - family3 pri≈2 `continuous-hp-dot` → 千蛊化魂, 万邪毒降, 洫血蟥食
  - family4 pri≈3 `defDown` → 降血化气, 气淤弱体
  - family5 pri≈4 `damage-amplification` → 溢散溃体, 万痛聚身, 抱残守缺
  - family6 pri≈5 `attackDown` → 瘀筋滞脉, 断筋蚀骨
  - family8 pri≈7 `temporary-guardian` → 怨灵护驾, 鬼道护驾, 斗尊护驾
  - family9 pri≈8 `combined-self-attributes` → 强魄术, 修罗降世, 唤心术
  - family10 pri≈9 `mobility` → 五虎追风, 神行咒, 灵步咒
  - family11 pri≈10 `physical-offence` → 怒魔咒, 鹰眼咒, 鬼影咒
  - family12 pri≈11 `speed-debuff` → 降伏意念, 克速缓攻
  - family15 pri≈14 `recovery` → 神清气爽, 精气百倍, 充沛加倍
- 关键技快照（cast/cd 引擎单位）：
  - 10743 毒蛊缠身 | castU=20 cdU=10 durU=800 mp=8 | 移动速度变慢
  - 10862 万蛊缠身 | castU=40 cdU=10 durU=3000 mp=50 | 移动速度变慢
  - 10102 降血化气 | castU=50 cdU=50 durU=1200 mp=74 | 降低目标物的防御能力
  - 10626 溢散溃体 | castU=50 cdU=50 durU=1200 mp=112 | 使目标物的受伤加倍
  - 11150 巫蛊毒咒 | castU=30 cdU=80 durU=600 mp=200 | 持续流失"生命力"与"法力"并造成"生命与法力"回复速度下降!!
  - 10901 魔神护驾 | castU=30 cdU=900 durU=0 mp=450 | 五十七级护驾
  - 11505 元神出窍 | castU=30 cdU=900 durU=0 mp=550 | 召唤魔礼海元神

## 7. 阴阳师
- 测号：`nana` lv60 learned≈47
- **用途**：定身控场+倒转放大器+四系法伤；罩身反弹；传送另册
- **开团/上场**：自buff→定身→站定倒转→四系；禁移动打断倒转
- **命中/条件**：定身/法伤需敌选中；倒转是自放放大器
- **间隔注**：鬼影定身 cast/cd U=30；倒转 U=20，移动或挨打丢失
- 画像职责（节选）：
  - family1 pri≈0 `single-root-highest-learned` → 鬼影定身, 定影灵针
  - family2 pri≈1 `strongest-single-hp-attack` → 灵动冰箭, 幻光漩流, 灭灵光环
  - family3 pri≈2 `stationary-spell-amplifier` → 倒转阴阳
  - family4 pri≈3 `wind-single-alternative` → 风疾爆袭, 旋风薪荣, 风翼垂天
  - family5 pri≈4 `lightning-single-alternative` → 坎离雷电, 天翔奔雷
  - family6 pri≈5 `magic-reflection` → 金刚护脉, 佛印罩脉
  - family7 pri≈6 `mana-regeneration` → 三藏净身, 仙水净身
  - family8 pri≈7 `strongest-area-hp-attack` → 尽灭暗道, 太阴神火, 寒冰星雨
  - family9 pri≈8 `area-root` → 地灵束缚, 瘫痪大地
  - family10 pri≈9 `physical-reflection` → 火神罩身, 烈焰罩身, 火灵罩身
  - family11 pri≈10 `hp-capacity` → 霞云护身
  - family12 pri≈11 `mp-capacity` → 佛光霞帔, 霞称云扥, 霞威光现
- 关键技快照（cast/cd 引擎单位）：
  - 10837 鬼影定身 | castU=30 cdU=30 durU=850 mp=60 | 固定敌人使他无法移动。
  - 10987 倒转阴阳 | castU=20 cdU=20 durU=12000 mp=50 | 强化攻击系法术威力[一旦移动或被攻击到将会失去效果]
  - 11318 冰锋箭锥 | castU=60 cdU=60 durU=0 mp=400 | 寒冽的水气凝成无数的冰锋化做箭锥直射而来。
  - 10741 炽炎波 | castU=20 cdU=30 durU=0 mp=112 | 模仿翱翔於冥界的凰凤所遗落片羽之术
  - 10214 火神罩身 | castU=20 cdU=20 durU=36000 mp=300 | 能够将自身的物理伤害反弹一定的比例给敌人[[队伍全员]
  - 10836 瘫痪大地 | castU=20 cdU=80 durU=400 mp=86 | 固定敌人使他无法移动。[自己周围]

## 8. 祭符师
- 测号：`nana1` lv50 learned≈43
- **用途**：召宠战斗线：召→疗宠/魔化→宠输出；睡控；制符吸血管线另页
- **开团/上场**：战前召高阶宠；人远程；宠可协助；制符/召尸不进开团主循环
- **命中/条件**：宠技需已召宠物；法伤单点需敌选中
- **间隔注**：幻影撒旦 castU=90 cdU=0；夜叉 cast/cd U=100
- 画像职责（节选）：
  - family1 pri≈0 `combat-pet` → 业焰, 冰袭, 残风
  - family2 pri≈1 `pet-heal` → 灵疗宠物, 治疗宠物, 御疗宠物
  - family3 pri≈2 `pet-attribute-buff` → 强化防御, 强化攻击, 魔化宠物
  - family4 pri≈3 `petSpeed` → 强化速度
  - family5 pri≈4 `sleep-control` → 南柯一梦, 梦魇睡意
  - family6 pri≈5 `mana-regeneration` → 法力加速, 灵力加速, 魔力加速
  - family7 pri≈6 `reclaim` → 回收宠物
  - family8 pri≈7 `singleDamage` → 灵动冰箭, 坎离雷电, 幻光漩流
  - family9 pri≈8 `attack-speed` → 风神之威, 蹑影追风, 风影之疾
  - family10 pri≈9 `group-mana-support` → 天露甘泉
  - family11 pri≈10 `fear` → 避日之恐
  - family12 pri≈11 `cleanse` → 仙音净化
- 关键技快照（cast/cd 引擎单位）：
  - 11336 幻影撒旦 | castU=90 cdU=0 durU=0 mp=750 | 召唤宠物幻影撒旦。
  - 10833 夜叉 | castU=100 cdU=100 durU=0 mp=600 | 召唤出一只符鬼之王-夜叉
  - 11140 御疗宠物 | castU=50 cdU=50 durU=0 mp=200 | 治疗宠物生命力专用
  - 10817 魔化宠物 | castU=40 cdU=10 durU=12000 mp=150 | 强化宠物所有能力[限定宠物]
  - 10058 回收宠物 | castU=20 cdU=20 durU=0 mp=6 | 回收宠物并回收召唤生物所使用的七成法力
  - 10711 幻光漩流 | castU=80 cdU=80 durU=0 mp=86 | 模仿「太古之初」第一道光波，用以使敌人遭受极大伤害
  - 10806 南柯一梦 | castU=30 cdU=30 durU=600 mp=60 | 使目标入睡

## 与推荐栏 / 挂机的关系
- 设置「推荐」= `LegacyQuickMagicRoleProfile` 职责序，不是使用频次 AI。
- 阴阳现码把倒转抬到定身前；**实战/验收序**仍是定身→倒转→四系（见 t421）。
- 祭符推荐偏宠疗→回收→召唤；制符不进挂机 page0。
- 魔刀大量 `*-manual-choice`：挂机勿盲切形态；开团由人先叠再打。

## 证据路径
- DENOM：`../SKILL-DENOM.json`
- 推荐优化：`../RECOMMEND-AND-TEST-OPT-20261101.md`
- RoleProfile：`/workspace/swdol-box-backend/srcbuild/client/src/ui/LegacyQuickMagicRoleProfile.h`
- lookup：`/workspace/skills-onhand/swdol-test-efficiency/references/magic-role-lookup.jsonl`
