# TEN-COMBOS-SCORED · 十套退神龙组合打分

- **stamp**：2026-11-03 13:15:40 Asia/Shanghai（Asia/Shanghai 箱钟）
- **引擎**：honest_parallel · dt=100ms · ticks≤2000 · wall≤18.0s · seed=0x1234567e
- **书档权威**：`raid-open-r1/ROSTER-TEN-MAN-r2.json` `book_truth_r2` / `teams`；禁发明 6044/学技
- **paper6044**：`CLOSED_ABSENT_0` evidence=`LIS kv full sweep ROSTER-B-BOOKS-r2` → **全库 any=0**
- **Boss 招唤10735**：已改为**拽玩家**（非刷怪）；证据 FULL-BOSS-AI「点名拉近」+ Object handler=21 flags20=0x20
- **能退硬门**：`final_mp<=0`；若全员否 → 仍给相对分，标 能退=NO + GATE_DOCUMENTED

## 计分公式（透明）

```
composite=35*min(party_drain_mp/25332,1)+(12 if first_root)+8*min(root_hits/10,1)+20*(1 if first_root else 0.25)*max(0,1-min(dbr,5)/5)*max(0,1-min(deaths,8)/8)+15*min(drain_hits/40,1)+(6 if not wipe)+4*min(alive_n/10,1)+5*min(chase/50,1); party_drain from timeline drain_mp_delta_sum (NOT Boss 11102 self-spend)
```

## 排名总表

| Rank | ID | 组合 | 分 | 能退 | final_MP | 队伍抽蓝 | 总ΔMP含Boss自耗 | 首定身ms | 定身前死 | 总死 | drain命中 | root | wipe | 存活 | yank | 为何不能退 |
|---:|---|---|---:|:---:|---:|---:|---:|---:|---:|---:|---:|---:|:---:|---:|---:|---|
| 1 | C09 | A坦=helen3（jiuyue作B） | 55.5 | N | 23027 | 505 | 2305 | 8000 | 0 | 2 | 30 | 15 | False | 6 | 0/3 | 纸门6044=0 + 11125抽速不足 |
| 2 | C07 | 控场更近开场（nana@3500） | 53.2 | N | 23210 | 522 | 2122 | 2700 | 1 | 3 | 31 | 15 | False | 6 | 0/3 | 纸门6044=0 + 11125抽速不足 |
| 3 | C01 | OPTIMAL基线 A+B（r2编制） | 52.4 | N | 23027 | 505 | 2305 | 7200 | 1 | 3 | 30 | 15 | False | 6 | 0/3 | 纸门6044=0 + 11125抽速不足 |
| 4 | C03 | 双弱奶满编 | 52.4 | N | 23027 | 505 | 2305 | 7200 | 1 | 3 | 30 | 15 | False | 6 | 0/3 | 纸门6044=0 + 11125抽速不足 |
| 5 | C02 | 无F1换嘲（剔B.tank helen3） | 52.0 | N | 23027 | 505 | 2305 | 7200 | 1 | 3 | 30 | 15 | False | 5 | 0/3 | 纸门6044=0 + 11125抽速不足 |
| 6 | C04 | 无奶（helen1+helen2出局） | 51.6 | N | 23027 | 505 | 2305 | 7200 | 1 | 3 | 30 | 15 | False | 4 | 0/3 | 纸门6044=0 + 11125抽速不足 |
| 7 | C10 | 无祭符位（nana1+ariesfei出局） | 51.6 | N | 23027 | 505 | 2305 | 7200 | 1 | 3 | 30 | 15 | False | 4 | 0/3 | 纸门6044=0 + 11125抽速不足 |
| 8 | C08 | 核心三人（仅 jiuyue+nana+A88888） | 44.0 | N | 24427 | 505 | 905 | 7200 | 1 | 3 | 30 | 15 | True | 0 | 0/0 | 纸门6044=0 + 11125抽速不足 |
| 9 | C05 | 无辅抽11125（剔A88888噬灵） | 42.9 | N | 24132 | 0 | 1200 | 7200 | 1 | 2 | 0 | 14 | False | 7 | 0/3 | 纸门6044=0 + 11125抽速不足 |
| 10 | C06 | 无定身（disable_root） | 13.1 | N | 18532 | 0 | 6800 | None | 2 | 2 | 0 | 0 | False | 7 | 1/18 | 无定身 |

## 各组合书档引用（CLOSED）

### 1. C09 A坦=helen3（jiuyue作B） · 分=55.5
- brief：换嘲号当主坦；书 CLOSED 有10513，面板HP可能更脆（binding）
- source claim：helen3_slot0 CLOSED_YES_HAS_TAUNT
- 能退：**N**；gate：GATE_DOCUMENTED: paper6044=CLOSED_ABSENT_0; B.control GATED_NO_B_ROOT (no second 10837); 11125 LIVE≈-19/hit insufficient to 0 within wipe/window; no invented combat regen / 6044
- 指标：首定身=8000ms；队伍抽蓝=505；总ΔMP=2305；drain_hits=30；deaths_before_root=0；yank=0/3
- 组件分：`{'mp_score_party_drain': 0.7, 'root_score': 20.0, 'surv_score': 15.0, 'drain_score': 11.25, 'alive_score': 8.4, 'chase_score': 0.2}`
- 运行目录：`runs/20261103-ten-combos/C09/`

### 2. C07 控场更近开场（nana@3500） · 分=53.2
- brief：缩短走位延迟，提高首定身前被龙奔/招唤点杀风险
- source claim：10837 range=2500 Object；开场几何变体
- 能退：**N**；gate：GATE_DOCUMENTED: paper6044=CLOSED_ABSENT_0; B.control GATED_NO_B_ROOT (no second 10837); 11125 LIVE≈-19/hit insufficient to 0 within wipe/window; no invented combat regen / 6044
- 指标：首定身=2700ms；队伍抽蓝=522；总ΔMP=2122；drain_hits=31；deaths_before_root=1；yank=0/3
- 组件分：`{'mp_score_party_drain': 0.72, 'root_score': 20.0, 'surv_score': 10.0, 'drain_score': 11.62, 'alive_score': 8.4, 'chase_score': 2.5}`
- 运行目录：`runs/20261103-ten-combos/C07/`

### 3. C01 OPTIMAL基线 A+B（r2编制） · 分=52.4
- brief：jiuyue嘲+nana定身+A88888噬灵+nana1纸门+helen1弱奶；B helen3换嘲+helen2弱奶+ariesfei纸门
- source claim：ROSTER-TEN-MAN-r2 teams + OPTIMAL-RAID-COMBO-r1
- 能退：**N**；gate：GATE_DOCUMENTED: paper6044=CLOSED_ABSENT_0; B.control GATED_NO_B_ROOT (no second 10837); 11125 LIVE≈-19/hit insufficient to 0 within wipe/window; no invented combat regen / 6044
- 指标：首定身=7200ms；队伍抽蓝=505；总ΔMP=2305；drain_hits=30；deaths_before_root=1；yank=0/3
- 组件分：`{'mp_score_party_drain': 0.7, 'root_score': 20.0, 'surv_score': 10.0, 'drain_score': 11.25, 'alive_score': 8.4, 'chase_score': 2.1}`
- 运行目录：`runs/20261103-ten-combos/C01/`

### 4. C03 双弱奶满编 · 分=52.4
- brief：保留 helen1+helen2；其余同基线
- source claim：helen1/helen2 10856 VERIFIED_SAFE_ONLY
- 能退：**N**；gate：GATE_DOCUMENTED: paper6044=CLOSED_ABSENT_0; B.control GATED_NO_B_ROOT (no second 10837); 11125 LIVE≈-19/hit insufficient to 0 within wipe/window; no invented combat regen / 6044
- 指标：首定身=7200ms；队伍抽蓝=505；总ΔMP=2305；drain_hits=30；deaths_before_root=1；yank=0/3
- 组件分：`{'mp_score_party_drain': 0.7, 'root_score': 20.0, 'surv_score': 10.0, 'drain_score': 11.25, 'alive_score': 8.4, 'chase_score': 2.1}`
- 运行目录：`runs/20261103-ten-combos/C03/`

### 5. C02 无F1换嘲（剔B.tank helen3） · 分=52.0
- brief：同C01但 helen3 出局 → A坦死后无第二10513
- source claim：book_truth helen3 CLOSED_YES；本组合主动EXCLUDE测F1缺失
- 能退：**N**；gate：GATE_DOCUMENTED: paper6044=CLOSED_ABSENT_0; B.control GATED_NO_B_ROOT (no second 10837); 11125 LIVE≈-19/hit insufficient to 0 within wipe/window; no invented combat regen / 6044
- 指标：首定身=7200ms；队伍抽蓝=505；总ΔMP=2305；drain_hits=30；deaths_before_root=1；yank=0/3
- 组件分：`{'mp_score_party_drain': 0.7, 'root_score': 20.0, 'surv_score': 10.0, 'drain_score': 11.25, 'alive_score': 8.0, 'chase_score': 2.1}`
- 运行目录：`runs/20261103-ten-combos/C02/`

### 6. C04 无奶（helen1+helen2出局） · 分=51.6
- brief：测弱奶对定身前存活贡献
- source claim：10856 VERIFIED；本组合EXCLUDE奶位
- 能退：**N**；gate：GATE_DOCUMENTED: paper6044=CLOSED_ABSENT_0; B.control GATED_NO_B_ROOT (no second 10837); 11125 LIVE≈-19/hit insufficient to 0 within wipe/window; no invented combat regen / 6044
- 指标：首定身=7200ms；队伍抽蓝=505；总ΔMP=2305；drain_hits=30；deaths_before_root=1；yank=0/3
- 组件分：`{'mp_score_party_drain': 0.7, 'root_score': 20.0, 'surv_score': 10.0, 'drain_score': 11.25, 'alive_score': 7.6, 'chase_score': 2.1}`
- 运行目录：`runs/20261103-ten-combos/C04/`

### 7. C10 无祭符位（nana1+ariesfei出局） · 分=51.6
- brief：纸门本就6044=0；确认去掉祭符不改变能退门
- source claim：paper6044 CLOSED_ABSENT_0
- 能退：**N**；gate：GATE_DOCUMENTED: paper6044=CLOSED_ABSENT_0; B.control GATED_NO_B_ROOT (no second 10837); 11125 LIVE≈-19/hit insufficient to 0 within wipe/window; no invented combat regen / 6044
- 指标：首定身=7200ms；队伍抽蓝=505；总ΔMP=2305；drain_hits=30；deaths_before_root=1；yank=0/3
- 组件分：`{'mp_score_party_drain': 0.7, 'root_score': 20.0, 'surv_score': 10.0, 'drain_score': 11.25, 'alive_score': 7.6, 'chase_score': 2.1}`
- 运行目录：`runs/20261103-ten-combos/C10/`

### 8. C08 核心三人（仅 jiuyue+nana+A88888） · 分=44.0
- brief：其余账号出局；最小可证抽灵三角
- source claim：三账号书均 VERIFIED；EXCLUDE其余
- 能退：**N**；gate：GATE_DOCUMENTED: paper6044=CLOSED_ABSENT_0; B.control GATED_NO_B_ROOT (no second 10837); 11125 LIVE≈-19/hit insufficient to 0 within wipe/window; no invented combat regen / 6044
- 指标：首定身=7200ms；队伍抽蓝=505；总ΔMP=905；drain_hits=30；deaths_before_root=1；yank=0/0
- 组件分：`{'mp_score_party_drain': 0.7, 'root_score': 20.0, 'surv_score': 10.0, 'drain_score': 11.25, 'alive_score': 0.0, 'chase_score': 2.1}`
- 运行目录：`runs/20261103-ten-combos/C08/`

### 9. C05 无辅抽11125（剔A88888噬灵） · 分=42.9
- brief：A88888 仍在场但 STRIP 11125；纸抽仍6044=0不放
- source claim：A88888 11125 VERIFIED_LIVE；strip 仅本组合
- 能退：**N**；gate：GATE_DOCUMENTED: paper6044=CLOSED_ABSENT_0; B.control GATED_NO_B_ROOT (no second 10837); 11125 LIVE≈-19/hit insufficient to 0 within wipe/window; no invented combat regen / 6044
- 指标：首定身=7200ms；队伍抽蓝=0；总ΔMP=1200；drain_hits=0；deaths_before_root=1；yank=0/3
- 组件分：`{'mp_score_party_drain': 0.0, 'root_score': 20.0, 'surv_score': 12.0, 'drain_score': 0.0, 'alive_score': 8.8, 'chase_score': 2.1}`
- 运行目录：`runs/20261103-ten-combos/C05/`

### 10. C06 无定身（disable_root） · 分=13.1
- brief：nana书有10837但不放 — 对照抽灵窗
- source claim：nana 10837 VERIFIED；disable_root 组合开关
- 能退：**N**；gate：GATE_DOCUMENTED: paper6044=CLOSED_ABSENT_0; B.control GATED_NO_B_ROOT (no second 10837); 11125 LIVE≈-19/hit insufficient to 0 within wipe/window; no invented combat regen / 6044
- 指标：首定身=Nonems；队伍抽蓝=0；总ΔMP=6800；drain_hits=0；deaths_before_root=2；yank=1/18
- 组件分：`{'mp_score_party_drain': 0.0, 'root_score': 0.0, 'surv_score': 2.25, 'drain_score': 0.0, 'alive_score': 8.8, 'chase_score': 2.0}`
- 运行目录：`runs/20261103-ten-combos/C06/`

## 全局诚实结论

- **十套全部 能退=NO**（GATE_DOCUMENTED）。
- 根因：`paper6044=CLOSED_ABSENT_0`（全账号 LIS）；B 无第二 10837（`GATED_NO_B_ROOT`）；11125 单次 LIVE≈−19，未到耗尽窗。
- 相对最优看排名第1：更早定身 / 更多 drain_hits / 更少定身前死亡 / 非全灭。

## 10735 yank 遥测（本板）

| combo | yank_hits/attempts | 主因 |
|---|---|---|
| C01–C05,C07,C09,C10 | 0/3 | resolve 时 currentTarget 已死且存活者多在 SAFE>12000 → `targetDead_noLivingInRange` |
| C08 | 0/0 | 三人团较早灭，未抽到 10735 |
| C06 | **1/18** | 无定身窗更长；首次 hit dist=7290 → teleport ASSUMED boss+80 |

目的地偏移 **ASSUMED** +80（精确脚位 OPEN）。尸体改点标 `ASSUMED_alive_hate_in_range_when_currentTarget_dead`。

## ABSENT / GATED（不计已学分）

| 项 | 状态 | 源 |
|---|---|---|
| paper6044 全账号 | CLOSED_ABSENT_0 any=0 | ROSTER-TEN-MAN-r2 book_truth_r2 |
| 第二 10837（B.control） | ABSENT / GATED_NO_B_ROOT | helen3_slot1 / jiujiu CLOSED_NO_ROOT |
| m4ak83 11125 | ABSENT（loader 无此技） | book_truth m4ak83 CLOSED_NO_11125 |
| nana1/ariesfei 10712/10821 | 书有但 6044=0 → **不放** | VERIFIED_BOOK_GATED_PAPER |
| C05 STRIP 11125 | 本组合主动剥离，score drain=0 | combo strip_skills |
| boyzyw 10513 | TAUNT_ABSENT_CONFIRMED（未编入十人） | book_truth_r2 |

## 证据路径

- `/workspace/swdol-smoke/algo-hate-20261031-r10/fork6-inject-r58/allclass-magic/raid-open-r1/ROSTER-TEN-MAN-r2.json`
- `.../FULL-BOSS-AI-EXTRACT-r1.md` · `call_target_10735`
- `dual-model-src/magic-defs.json` ← Object.dbf `swdol_legacy_magic_object_audit all-magic`
- `runs/20261103-ten-combos/BOARD.json` · `BOOKS-CITED.json`

