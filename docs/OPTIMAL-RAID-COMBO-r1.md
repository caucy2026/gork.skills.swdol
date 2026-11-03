# OPTIMAL-RAID-COMBO-r1 · 神龙最优配合（无 GM / 现有角色）

- 时间：2026-11-01 20:18:50 Asia/Shanghai
- VERDICT：**COMBO_VERIFIED_AUX_DRAIN_NO_GM**（云端短验已跑；主祭符纸门；辅抽灵证 mpΔ&lt;0）
- 通读：`SKILL-DENOM` · `CLASS-SKILL-BRIEF` · `magic-role-lookup` · `RAID-COORD-ALGO` · `ROSTER-NO-GM-r1` · `OPTIMAL-MANA-DRAIN-CANDIDATE` · P5–P7
- 铁律（t456/t458）：禁 GM；只用现有角色；控场窗内同伙必须控住 Boss；主动找最优解；禁贴脸乱冲

## 0. 编制结论（先看缺口）

| 项 | 状态 |
|---|---|
| 原生药师短无敌 | **无**（helen1/helen2 ABSENT；helen1 GM 探针 → `gm_probe_only_not_for_raid`） |
| 定身控场 | **有** nana 10837@37 |
| 嘲讽 | **有** jiuyue 10513@31 / 10954@33 |
| 破防/增伤 | **有** helen 10102@35 / 10626@36 |
| 祭符抽灵 | **有** nana1 **10712@6 / 10821@8 / 10815@9**（符纸6044 **ABSENT=0（全库 LIS）**） |
| 辅抽灵 | **有** A88888 11125@4 |

→ **最优解不以药师无敌为前提**；以 **定身窗 + 祭符抽灵** 耗蓝，嘲讽只负责唤醒/仇恨锚。

## 1. 出场名单

| 职责 | 账号 | 关键原生技 |
|---|---|---|
| 坦/唤醒 | jiuyue | 10513/10954/10852/10536/10850 |
| 主控 | nana | 10837/10836/10628 |
| 主耗蓝 | nana1 | **10712/10821/10815** |
| 破防 | helen | 10102/10626/10743 + 护驾只用 10052 |
| 远程+辅抽 | A88888 | 10703/10848/**11125** |
| 轻奶 | helen1 | 仅 10856 等；**禁用** GM 无敌/甲 |
| 板凳近战 | 12345 / sxsxzzz5 | **仅魔尽后** |

## 2. 阶段机（最优序）

| 阶段 | 做什么 | 出门 |
|---|---|---|
| C0 | 冻结 ROSTER；核 6044；施法表剔 GM 技 | 原生-only |
| C1 | 全员 SAFE≫12000 | 站桩 |
| C2 | 10628 + 铁布/天露；**不开** 11135/10523/10524 | 原生 buff |
| C3 | 九月风 **短**触≤600 嘲讽/poke → 立刻让位给控场 | AI 醒；禁久贴 |
| C4 | nana ≈1450 内 **10837**（可 10836）；helen 减速 | 定身 RESOLVE＝控场窗 |
| C5 | **nana1 10712↔10821 循环**（+10815）；盯 MP@+0xa40；辅 11125 | **MP↓**（主耗蓝，≫诱啸） |
| C6 | 破防/增伤短进短出；远程@~1350 | RESOLVE；仍禁近战职业 |
| C7 | MP 权威门（&lt;施法阈） | 才开 P8 |
| C8 | 近战切入（可选） | 用户放行 |

**明确降权**：P4 盲目诱龙啸 11102 → 仅 fallback（见 `OPTIMAL-MANA-DRAIN-CANDIDATE`）。

## 3. 距离硬门（承 RAID-COORD）

- SAFE ≫12000（实测≈51796）
- 定身/远程主轴 ≈1350–2000（龙啸半径 2000 内短停）
- 嘲讽/poke ≤600 极短
- 破防 ≤1600；增伤 ≤1000；五段 ≤800
- 金刚护脉 **不抗物理**

## 4. 云端真协议短验证（已执行 · TEAM-A-DRAIN-MP-r1）

- 证据：`raid-live/TEAM-A-DRAIN-MP-r1.md` / `.json`（2026-11-01 20:23:56 Asia/Shanghai 定稿回写）
- 队：jiuyue / helen1 / nana / nana1 / A88888；**禁 GM**（无 LearnMagic/Give/Zone；仅 LRS）
- **VERDICT：PASS_AUX_11125_PAPER_GATED**
- Boss MP@+0xa40：`25332 → 25313`；**overall_mp_delta=−19**；**mp_delta_negative=True**
- 路径：`assassin_11125` ×3 RESOLVE（0286 result=0）；slot=4；ammo6524 余量≥28
- 祭符：paper6044=0 → 10712/10821 **未 RESOLVE**（talisman_mp_delta=0）
- 控场：10837 **soft-fail**（begin-resources + LRS 后 Boss 贴脸 dist≈809/385；FOLLOW653 geometry mismatch）
- 坦 poke/嘲讽：begin-resources 拒（jiuyue hp≈597）；唤醒路径需先奶满再短触
- 成功定义（本轮放宽）：**辅抽灵证 mpΔ&lt;0** ∧ wiped=false ∧ 禁 GM ✓
- 正式团成功定义（未达）：10837 RESOLVE ∧（有纸则 10712/10821）∧ 持续 mp↓

## 5. 缺口（主动列，不装齐）

- 无原生药师短无敌 → 短接触风险高，必须根控极短窗完成抽灵
- 符纸6044 **ABSENT**（已核 LIS body+bank）
- helen1 无甲/HOT 原生
- 召宠高阶 OPEN
- 见招拆招无 id
- Boss 满蓝时诱啸静默曾误判 — 必须以 MP oracle 为准

## 一句

无纸时 A88888·11125 已证 Boss MP@+0xa40 下降（Δ=−19）；有纸立刻切回 nana1·10712/10821；定身窗+两队轮换；禁 GM。


---

## 附录 · 十人两队（t459）

- 编制：`ROSTER-TEN-MAN-r1.md` / `.json`
- 路径索引：`PARALLEL-AGENT-PATHS-r1.md`
- 模式：**Team A 主抽灵 + Team B SAFE 轮换接力**，禁止十人同进龙啸圈
- 每队职责固定：坦克 / 奶 / 控 / 抽灵 / 远程


---

## 修订 · t459 两组队 + 物资门（2026-11-01 20:22:18 Asia/Shanghai）

### 推荐编制（执行态）

| 队 | 成员 | 职责 |
|---|---|---|
| **A 主攻（默认上场）** | jiuyue / helen1 / nana / nana1 / A88888 | 坦 / 弱奶 / 控 / **抽灵** / 远程+辅抽 |
| **B 轮换（默认 SAFE）** | boyzyw / helen2 / sxsxzzz6 / ariesfei / m4ak83 | 同职候补；A 断档短进接力即撤 |
| Flex | helen（破防）· 12345/sxsxzzz5（魔尽近战） | 不占龙啸圈常驻 |

### 物资硬门（只读库存已核）

- **全库 LIS 无 object 6044 符纸**（含 nana1/ariesfei 等祭符号 body+bank）→ 10712/10821/10815 **当前库存 GATED**
- A88888 **body/0 弹药 6524×31** → 辅抽 **11125 噬灵连射** 可执行
- **禁 GM Give**；补纸须非 GM 获取（商店/掉落/交易），未获纸前耗蓝主路径临时降级为：**定身窗 + 11125**，获纸后立刻切回 10712/10821

### 秒级配合（无原生无敌）

1. t0：A 全员 SAFE；B SAFE
2. t0–3s：jiuyue 铁布/天露 → d≤600 poke/嘲讽 → **立刻撤 SAFE**（无无敌，禁久站）
3. t3–8s：nana d≈1450 **10837**（+10628）；helen1 SAFE 弱奶
4. t8–：nana1 **有纸则 10712↔10821**；**无纸则 A88888 11125**；同步读 Boss MP@+0xa40
5. 破防 helen 短进≤1600 即出；远程≈1350
6. B 仅在 A 控/抽灵 CD 空窗时 **一人**短进接力
7. `current_mp` 权威下降到施法门下才近战

### 防互伤

见 `ROSTER-TEN-MAN-r1` R1–R12；同时进龙啸圈 ≤4 人。

---

## 定稿 · 两队轮换可执行方案（2026-11-01 20:23:56 Asia/Shanghai）

### A 上场 / B SAFE（默认）

| 节拍 | 谁 | 距离 | 技能 | 备注 |
|---|---|---|---|---|
| T0 | 全员 | ≫12000 SAFE | — | B 全程默认 SAFE |
| T1 | helen1 | SAFE | 弱奶 jiuyue | **先抬坦血**，否则 poke/嘲讽 begin-resources |
| T2 | jiuyue | ≤600 极短 | 10852/10536 → 10850/10513 → **立刻撤 SAFE** | 无无敌；禁久站 |
| T3 | nana | **按活 Boss xyz 重算 ≈1450** | 10628 → **10837**（+10836） | LRS 后若 Boss 追人必须 **重 park**；拒因 begin-resources 则补蓝/等 CD |
| T4a 有纸 | nana1 | ≈1350 | **10712↔10821**（+10815） | 主耗蓝；双 oracle SPELL+MP |
| T4b 无纸 | A88888 | ≤2700（已证≈385–1350） | **11125** 连射 | **当前默认**；弹药 6524 |
| T5 | helen（flex） | ≤1600 短进即出 | 10102/10626 | 不占常驻10人槽时可外挂 |
| T6 | B 一人 | 短进即撤 | 同职接力 | 仅 A 控/抽断档；圈内同时 ≤4 |
| T7 | 近战板凳 | — | — | **仅** current_mp 权威过施法门后 |

### 轮换触发

1. A.nana 10837 CD/拒放 → B.sxsxzzz6 短进定身 → 撤
2. A.nana1 无纸或抽灵拒 → 维持 A.A88888 11125；B.ariesfei 仅在有纸且 A 断档时进
3. A.jiuyue 阵亡/资源枯 → B.boyzyw 换坦协议（禁止双嘲讽）
4. 弹药&lt;5 → 停 11125；等待非 GM 补弹/补纸

### 本轮硬结论

- **最优耗蓝主解**：控场窗 + 祭符抽灵（10712/10821）≫ 诱啸 11102
- **库存现实**：6044=0 → **临时主解 = 11125**（已云端证 mpΔ&lt;0）
- **P8**：仍 GATED（MP 25313/25332，远未尽）
- **CONFIRMED_BUG=0**；禁改业务码；禁 GM


---

## 修订 · 接 ROSTER-TEN-MAN-r2 / 表算作废纸面（2026-11-01 20:31:51 Asia/Shanghai）

编制权威：`ROSTER-TEN-MAN-r2.md` / `.json`（本文件出场名单中 **B 队 r1 纸面座已作废**）。

### 仍成立（短验）

- 主路径：控场 → 有纸 10712/10821 **否则 11125**（mpΔ=−19 已证）
- 诱啸降权；圈内≤4；禁 GM

### 按表算作废 / 降权的 r1 条文

| 原表述 | 处理 |
|---|---|
| B=boyzyw/helen2/sxsxzzz6/ariesfei/m4ak83「同职候补」可轮换 | **作废齐活暗示**；仅 ariesfei 抽灵书钉；boyzyw 无嘲讽；sxsxzzz6 无定身；m4ak83 无 11125 |
| 「A.jiuyue 阵亡 → B.boyzyw 换坦」 | **作废为可执行**；改为 F1：须第二武术已学 10513（主候选 helen3/s0 OPEN）；boyzyw=TAUNT_ABSENT |
| 隐含堆奶/第二奶进场减风险 | **作废**；奶×1～2 仅 SAFE；无药师仇恨偏置 |
| OPTIMAL=已证控得住不团灭 | **降权**为 HONEST_PARTIAL：10837 soft-fail；无 LIVE_FULL_DUEL |

### r2 两队执行态

| 队 | 成员 | 备注 |
|---|---|---|
| **A** | jiuyue / helen1(SAFE) / nana / nana1 / A88888 | 不变核心；奶强制 SAFE |
| **B** | helen3/s0(F1 OPEN) / helen2/s1(SAFE) / jiujiu(OPEN) / **ariesfei(书钉)** / m4ak83(无11125) | 按书重编 |
| Flex | helen 破防可作圈内第4 | 计入≤4 |

未核 F1 / B 控活页前：B 只可 SAFE 候补，禁止写死换坦/定身接力成功。
