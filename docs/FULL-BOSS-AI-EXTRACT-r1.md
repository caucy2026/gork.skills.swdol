# FULL-BOSS-AI-EXTRACT-r1 · 真龙王后台 AI 全量抽离

- 时间：2026-11-01 20:29:46 Asia/Shanghai
- 权威源码树：`/workspace/swdol-box-backend/srcbuild-main-425afa9f/server/src/monster/`（Map tip 对齐 `srcbuild-main-425afa9f`）
- Boss：role **2009144** / Object **20748** / combat 接 `npccombat_2009144.json` + `boss-object-magic-slots.txt`
- 配套：`SIMULATION-HONESTY-r1` · `FULL-DUEL-SIM-TABLE-r1`
- CONFIRMED_BUG=0；本文件只抽离不改业务码

## 1. 状态机（闲置→记恨→选敌→施法→物理→追击/脱战）

```
[IDLE]
  │ 有人造成伤害/法术仇恨 → CalHate / addHateCore
  │ （retaliationRequested → plan3290=4 + Plot0/Plot5）
  ▼
[HAS_THREAT]
  │ MapTick: legacyAdvanceMonsterRetaliationTick
  │   hasThreat? → combat.selectTarget → ThreatTable::select (CheckHate434E0F)
  │   无威胁? → advanceSearchCounter → selectProactiveTarget（搜敌）
  ▼
[SELECTED_TARGET]
  │ deferPursuitToPlots? → queueSearchEnemyPlots(Plot0+Plot5)
  │ else → pursue / Follow → attackRange? → completeAttackAttempt（物理钟）
  ▼
[SPELL_ATTEMPT]（与追击并行调度，见 Runtime.selectSpell）
  │ legacySelectNpcSpell：50% randomSkip；低血偏 slot0；否则 slot∈[4,15]
  │ cooldown / definition / resources（hp,mp ≥ 2×cost）
  │ legacyDispatchNpcSpell 路由 currentTarget / self / …
  ▼
[CAST → HIT → DAMAGE]
  │ legacyCheckMagicHit → CalMagicDamageRate / SpellHPMPPercent
  │ AOE：targetRule∈{1,3,5} 自中心；11102 area=2000 收集合集
  ▼
[PURSUE / LOSE]
  │ Follow dist≥5000 → clearMovement（不清 selected）
  │ 目标死亡/无同 section → clearMovement；威胁槽保留（不自动 ReturnHome）
```

## 2. ThreatEntry / ThreatCandidate 数据结构

### 2.1 ThreatEntry（`ThreatTable.h`）

| 字段 | 类型 | 含义 |
|---|---|---|
| connectionIdentity | int64 | 连接绑定（非选敌主键） |
| sourceRoleId | int64 | 耐久身份 / 候选 map key |
| contribution | int64 | 贡献累计（可被 suppressContribution 跳过） |
| threat | int64 | **仇恨值**（选敌用） |
| sourceRuntime3174 | int32 | 活 Actor+3174；-1=未绑定 |
| rewardRoleId | int64 | 奖励角色 |

容量：`legacyThreatCapacity = 15`。**无 job / healer / class 字段。**

### 2.2 ThreatCandidate（选敌输入）

| 字段 | 含义 |
|---|---|
| connectionIdentity | 身份校验 |
| distance | 平面距（hypot Δx,Δz） |
| relationScore | 阵营/友好分（≥0 参与系数） |
| canAttack / lineClear / controlDetected | 门闸 |
| hateTopology | 可选线闸 |
| nativeRuntime3174 | 活 Actor |

**无职业偏置字段。**

### 2.3 addHateCore（CalHate 表突变）

路径：`ThreatTable.cpp` `addHateCore` ← `LegacyCalHate` / `legacyCalHateWithOwner`。

1. amount==0 → reject  
2. victimHpBefore2008<0 → adjusted += victimHp  
3. (victimFlags|sourceFlags)&0x40000 → adjusted=0  
4. adjusted<0 → reject  
5. 找槽：空槽或同 runtime3174/sourceRoleId；满则替换 contribution+threat 最小槽（初值 minTotal=100000）  
6. threat += adjusted；若 !suppressContribution → contribution += adjusted  
7. retaliation：victim 未攻击或跟随中等门闸 → retaliationRequested；contributionBonus=min(adjusted*5, maxHp/5)  
8. 若 threat > threshold 且 !ownerGuarded → selectedRoleId = source（更新 threshold）

嘲讽/伤害进表：**同一 addHate 路径**；嘲讽具体 rawAmount = OPEN（须 Magic 定义/回执）。

### 2.4 selectCore（CheckHate@434E0F）— **无药师偏置**

对每个 threat>0 槽：

```
denominator = distance/50 + 20
score = (relation * monsterLevel + 20) * threat / denominator   // native DWORD 溢出语义
```

- score > selectedScore → 候选换人（需 lineClear/hateTopology）  
- score ≤ 且已有人选 → `rand%10==0` 才换（约 10%）  
- controlDetected==false → **整次 abort**，不换到下一槽  
- retainedCurrentTarget：选中==当前 controller 且正在攻击 → returnedRoleId=-1（抑制重复 Plot）

**结论：先打谁 = 威胁分最高；药师不会因为是药师被优先。** 药师若被点，只因：她在表里 threat 高、距离近抬分、或当前目标保留。

## 3. LegacyNpcSpellSelector（全文逻辑）

文件：`LegacyNpcSpellSelector.cpp`（CheckAndSetNPCSpell@4358C0 同源）。

### 3.1 legacySelectNpcSpell

1. HP 非法 → invalidHitPoints  
2. `rand1%2==0` → **randomSkip**（≈50% 不施法）  
3. `lowHp = (currentHp*3 < maximumHp*2)` 即 HP<2/3  
4. 若 `rand2%4==0 && lowHp` → **slot=0**（另耗一次 rand）  
5. 否则 `slot = rand3%12 + 4` ∈ **[4,15]**  
6. magicId=magicIds[slot]；<0 → missingMagic；!definitionExists → definitionMissing；cooldown 未好（signed jle）→ cooldownActive  
7. 否则 selected

### 3.2 legacyDispatchNpcSpell（目标路由）

- !resourceAllowed → 拒  
- runtime 未激活：仅 flags&3 且 &0x1000 可走 self/petOwner  
- 激活且 !(flags&0x1000) → **currentTarget**  
- flags&0x20 → selfSpecial（需 specialPrecheck）  
- else self / petOwner / searchedHelpTarget  

资源门：`hp>=2*hpCost && mp>=2*mpCost`。

### 3.3 真龙王槽位常数（Object npcMagicSlots17e）

| slot | magicId | 名 | range | area | mp | hpPct | flags20 | targetRule | AI 可达 |
|---|---|---|---|---|---|---|---|---|---|
| 0 | 11101 | 龙鸣泽水 | 1200 | 0 | 40 | +100 | 0x11020 | 3 | 仅低血分支 |
| 2,3 | 11078 | 力抗山河 | 100 | 0 | 0 | 0 | 0x11008 | 3 | AI 不选 2/3 |
| 4,5 | 10536 | 天露赋畀 | 100 | 0 | 0 | 0 | 0x11008 | 3 | 是 |
| 6–9,14 | -1 | — | | | | | | | missingMagic |
| **10,13** | **11098** | **龙奔三界** | **6000** | 0 | **0** | **-200** | 0x4 | 0 | 是 |
| **11,12** | **11102** | **龙啸九天** | **2000** | **2000** | **200** | **-888** | 0x20 | **5** | 是 |
| 15 | 10735 | 招唤目标 | 12000 | 0 | 0 | 0 | 0x20 | 4 | 是 |

条件选中后（已过 50% skip）均匀 slot∈[4,15]：  
P(10536)=2/12，P(11098)=2/12，P(11102)=2/12，P(10735)=1/12，P(missing)=5/12。  
11102 另需 mp≥400（2×200）；MP=25332 时资源通常过。

## 4. MapTick 选敌 + 追击调度

`legacyAdvanceMonsterRetaliationTick`（`LegacyMonsterMapTick.cpp`）：

1. 组装 candidates（alive、sameSection、persistentRoleId>0）  
2. distance = hypot(dx,dz)；relationScore 来自输入  
3. hasThreat → `selectTarget`；否则搜敌门 → proactive  
4. 无目标：不清 selected；仅清移动意图（不自动回出生点）  
5. deferPursuitToPlots：绑 controller / queue Plot0+5 后 return  
6. 否则 Follow；在攻击距离且 attackReady → completeAttackAttempt

物理：`LegacyNpcPhysicalAttackExecution` → `ownPhysicalDamage`（CombatMath）。

追击上限：`pursue` 中 dist≥**5000** → followDistanceExceeded。

## 5. AOE area=2000 如何选中集

1. 选主目标：仇恨 currentTarget（11102 flags20=0x20 **无** 0x1000 → dispatch 走 currentTarget 分支）  
2. 中心：`LegacySpellAoeCenterSwap` — **targetRule=5 ∈ {1,3,5} → 自中心（Boss xz）**  
3. 命中集：距中心 ≤ **area=2000** 的合法实体（简化引擎 `SpellExecutionEngine`：`distance(primary,candidate)<=areaRange`；与自中心语义在 rule5 下等价于「围 Boss ≤2000」）  
4. SAFE≈51790 ≫ 2000/6000/12000 → **SAFE 不进任何四点/招唤结算**

## 6. 成功率 / 伤害核（LegacyCombatMath）

### 6.1 legacyCheckMagicHit（摘要）

- 空间准入 → immediate / directRange / randomPath …  
- 保护位：flags&0x34100000 时 20% 直接 range  
- 主阈：视 spellFlags20&0x18 — `%110` 或 `%100` vs `(target2f36 + f(levelDelta))`  
- 抗性支：`%300` vs min(200, resist[sel]+10*levelDelta)  
- 距离：distanceInt ≤ rangeB8+range8c（除非 unbounded）

### 6.2 CalMagicDamageRate

`rate = ((roll20 + 3*skill/2 + 30) * (100 - min(resist,75))) / 200`（ignoreResistance 时 resist 当 0）  
再经 base30 / casterScaled / post 得到 finalDelta；百分比技另走 SpellHPMPPercent。

### 6.3 ownPhysicalDamage

`physicalDamageCore`：命中阈、攻防差、angle 系数、weightAc 等（PRE-SIM 已用 weightAc=380 算出十人分布）。  
Boss 2f18=14537 vs 非坦防御 → 中位一击死（九月风正面可残）。

## 7. CalSpellHate

`legacyCalSpellHateDecision`：对非玩家目标用 Magic+150 作 amount，suppressContribution=true；flags&0x1000 可把仇恨转到 controller（距≤500）。**玩家被 Boss 打不走这条「给 NPC 加仇」**；玩家打 Boss 走普通 CalHate。

## 8. 物理核相关文件

- `LegacyNpcPhysicalAttackExecution.h` → ownPhysicalDamage  
- `LegacyMonsterAttackClock` 攻击间隔  
- `legacyCheckAttackRange` / `legacySetMoveVector`

## 9. OPEN

- 10513/10954 写入 threat 的精确 rawAmount  
- relationScore 在玩家 vs 真龙王 的运行时取值（npccombat 有 relation_0..；Map 适配映射 OPEN）  
- Plot650/653 与施法调度的精确交织时序（tick 量子）  
- 11102 与定身互斥的完整 status 表  
- 见招拆招 id  

**VERDICT: SOURCE_EXTRACT_COMPLETE_FOR_CORE_AI — 选敌/记恨/施法/AOE/命中伤害核已钉；逃跑全支与嘲讽量仍 OPEN。**
