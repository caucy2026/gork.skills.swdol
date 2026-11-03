# HARD_GATE · sim-root-mp-gate · GATE_DOCUMENTED

- **stamp**：2026-11-03 12:30 Asia/Shanghai（箱钟）
- **status**：`GATE_DOCUMENTED` — 魔尽（party-drain Boss MP→0）在 CLOSED 证据下不可达
- **sim stamp 参照**：`runs/20261103-122823-stopmp0/`（wall 11.80s，`STOP_TICKS`，MP→270，deaths=0）
- **权威根**：`/workspace/swdol-smoke/algo-hate-20261031-r10/fork6-inject-r58/allclass-magic/raid-open-r1/`

## Unblocker 检索（仅证据，未发明）

| # | 候选 | 结果 | 证据路径 |
|---|---|---|---|
| 1 | 符纸 **6044** qty>0（nana1 / A 队 / 全库） | **CLOSED_ABSENT_0** | `ROSTER-TEN-MAN-r2.json` `paper6044.status=CLOSED_ABSENT_0`；`raid-live/ROSTER-B-BOOKS-r2.json` inventory 全槽 `paper6044:0`；`raid-live/roster-b-books-r2.stdout` `paper_any:0` `broad_6044:[]`；`OPTIMAL-RAID-COMBO-r1.json` `paper6044:0`；`raid-live/TEAM-A-DRAIN-MP-r1.json` `paper6044_nana1:0` |
| 2 | 战中 party MP 回复率 / 蓝药 qty | **OPEN 无数值**；LIS 无蓝药计数 | 战中率：raid-open-r1 **无** in-combat tick 率。仅证脱战 `advanceNpcResourceRecovery`（`SAFE-DRAIN-BOSS-MP-r1.json` offset_proof）。库存 sweep 只钉 6044/6524；nana/nana1 `items_n=0`；A88888 仅 `ammo6524=25..31`（`ROSTER-B-BOOKS-r2` / TEAM-A-DRAIN）。CLASS-SKILL-BRIEF 有「灵力加速」等族名，**无**本编制已学+回复率 CLOSED 数 |
| 3 | 第二 10837 持有者 | **CLOSED_NO**（holders=1，beyond_nana=0） | `raid-live/SCAN-10837-POOL-r1.md`；`ROSTER-TEN-MAN-r2.json` `root_10837_scan` / B.control `CLOSED_NO_10837` |
| 4 | 其它 CLOSED 抽灵 | 仅 **11125** 可跑 | 10712/10821/10815 **书有、纸无** → GATED（同 #1）。主路径文档：`OPTIMAL-RAID-COMBO-r1.md` current_stock=`assassin_11125` |

**结论：无可接线 CLOSED 解锁项。** 不启用 10712/10821；不发明回蓝；不加人定身。

## 精确算术（控场蓝池 vs 抽灵需求）

### 控场消耗（CLOSED Object）

| 量 | 值 | 来源 |
|---|---|---|
| nana `max_mp` | **5508** | sim roster / loader |
| 10837 `mp_cost` | **60** | `magic-defs` Object.dbf |
| 最大起手次数 | \(\lfloor 5508/60\rfloor=\)**91** | 算术 |
| 实测起手 | **91** 后 `root_10837_oom` 海量 | `runs/20261103-122744-10k` timeline |
| 理论命中率 | \(0.46\times155/300\approx\)**23.77%** | flags20&0x18；2f36=39 resist5=95 Δlv=5 |
| 实测命中 | attempts 91 / hits **10**（含 42 unlock cancel）；resolve 命中≈20.4% | 同 run |
| 平均定身时长 | **≈23327 ms** | CLOSED AssistDuration 采样 |
| 定身窗合计 | `rootedSkip`=**1144** ticks ≈ **114.4 s** 后永久解锁 | stop-mp0 100k 同种子封顶 |

无 CLOSED 战中回蓝 → **91 次后控场永久停机**。

### 抽灵吞吐（CLOSED 11125 路径）

| 量 | 值 | 来源 |
|---|---|---|
| 唯一可跑抽灵 | A88888 **11125** | paper 门；弹药 6524≥25 |
| 每击 MPΔ（实测均） | **≈ −16.97** | drain_sum −662 / 39 hits |
| CD | 20u×100ms=**2 s**（cooldown_units=0 时 stand-in 20u） | party_model |
| 命中率（本盘） | 39/58≈**67.2%** | summary |
| 定身窗内有效抽灵速率 | \((1/2)\times0.672\times16.97\approx\)**5.70 MP/s** | 算术 |
| 实测 party 总抽 | **−662** | 定身窗耗尽即停 |
| Boss 终值 | **270**（自耗蓝地板） | 11102×122×200mp 等；非抽穿 |

### 乐观上界（仍不够）

假设 91 起手全无 cancel、按 CD=3s 连锁刷新、定身几乎不掉：

- 日历跨度 ≈ \(90\times3 + 23.3 \approx\) **293 s** 近连续定身  
- 乐观 party 抽 ≈ \(5.70\times293\approx\) **1670 MP**  
- Boss 池 **25332**；乐观抽 **≪ 6.6%** 池  
- 即使 Boss 先自耗到 270：还需在控场 **OOM 前** 把残余抽完；本盘 OOM 时窗只抽到 −662，之后无定身无法再进 1350 抽灵带  

**故 MP→0 不可达。** stop-mp0 100k：`final_boss_mp=270`，从未 `STOP_MP_GATE`。

## 已排除的假解锁

- 发明 paper qty / 战中回蓝率 / 第二 10837  
- Map/C++/GM 补纸补书补药  
- 把 Boss 自耗到 270 记成 party 魔尽  

## 标记

```
sim-root-mp-gate = GATE_DOCUMENTED
魔尽 = NOT_REACHED
unblockers_closeable = 0
```

