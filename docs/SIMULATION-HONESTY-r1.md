# SIMULATION-HONESTY-r1 · 对照用户质问逐条钉死

- 时间：2026-11-01 20:29:22 Asia/Shanghai
- 质问源：t460/t461/t462/t463（短探针≠真仿真；要后台 AI 全量 + 双方数据 + 成功率/数据结构）
- 本轮交付：`FULL-BOSS-AI-EXTRACT-r1` · `FULL-DUEL-SIM-TABLE-r1` · 本文件
- 铁律：不编造；OPEN 写 OPEN；禁 GM

## 0. 一句 verdict

**此前短探针不算全量双方仿真。** 本轮已把后台 Boss AI / 仇恨 / 施法 / 物理命中成功率 **源码抽离 + 表算对打**；**仍未**在 Map 上跑「双方完整智能逐步 tick 活体对打日志」。算 **SOURCE_EXTRACT + TABLE_DUEL**，不算 **LIVE_FULL_DUEL**。

## 1. 逐条 YES / NO / OPEN

| # | 用户质问要点 | 判定 | 证据 / 说明 |
|---|---|---|---|
| H1 | 有没有用真正后台 Boss **所有** AI 算法做仿真？ | **NO→本轮 PARTIAL** | 此前：P3-HATE-TOUCH / SAFE-DRAIN / TEAM-A-DRAIN 仅短触/抽蓝探针。本轮：已全文抽 `LegacyNpcSpellSelector` / `ThreatTable::addHateCore+selectCore` / `LegacyMonsterRuntime::selectTarget` / `legacyAdvanceMonsterRetaliationTick` / `LegacyCalHate*` / `LegacyCombatMath` 命中与伤害核。**未**把逃跑/搜敌全分支 + Plot 调度全量编成可执行仿真器。 |
| H2 | 有没有把后台数据抽离出来？ | **YES（本轮）** | 见 `FULL-BOSS-AI-EXTRACT-r1.md/.json`；槽位接 `boss-object-magic-slots.txt` + `npccombat_2009144.json`。 |
| H3 | 双方智能是否都进仿真？ | **PARTIAL** | Boss 侧：表算按抽出 AI。我方：十人 binding HP/防/已学技进表（`PRE-SIM-SURVIVAL` / `ROSTER-*`）。**我方「决策 AI」不是自动策略机**，是人工阶段脚本（OPTIMAL C0–C8）；不是代码级双方 agent。 |
| H4 | 仇恨先打谁看过没？有没有药师偏置？ | **YES 看过源码；无职业/药师偏置** | `ThreatTable::selectCore` 分数 = `(relation*monsterLevel+20)*threat / (distance/50+20)`；`ThreatCandidate` / `ThreatEntry` **无 job/healer 字段**。先打谁 = 仇恨表威胁分最高者（同分 10% 随机换），不是「先点奶」。 |
| H5 | 嘲讽怎么进表？ | **YES 路径钉死；量 OPEN** | 玩家法术结算走 CalHate（`LegacyCalHate` / Runtime addHate）；嘲讽 10513 活体曾 RESOLVE（P3/SAFE-DRAIN）。**10513 写入的 rawAmount 精确值 OPEN**（未在本轮从 Magic.dbf+150/伤害回执钉死数值）。 |
| H6 | 一个魔法会不会团灭？龙啸 area=2000？ | **YES 可团灭（表算）** | 11102 龙啸九天 area=2000、mp=200、hpPct=-888；PRE-SIM 单发中位 529–1078。低血职叠两发或「魔+物理」死；十人叠圈 = 命中集=圈内全员。 |
| H7 | 定得住 / 控制得住吗？ | **OPEN→偏 NO（现网短验）** | TEAM-A-DRAIN：10837 soft-fail；jiuyue poke 曾 begin-resources 拒（低血）。源码有定身技，**活体控稳未证**。 |
| H8 | OPTIMAL 编制是不是纸面？ | **PARTIAL 纸面** | A 队技能盘 YES（ROSTER-NO-GM）；B 队 UNVERIFIED；无原生无敌药师；符纸6044 ABSENT；「定身窗内祭符主抽」主路径 **纸面+辅抽短验**，非长时双方仿真通过。 |
| H9 | 反击算法是否一致？ | **YES 源码一致声明；活体一致性 OPEN** | 箱 `srcbuild-main-425afa9f` 的 CalHate/CheckHate/NpcSpellSelector 即运行 Map 同源树。**未**本轮对 pid300236 做 instruction-level 旁路对照。 |
| H10 | 成功率公式有没有进？ | **YES（抽离）** | `legacyCheckMagicHit` 主阈/抗性阈；`legacyMagicDamageRate`；`ownPhysicalDamage` 物理核。表算用 PRE-SIM 已算伤害分布，非口号。 |
| H11 | 本轮 FULL-DUEL-SIM-TABLE 表算是否闭环？ | **PARTIAL** | 选敌公式/施法槽/PRE-SIM 伤害团灭 **闭环**；嘲讽 rawAmount、Map relation 真值、活体 20–40 tick 日志、我方自动决策 AI **未闭环**（见 FULL-DUEL-SIM-TABLE §0）。 |


## 2. 此前算什么 / 不算什么

| 产物 | 算 | 不算 |
|---|---|---|
| PRE-SIM-SURVIVAL | 伤害数学模型 vs 十人防 | Boss 选敌逐步仿真 |
| P3-HATE-TOUCH / SAFE-DRAIN | 短触嘲讽、观测 11098/11102/10735 | 20–40 tick 目标序列 |
| TEAM-A-DRAIN-MP | 辅抽 11125 mpΔ=-19 | 全量 AI 对打 / 定身稳控 |
| OPTIMAL-RAID-COMBO | 编制与阶段建议 | 已证明「控得住+不团灭」 |

## 3. 还差哪步活体才算真仿真

1. Map 上打开 `SWDOL_TRACE_NPC_COMBAT_RECOVERY`（或等价）录 **20–40 次** `NPC_COMBAT_SELECTION` + 0286 槽位，对照本表算目标序。
2. 单坦 10513 RESOLVE 后读 ThreatEntry（或 proxy）确认 jiuyue 槽 threat。
3. 药师 SAFE vs 圈内 两组对照：证明 **不被职业优先**，仅距离/仇恨分。
4. 定身 10837 稳定 RESOLVE 窗口内再抽灵。
5. （可选）把抽出 AI 编成可回放脚本，RNG seed 固定逐步跑。

## 4. OPTIMAL 编制诚实标签

| 组件 | 标签 |
|---|---|
| jiuyue 嘲讽唤醒 | LIVE_SHORT（有 RESOLVE 证据） |
| nana 10837 定身窗 | PAPER / LIVE_SOFT_FAIL |
| nana1 10712/10821 主抽 | PAPER_GATED（6044=0） |
| A88888 11125 辅抽 | LIVE_SHORT（mpΔ=-19） |
| 两队轮换 ≤4 进圈 | PAPER_POLICY（防龙啸表算支撑） |
| Team B 五人上线 | OPEN_UNVERIFIED |

**VERDICT: HONEST_PARTIAL_SIM — 源码抽离+FULL-DUEL 表算已落（选敌/施法/团灭闭环；嘲讽量/活体 tick 未闭环）；LIVE_FULL_DUEL 未做。**
