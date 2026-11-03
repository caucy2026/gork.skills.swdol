# ROSTER-TEN-MAN-r2 · 十人两队（表算重编 · 禁纸面职配 · 禁GM）

- 时间：2026-11-01 20:31:51 Asia/Shanghai
- 取代：`ROSTER-TEN-MAN-r1`（r1 纸面缺口见 §作废）
- 真源：`FULL-DUEL-SIM-TABLE-r1` · `HATE-AI-GROUNDING-r1` · `SIMULATION-HONESTY-r1` · `ROSTER-NO-GM-r1` · `raid-live/full-books.json` · `players-from-backend-binding.json`
- 机器摘要：`ROSTER-TEN-MAN-r2.json`
- **VERDICT：ROSTER_R2_TABLE_GROUNDED** — A 钉死；B 抽灵书钉；B 坦/控 OPEN（boyzyw/sxsxzzz6 纸面座已作废）；F1 未闭环

## 硬约束（表算已证）

1. **无药师职业仇恨偏置**；奶必须 SAFE；禁叠圈堆奶（F2）
2. **圈内≤4**；十人叠圈 → 啸+物理实质团灭（F3）
3. **必须有武术持嘲讽**；尽可能落实 **第二武术换坦（F1）**
4. **A 倒前换嘲**；禁双坦同时 10513（F5）
5. 主路径：**控场 10837 → 有纸 10712/10821 否则 11125**；诱啸降权
6. 禁 GM 授书；禁纸面职配（无书不写可上场）

## Team A · 主攻抽灵队（VERIFIED_CORE）

| 职责 | 账号 | 职 | 关键技 | 状态 |
|---|---|---|---|---|
| **坦克** | jiuyue | 武术家 | 10513@31 / 10954@33 / 10852@30 / 10536@34 / 10850@0 | VERIFIED_LIVE_TAUNT |
| **奶** | helen1 | 药师 | 10856@15 弱奶；**全程 SAFE** | VERIFIED_SAFE_ONLY · NO_INVULN |
| **控** | nana | 阴阳师 | 10837@37 / 10836@38 / 10628@50 | VERIFIED_BOOK · LIVE soft-fail |
| **抽灵** | nana1 | 祭符师 | 10712@6 / 10821@8 / 10815@9 | VERIFIED_BOOK · 6044=0 GATED |
| **远程** | A88888 | 刺客 | **11125@4** / 10703 / 10848 | VERIFIED_LIVE_AUX（mpΔ=−19） |

圈内默认：坦短触 + 控 +（有纸则抽｜无纸则刺客）≤3；可选 helen 破防凑第 4。**helen1 永不进圈。**

## Team B · 轮换候补（MIXED · 按书重编）

| 职责 | 账号 | 职 | 书/状态 | 职责说明 |
|---|---|---|---|---|
| **坦克（F1）** | **helen3/slot0** 风之云 | 武术家 lv33 | **OPEN_BOOK**（binding 有；full-books ABSENT） | 第二武术换坦主候选；须核 10513 |
| └ 降级 | boyzyw | 武术候选 | **TAUNT_ABSENT_CONFIRMED**（有 10852/10850，**无**10513/10954） | r1 主候选 **作废**；不可换嘲 |
| **奶** | helen2/**slot1** 风之痕 | 药师 | 10856 等；NO_INVULN | **SAFE only**；勿用 full-books 里 slot0 魔刀书当奶 |
| **控** | **jiujiu**（主 OPEN） | 阴阳 | books ABSENT | 须核 10837；**sxsxzzz6=ROOT_ABSENT 作废** |
| **抽灵** | **ariesfei** | 祭符 | **10712/10821/10815 已钉** | 有纸接力；无纸不空放 |
| **远程** | m4ak83 | 刺客 | 有 10703；**无 11125** | PARTIAL；辅抽仍靠 A88888 |

## F1 · 第二武术换坦协议

1. 盯 jiuyue HP（表算：物理中位≈4179，5771→残≈1592）
2. **倒前**：已学 10513 的 B 武术短进，**仅 B** 放嘲讽
3. A 禁止再 10513（防 F5 双嘲抖目标）
4. 现状阻断：**池内无第二账号 full-books 已证 10513** → OPEN-F1-HELEN3-S0 优先核活

## 相对 r1 改了什么

| 项 | r1 | r2 |
|---|---|---|
| B 坦 | boyzyw 纸面武术候补 | boyzyw **嘲讽缺书作废**；主候选 **helen3/s0 OPEN** |
| B 控 | sxsxzzz6 纸面定身 | sxsxzzz6 **无 10837 作废**；主候选 **jiujiu OPEN** |
| B 抽 | ariesfei UNVERIFIED | **ariesfei 书钉**（仍 6044 门） |
| B 远程 | 暗示可辅抽 | m4ak83 **无 11125** 标明 PARTIAL |
| 奶位 | 两队奶未强调 SAFE/禁堆 | **强制 SAFE**；明确堆奶≠减仇恨 |
| 换坦 | 一句协议 | **F1 逐步 + 阻断写死** |
| 标签 | 偏「可执行齐活」 | **TABLE_GROUNDED / OPEN 诚实** |

## r1 纸面缺口作废清单

| ID | 作废内容 | 依据 |
|---|---|---|
| V1 | boyzyw 可换坦 | full-books 无 10513/10954 |
| V2 | sxsxzzz6 可定身接力 | full-books 无 10837/10836 |
| V3 | B 五职纸面齐可轮换 | HONESTY H8；禁纸面职配 |
| V4 | 多加药师防团灭/先打奶 | 无职业偏置；F2 叠奶增尸 |
| V5 | helen2 full-books=奶书 | 该条目实为 slot0 魔刀 |
| V6 | OPTIMAL=已证控稳不团灭 | 短探针≠LIVE_FULL_DUEL |
| V7 | 可无第二武术先开打 | F1 软炸团风险 |

## 仍 OPEN · 账号核活（优先序）

1. **OPEN-F1-HELEN3-S0**：helen3/slot0 是否已学 10513/10954
2. **OPEN-CTRL-JIUJIU**（及 helen3/s1）：是否 10837
3. **OPEN-TANK-ALTS**：mz / 948102 / zhuzhu7125 / … 谁有武术+嘲讽
4. **OPEN-PAPER-6044**：非 GM 补符纸
5. **OPEN-LIVE-10837-STABLE**：定身稳 RESOLVE
6. **OPEN-RANGED-11125**：B 侧谁能辅抽

## 防团灭规则（摘要）

- R1 圈内≤4；R2 同时仅一坦 10513；R3 奶必须 SAFE  
- R5 禁十人叠圈；R6 主路径控→抽；诱啸降权；R10 禁 GM；R12 F1 未证不粉饰  

## 一句

A 队按真书/短验钉死并强制奶 SAFE、圈≤4、F1 换嘲；B 队 ariesfei 抽灵已钉，**boyzyw/sxsxzzz6 纸面座作废**，第二武术与 B 控仍待核活页。


---

## OPEN 核活回写（2026-11-01 20:35:25 Asia/Shanghai · ROSTER-B-BOOKS-r2）

证据：`raid-live/ROSTER-B-BOOKS-r2.md` · `.json` · `raid-live/full-books.json`（已合并 helen3 / helen3#slot1 / jiujiu）  
方法：LIS 只读 + 软登 0x0283 扫栏；**禁 GM LearnMagic/Give**

| # | 原 OPEN | 结果 | YES/NO | 路径 |
|---|---|---|---|---|
| 1 | helen3/s0 是否 10513/10954（F1） | **已学** 10513@31 / 10954@33（另有 10852@30 / 10536@34） | **YES** | `raid-live/ROSTER-B-BOOKS-r2.md` → full-books `helen3` |
| 2a | helen3/s1 是否 10837 | 活页 17 技，**无** 10837/10836 | **NO** | 同上 `helen3#slot1` |
| 2b | jiujiu/s0 是否 10837 | 活页 11 技，**无** 10837/10836 | **NO** | 同上 `jiujiu` |
| 3a | 任号 6044 符纸 | 指定池+全库 inventory 键 **qty=0** | **NO**（CLOSED_ABSENT） | LIS `legacy-compat/inventory/v1/*` |
| 3b | B 侧 11125 | m4ak83 **无**；仅有 10703@2 等 | **NO** | ROSTER-B-BOOKS-r2 / full-books `m4ak83` |

### 编制状态变更

- **B.tank=helen3/s0**：`OPEN` → **`CLOSED_YES_F1_TAUNT`**（可落实换嘲协议；仍禁与 jiuyue 同时 10513；LIVE 换嘲短验仍 OPEN）
- **B.control**：主候选 jiujiu / helen3/s1 → **`CLOSED_NO_10837`**；B 控座 **仍缺定身**（STILL_OPEN 查 alts 或仅靠 A.nana）
- **6044**：`OPEN` → **`CLOSED_ABSENT_0`**（祭符主抽继续 GATED）
- **B 辅抽 11125**：`OPEN/PARTIAL` → **`CLOSED_NO_ON_B`**

### 仍 OPEN

1. **OPEN-CTRL-B-ALTS**：谁还有 10837 可作 B 控  
2. **OPEN-LIVE-10837-STABLE**：A.nana 定身稳 RESOLVE  
3. **OPEN-F1-LIVE-SWAP**：helen3 嘲讽换坦活体短验  

一句：F1 第二武术书页 **YES**；B 定身候选与 B·11125、6044 均为 **NO/缺**。


---

## 10837 全池扫描 + F1 短验回写（2026-11-01 20:39:37 Asia/Shanghai）

证据：`raid-live/SCAN-10837-POOL-r1.md` · `raid-live/HELEN3-TAUNT-SWAP-r1.md`

### 10837 持有者

| 账号 | slot | 10837@ |
|---|---|---|
| nana | 0 | 37 |

**B.control = GATED**（除 A.nana 外无人 10837）。

降级方案（仅 A.nana 控）：
- B 队不设控位常驻（或改 flex SAFE）
- 定身窗仅 A.nana 10837；CD/拒放则停抽撤 SAFE，不硬进圈
- 圈内≤3（坦短触+抽/刺客+可选破防），为 nana 留位
- 禁第二人无定身冒充控

### helen3 换嘲短验

- SKIPPED：relogin_taunt_pos:login:auth-status:-2

## F1 helen3 换嘲短验（本轮）

- 时间：2026-11-01 20:40:38 Asia/Shanghai
- verdict：`FAIL_NO_RESOLVE`
- resolve_10513：False
- hate_path：UNPROVEN
- 证据：`raid-live/HELEN3-TAUNT-SWAP-r1.md`
- B.control：仍 **GATED**（全池仅 nana 持 10837）；编制降级 A.nana-only

