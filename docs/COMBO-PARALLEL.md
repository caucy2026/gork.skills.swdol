# COMBO-PARALLEL · 全队配合 + 并行时钟诚实表算

- **stamp**：2026-11-03 12:50 Asia/Shanghai（箱钟）
- **verdict**：`HONEST_PARALLEL_CLOCK` · 瞬移 SAFE **已废除** · 死亡为合法结果
- **权威编制**：`raid-open-r1/OPTIMAL-RAID-COMBO-r1` · `ROSTER-TEN-MAN-r2` · `CLASS-SKILL-BRIEF` · `FULL-BOSS-AI-EXTRACT-r1`
- **引擎**：SimPy dual-model · `honest_parallel=True` · dt=100ms · 墙钟≤20s · 无 Map/C++/GM

---

## 0. 用户三问（t498u）直答

| 问 | 答 | 本盘是否接线 |
|---|---|---|
| Boss 会不会**走向**人？ | **会**。FULL-BOSS-AI：选敌后 Follow 追击；`dist≥5000` → `followDistanceExceeded` | **已接** `BossModel.tick_chase`（步长 `nowMoveSpeed×6` CLOSED；rank ASSUMED 8） |
| 人走近会不会死？ | **会**。未定身时进 ≤2000（龙啸）/≤6000（龙奔）/近战均可能被结算 | **已接**；禁止瞬移出圈 |
| 召唤 / 回血算法用了吗？ | **10735**：拽**当前仇恨玩家**到 Boss（CLOSED 点名拉近）；**非**刷怪。**10536**：self_buff 无 hp%→不回血（OPEN）。战中 regen **不发明** | 10735 已接线 yank；10536/regen 仍 OPEN |

---

## 1. 两队职责与配合顺序（理论 COMBO，非「全员瞬移保命」）

### Team A（主轴）

| 职责 | 账号 | 职业 | 本盘开场站位 | 要做什么 |
|---|---|---|---|---|
| 坦/唤醒 | jiuyue / 九月风 | 武术家 | **400**（短触后） | 10513 嘲讽唤醒 → **走路**撤向 HOLD≈6500（耗时；途中仍在 6000 龙奔内） |
| 主控 | nana / 娜娜 | 阴阳师 | **8000**（>6000） | **走路**进 ≤2500 → 读条 10837（1.6s）→ 命中后刷新；脚锁读条 |
| 辅抽 | A88888 | 刺客 | **SAFE≈51790** | 仅定身窗内**走路**进 ≈1350 放 11125；解锁应走回 SAFE |
| 主抽（纸） | nana1 | 祭符师 | SAFE | 书有 10712/10821/10815；**6044=0 → 本盘不放** |
| 弱奶 | helen1 | 药师 | SAFE | 10856；须走近目标 ≤1600（走路）；禁 GM 无敌 |

### Team B（轮换/冗余）

| 职责 | 账号 | 状态 | 作用 |
|---|---|---|---|
| 换坦 | helen3 | 有 10513 | F1：A 坦危急且已定身时走近换嘲（走路） |
| 弱奶 | helen2 | SAFE | 冗余抬血 |
| 控场 | （空） | **CLOSED_NO_10837** | 全库仅 nana 有鬼影定身 |
| 祭符 | ariesfei | 纸门同 A | 无纸不进 |
| 远程 | m4ak83 | **无 11125** | 不能替辅抽 |

**圈规**：同时进龙啸圈（≤2000）≤4；B 不与 A 抢圈。

**阶段序（OPTIMAL C1–C5，并行理解）**：

1. C1 全员远距 SAFE  
2. C2 buff（10628/铁布/天露）— 表算未全开 buff 表（OPEN）  
3. C3 坦短触嘲讽 → 走撤（**并行**：Boss 已醒，开始选敌/读条/追击）  
4. C4 控场从圈外走入读定身（**并行**：走+读条期间 Boss 可杀人）  
5. C5 定身窗内抽灵（11125；有纸才 10712/10821）

---

## 2. 每个魔法：谁 / 干什么 / 何时

### 2.1 我方（编制书内）

| ID | 名 | 谁 | 效果（证据） | 本盘 |
|---|---|---|---|---|
| **10513** | 嘲讽挑衅 | jiuyue / helen3 | Special16 仇恨 12000 CLOSED；range=400；cd=4s | **用**：开场嘲讽 |
| 10954 | 嘲讽狂舞 | 同上 | 群体嘲讽；cast 0.5s | 书有；表算主轴用 10513 |
| 10852 | 铁布杉 | 坦 | 自防御 buff | 书有；状态表 OPEN |
| 10536 | 天露赋畀 | 坦（玩家）/ Boss 槽 | 自 buff；**非**百分比抬血 | Boss 侧 OPEN self_buff |
| 10850 | （踢/百分比） | 坦 | hpPercent=-25 | 开场短触可选；未作主轴 |
| **10837** | 鬼影定身 | **仅 nana** | 定身；LIVE cast **1600ms**；CD 3s；AssistDuration；双门命中≈24% | **用** |
| 10836 | 瘫痪大地 | nana | 自周围定身 | 备选；主用 10837 |
| 10628 | 金刚护脉 | nana | 自 buff | C2；表算未强制 |
| **11125** | 噬灵连射 | A88888 | restoreMagicCore mp%−100；LIVE Δ≈−19；bound range 2700 | **用**（定身窗） |
| 10703/10848/11279 | 瞄准/连环/五段 | A88888 | 物理输出 | 魔尽前禁近战；本盘不主用 |
| 10712/10821/10815 | 祭符抽灵族 | nana1/ariesfei | mp 大额百分比 | **GATED** paper6044=0 |
| **10856** | 弱奶（药师） | helen1/2 | hpPercent=+150；range 1600；mp12 | 可走位施放 |
| 10102/10626/10743 | 破防/增伤/减速 | helen（flex） | 定身后上负面 | flex；本盘未强制 APL |

### 2.2 Boss（真龙王 2009144）

| ID | 名 | range | 关键 | 本盘 |
|---|---|---|---|---|
| **11098** | 龙奔三界 | 6000 | hp% **−200**；cast_units=20→**2.0s** 读条；mp0 | **用**；开场杀死九月风 |
| **11102** | 龙啸九天 | 2000 / AOE2000 | hp% **−888** + mp%；cast0 **瞬发**；mp200 | **用**；157.4s 双杀 nana+A88888 |
| **10735** | 招唤目标 | 12000 | handler21 拽玩家 | **CLOSED 点名拉近**（非刷怪）；hp%=0；杀靠贴脸物理/AOE |
| 10536 | 天露赋畀 | 自 | self_buff | 计数；**不回血** OPEN |

### 2.3 移动 / 追击（并行时钟）

| 项 | 公式/门 | 状态 |
|---|---|---|
| 步长 | `nowMoveSpeed × 6` / 100ms 切片 | **CLOSED** `legacySetMoveVector` |
| 玩家 rank | 15 | **ASSUMED**（无完整 AGI/负重 ValueCal）→ ≈90 u/tick |
| Boss rank | 8 | **ASSUMED**（npccombat `move_speed_source_v1` 头字段 / NPC 默认）→ ≈48 u/tick |
| 追击上限 | 5000 | **CLOSED** FULL-BOSS-AI |
| 读条锁脚 | 施法中不走路 | **OPEN**（简化） |

---

## 3. 开场并行时间轴（0–30s，主跑 `runs/20261103-parallel-3k`）

同一 `t_ms` 上队伍走路 / 读条 / Boss 读条·追击·出手 **同时推进**（不再瞬移 SAFE）。

| t_ms | 队伍 | Boss | 几何/血量 |
|---|---|---|---|
| **0** | jiuyue **10513 嘲讽命中**；开始走撤 HOLD；nana 从 **8000** 向 2300 走 | 选中坦；**11098 起手读条 2.0s**；**物理一刀 4385**（坦 5771→1386） | 坦 400；娜娜 8000 |
| 100–1900 | 坦外撤（仍 <6000）；娜娜持续走近（≈90/tick） | 11098 **casting**；**chaseStep** 追坦 | 坦距被追击拉锯上升 |
| **2000** | — | **11098 RESOLVE** hpΔ=−1386 → **jiuyue 阵亡** | 首杀；距≈1240 |
| 2100–5500 | 娜娜继续走近 | 对尸体空放/另选；11102/10536 穿插 | 坦死 |
| **5600** | 娜娜 **抵达 2300**；**10837 起手**（mp−60） | （可能仍在读条） | 进入龙奔半径 |
| **7200** | **10837 RESOLVE 命中**；root≈32.1s | 进入 **rootedSkip** | **first_root_ms=7200** |
| 7200–30000 | 刷新定身；A88888 从 SAFE 走入抽灵带 | 定身窗内跳过敌对施法/物理 | 九月风保持死亡 |

**定身前 Boss 有效伤害击中**：物理 4385 + 龙奔收刀 −1386 → 坦死（`boss_hits_before_first_root_n=2`）。

**对比旧谎**：旧盘瞬移 SAFE/HOLD→法术 OOR 空挥→「全员活着等五次定身」。本盘 **2.0s 内坦死**，与用户质疑一致。

---

## 4. 新跑次结果（诚实并行）

| 配置 | 路径 | 墙钟 | first_root | 死亡 | Boss 击中(定身前) | chase_steps | 终 MP | 抽灵 |
|---|---|---|---|---|---|---|---|---|
| 有定身 500 | `runs/20261103-parallel-500/` | 0.05s | **7200** | jiuyue@2.0s | 2 | 20 | 25132 | 0（窗短） |
| 有定身 3k | `runs/20261103-parallel-3k/` | 0.43s | **7200** | jiuyue@2.0s；**nana+A88888@157.4s（11102 AOE）** | 2 | 21 | 19427 | hits30 |
| 无定身 3k | `runs/20261103-parallel-3k-noroot/` | 0.40s | — | jiuyue@2.0s | 2 | 20 | 15332 | 0（无人进圈抽） |

无定身时 nana **不进圈**（disable_root），故只死坦；有定身则控/抽进圈，解锁窗被 **11102** 团灭进圈二人。

---

## 5. 旧模型缺什么 → 现在有什么

| 旧（假） | 新（并行） |
|---|---|
| 瞬移 SAFE / HOLD / 2300 | **走路** `speed×6` 耗时 |
| 同 tick cancel+闪出圈 | 取消读条仍在原距，**可被继续打** |
| Boss 不移动 | **tick_chase** 追选中目标（≤5000） |
| Boss 技能瞬发（忽略 cast_units） | **11098 等**按 cast_units×100 读条 |
| 「定身前列全活」 | **允许并出现死亡** |
| 召唤/回血装有 | **明确 OPEN**：10735 无实体；10536/战中回血无率 |

---

## 6. 仍 OPEN（诚实）

- 玩家 NowMoveSpeed 完整 ValueCal（现 ASSUMED 15）  
- Boss NowMoveSpeed 完整核（现 ASSUMED 8）  
- ProcessMove2 双分区是否每 200ms 一步（现按每 dt 一步）  
- 10735 召唤物实体与仇恨  
- 10536/战中 HP·MP 回复数值率  
- Plot650/653 与施法交织精确序  
- 玩家 GCD / 多数非 10837 读条  
- 符纸 6044 / 第二 10837（CLOSED 缺席，不是 OPEN）

---

## 7. 产物路径

- 本文：`/workspace/swdol-smoke/shenlong-sim-simpy/COMBO-PARALLEL.md`  
- 汇总：`/workspace/swdol-smoke/shenlong-sim-simpy/RESULT.md`  
- 跑次：`runs/20261103-parallel-{500,3k,3k-noroot}/`（`timeline.jsonl` · `summary.json`）


---

## 9. 十套组合打分（t501/t502）

见 **`TEN-COMBOS-SCORED.md`** / `runs/20261103-ten-combos/BOARD.json`。十套皆能退=NO；相对最优见排名表。

<!-- T502U-YANK -->
## t502u rescore · 2026-11-03 13:15:40 Asia/Shanghai

- 十套重跑：party_drain 计分；TOP **C09 55.5**；全员 能退=N GATE_DOCUMENTED
- 10735：resolve 优先 yank；C06 yank_hits=1/18；有根组合多 `targetDead_noLivingInRange`
- paper6044 CLOSED_ABSENT_0；m4ak83 11125 ABSENT；不发明书

