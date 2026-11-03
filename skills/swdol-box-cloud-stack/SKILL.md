# SWDOL 箱/云端调试栈部署与验收（gork.bot 全局技能）

给**其他账号的 Grok Bot**：克隆 `github.com/caucy2026/swdol2026` 的 `gork.bot/` 后，按本技能 + `RESTORE.md` 在新箱还原与 hate-pr101 / 长阳实验等价的 lab。本技能是部署来龙去脉、端口/目录、明文密钥安装、deferred boot、公办对齐、唯一 DB、三目标测法与约束红线的权威摘要；细节脚本与 sha 以 kit 内文件为准，**禁止臆造端口/sha/路径**。

配套：`RESTORE.md` · `MANIFEST.json` · `scripts/*` · `docs/smoke/*` · `docs/constraints/*` · 兄弟技能 `swdol-box-onboard-inject`（网页定靶/导航/攻击闭环）· `skill-1793274920988`（双人原版注入）。

---

## 1. When / Who

| 适用 | 不适用 |
|---|---|
| 异账号 Grok Bot 从 git 还原**箱实验栈** | 生产云端 `root@119.96.24.110` `/opt/kemi-gm/swdol`（用 `swdol-cloud-deployment`） |
| 对齐公办 shared、修 catalog FATAL、验溜达/物法/仇恨 | 把本机 ORICO 路径当同事默认路径 |
| tip **`f11d1f6a`** 制品 + deferred PID 注册 lab（旧 3934/8d38 仅明确钉旧 tip 时可装） | 个人拼库、混源 catalog/experience、CONFIRMED_BUG=0 改业务码 |

**三目标（用户钉死）**：①长阳溜达 ②仇恨追击 ③物法算伤。**踢图**另批 TRACE/修，**不挡**三目标结论。

---

## 2. 来龙去脉：为何这样部署

1. **箱 ≠ 生产云端**  
   箱根 `/workspace/swdol-box-backend`；端口 **63418/63419/63499/63497**。生产机游戏口约 21130/21131、HTTPS 21199，路径与本技能无关。

2. **deferred PID registry（硬）**  
   Map/Ops 必须以 `deferred_registered_exec.py` **先占 PID**，探针读 `linux-startticks`，写入 `kernel-registry.private.txt`，再起 `swdol_world_database_service`，DB `WORLD_DATABASE_SERVICE_READY` 后才往 packet 放真正 argv 让 Map/Ops exec。否则节点登记 Corrupt / lease / ownership 连环炸。一键：`runtime/bin/boot-stack-deferred.sh`（kit：`boot/boot-stack-deferred.sh`，`install-bins-from-kit.sh` 会拷入 live）。

3. **公办 shared 优先于算法结论**  
   仿真统一入口 `local-shared-current.json` + `database-current/CURRENT.json` → 唯一 `world.sqlite`。箱落地 `runtime/config/shared-from-sim/` + `runtime/runtime-definitions/`。口诀：**先环境与公办一样样，再判算法**；未对齐前的静止/跳字不作算法结论。

4. **默认 tip `f11d1f6a`（2026-10-31 箱 live）**  
   全名 `f11d1f6a6accbf0956edf89573c590781f3f44fa`。Map live sha256：`7f50036826ef48f615fed2c9751d5928a0551b861f95e8e24fc3a051b1256890`；DB：`79bcaf5b7618ce338aee828de118f653ee3f121fefcfbec84974b489363ff40b`；front_probe `a140637a…`；catalog `d0d665f782…`（cold-birth PASS，REFUSED=0）。Web：`main-943667f55-r138`，client `0.6.106-dev`。旧 pin `3934d6c1`/`8d38d879` **已过时**。云电脑从零：`docs/smoke/CLOUD-BOX-SETUP-FOR-SWDOL-DEEP-ANALYSIS.md`。死亡聊天走 CLEAR6（`DEATH-PUBLICATION-DEFERRED-FIX.md`），禁瞎直改库。

5. **与生产差异摘要**  
   箱：deferred + 本地 sock `/tmp/swdol-box-db/service.sock` + 测号非 GM + 可加 stderr TRACE。生产：受管部署、不同端口/路径、禁止把箱结论直接当现网运维步骤。

---

## 3. 目录与端口地图

| 项 | 值 |
|---|---|
| 箱根 `SWDOL_ROOT` | `/workspace/swdol-box-backend` |
| Kit（克隆后） | 仓库内 `gork.bot/`；本机打包源 `/workspace/gork.bot-backup/` |
| Web 根 | `runtime/web/site/v/main-943667f55-r138/` |
| 公办落地 | `runtime/config/shared-from-sim/` |
| 定义文件 | `runtime/runtime-definitions/{Object.dbf,NPCCombat.tsv,Table.TB}` |
| DB | `runtime/data/world.sqlite`；locks：`world.sqlite.map-owner-locks` **mode 0700** |
| 真 stderr | `runtime/logs/map-defer.stderr` · `ops-defer.stderr` · `db.stderr`（不是 pid 文件） |
| DB sock | `/tmp/swdol-box-db/service.sock` |

| 口 | 用途 |
|---|---|
| **63418** | Master（同 Map 进程） |
| **63419** | Map |
| **63499** | Ops HTTP |
| **63497** | Web front-client |

`pkill -f` 须用**全路径**；勿 `pkill -x`（comm 截断 15）。停栈可保留 :63497 web。

---

## 4. 明文密钥与 `install-secrets-from-kit.sh`

用户裁决（2026-10-31）：**密钥不要脱敏**；其他 bot 仅凭拉仓即可还原相同环境。明文在 kit `secrets/`（**技能正文不写密钥内容**，只写文件名与安装路径）。

```bash
export SWDOL_ROOT=/workspace/swdol-box-backend
KIT=/path/to/gork.bot   # 或 /workspace/gork.bot-backup
bash "$KIT/scripts/install-secrets-from-kit.sh"
# → mode 600 装到 $SWDOL_ROOT/runtime/config/
```

| kit 路径 | live 路径 |
|---|---|
| `secrets/ops-hmac.secret` | `runtime/config/ops-hmac.secret` |
| `secrets/admin.private.json` | `runtime/config/admin.private.json` |
| `secrets/admin.password.local.txt` | `runtime/config/admin.password.local.txt` |
| `secrets/LAB-ACCOUNTS.txt` | 文档；测号见该文件（Ops 用 boxop） |
| `secrets/kernel-registry.private.SAMPLE.txt` | 可选；**live 由 deferred boot 重写** |

清单：`config-samples/SECRETS-WHERE.txt`。`config-samples/*REDACTED*` 对还原已过时，优先 `secrets/`。

---

## 5. 还原流程（对齐 RESTORE.md）

前提：箱上仍有大体骨架（`matched/`、`srcbuild/`、大 web 资源、`runtime-maps`）。kit **不含** ~828M 完整 web、~97M server freeze tgz、pristine 12M catalog（见 RESTORE §5）。

```bash
export SWDOL_ROOT=/workspace/swdol-box-backend
KIT=/path/to/gork.bot

# 5.1 停 Map/Ops/DB（可留 web）
pkill -f "$SWDOL_ROOT/runtime/bin/swdol_legacy_compat_server" || true
pkill -f "$SWDOL_ROOT/srcbuild/server/operations/operations_server.py" || true
pkill -f "$SWDOL_ROOT/runtime/bin/swdol_world_database_service" || true
sleep 1

# 5.2 二进制 + boot
bash "$KIT/scripts/install-bins-from-kit.sh"
# 确认 Map sha == tip live 7f500368…（见 PIN-f11d1f6a）

# 5.3 公办 shared + Object.dbf / NPCCombat / Table.TB
bash "$KIT/scripts/restore-shared-from-kit.sh"

# 5.4 DB（kit vacuum gzip 或已知 bak）
bash "$KIT/scripts/restore-world-sqlite.sh"
# 再按 docs/smoke/CLEAR-PLAN-… 清 lease/service-* kv；locks 0700 已由脚本 mkdir

# 5.5 front-client hook（仇恨刀测）或 pristine
bash "$KIT/scripts/restore-web-hook.sh" hook      # 含 SWDOL_HATE_FC_HOOK / fc-chart
# bash "$KIT/scripts/restore-web-hook.sh" pristine

# 5.6 secrets → deferred boot
bash "$KIT/scripts/install-secrets-from-kit.sh"
bash "$SWDOL_ROOT/runtime/bin/boot-stack-deferred.sh"
```

脚本一览：`install-bins-from-kit.sh` · `install-secrets-from-kit.sh` · `restore-shared-from-kit.sh` · `restore-world-sqlite.sh` · `restore-web-hook.sh`。

---

## 6. deferred boot 硬顺序 + TRACE 环境变量

### 硬顺序

1. `deferred_registered_exec` 占 **map + ops** PID → 写 `kernel-registry.private.txt`（`linux-startticks`）。  
2. 起 `swdol_world_database_service`；`--database-service-inode` = **`world.sqlite` 的 `st_dev:st_ino`**（**不是** sock inode）。boot 脚本自动 `stat` 写入 packet。  
3. 等到 `WORLD_DATABASE_SERVICE_READY`。  
4. 写 map/ops packet（含 argv + TRACE env）→ deferred 放行真正 exec。  
5. 验 `ss`：**63418/63419/63499** LISTEN（web 63497 常独立长活）。

Corrupt(code=7) / ownership lost：停栈 → 清 kv 中该 node 的 `service-directory` / `service-owner` / 相关 lease（配方 `docs/smoke/CLEAR-PLAN-stack-restart-20261031-0357.txt`）→ `map-owner-locks` 0700 → 再 boot。

### TRACE / ENABLE 清单（boot 写入 Map packet）

以 kit `boot/boot-stack-deferred.sh` 与 `boot/live-trace-env.txt` 为准：

| 变量 | 用途 |
|---|---|
| `SWDOL_ENABLE_SOURCE_NPC_BOOTSTRAP=1` | 公办对齐 NPC 源 |
| `SWDOL_ENABLE_SOURCE_SLEEP_PHASE_VM=1` | sleep phase VM |
| `SWDOL_ENABLE_SOURCE_NPC503=1` | NPC503 |
| `SWDOL_ENABLE_DURABLE_DELAYED_SOURCE=1` | durable delayed source |
| `SWDOL_TRACE_NARRATIVE_MOVE=1` | 溜达 INSTALL / SKIP |
| `SWDOL_TRACE_SPELL=1` | 法攻 |
| `SWDOL_TRACE_PET_ACTOR=1` | 宠物演员 |
| `SWDOL_TRACE_PLAYER_SPELL_ADMISSION=1` | 玩家法术准入 |
| `SWDOL_TRACE_HATE=1` | **仇恨追击**（SKIP265 / NPC_HATE_PLOT） |
| `SWDOL_TRACE_PLAYER_PHYSICAL=1` | **物攻 settle** |

改 TRACE 须重跑 deferred boot（改 packet）。踢图 opcode 另批 TRACE，勿与三目标混为一谈。

---

## 7. 公办对齐清单

| 资源 | 期望 / 注意 |
|---|---|
| NPCCombat.tsv | sha `507bbaca…` → `runtime/runtime-definitions/` |
| Object.dbf | sha `cb2e1c69…` |
| Table.TB | sha `8fbd2bc2…` |
| quest 五件套 | `shared-from-sim/npc-preserved-task-source/{catalog,details,activations,completions,combat}.tsv` |
| `--player-combat-bindings=` | → `shared-from-sim/binding.private.active.tsv`；缺测号行按 account-store 补洞，**勿空模板冒充** |
| catalog + npc-experience | 成对 **box-admit**（钳 level∈[0,100)）；**禁止** catalog 与 experience 混源；pristine level≥100 会 FATAL |
| bindings 改后 | **必须重登** 角色 |

`restore-shared-from-kit.sh` 一次落地共享与 definitions。

---

## 8. 数据库

- **唯一服务**：所有读写经 `swdol_world_database_service`；业务进程禁直开库（见约束 C7）。  
- **inode**：boot 用 `world.sqlite` 的 `st_dev:st_ino` 填 `--database-service-device` / `--database-service-inode`。换库文件后必须重 boot。  
- **还原**：`restore-world-sqlite.sh` 解压 `artifacts/db/world.sqlite.vacuum.gz`（lab 态，含 kv；公开镜像前需审查）。  
- **lease 清理**：停 map/ops/db 后删 CLEAR-PLAN 所列 kv 键 + 清空 `*.map-owner-locks` 内锁文件；目录保持 **0700**（否则 StorageFailure code=8）。  
- **role-state / HP 离线修补**：可改 kv（例 `legacy-compat/role-state/1/0`），但须 **角色登出**；在线时乱补会被回写覆盖。sectionId 须为真实图（长阳常见 **1**，勿长期 -1）。见 `docs/smoke/ROLE-STATE-HATE-PR101-REPAIR.md`。  
- **HP=1 / death-stage**：可能挡仇恨/追击刺激；测追击前确认目标与玩家状态。

---

## 9. 测试方法（三目标 + 踢图）

### 9.1 溜达（不登号也可）

- Ops `map-detail` 多窗读 NPC/怪 **XYZ**（权威在 Map，不靠截图）。  
- stderr：`NARRATIVE_MOVE_INSTALL section=0 count=…` / `section=1 count=…`（公办对齐后常见 **14/67**；count=0 先查资源勿只开 env）。  
- 通过参考：长阳 section1 INSTALL>0、毛毛 anyMoved、maxPairDist 量级对齐公办 G2（约 75–168）。

### 9.2 物法攻

- 网页点选怪自动寻路贴脸；或 `front_navigate_route` + 选中 + `front_physical_attack` / 键盘 **Q** / 剑按钮。  
- **毛毛**常 Mode **`0x20`**：可有跳字/0290，**可不掉血** → 只作协议/表现靶。  
- **真扣血**：Mode **无 0x20**（例长阳城男守卫 oid **20337** role **265**）。Ops 同 oid **HPΔ** 为准。  
- stderr：`PHYSICAL` / `SPELL_*` / `0280`（以实际 TRACE 名为准）；缺 binding → `SPELL_BEGIN_REFUSED`。  
- 分层：协议进链 ≠ 真扣血。

### 9.3 仇恨追击

- boot 须含 `SWDOL_TRACE_HATE=1`（及物攻刺激时的 `SWDOL_TRACE_PLAYER_PHYSICAL=1`）。  
- 先证明**命中**刺激（0280 / ATTACK_TRACE / HPΔ），再看：  
  - `NARRATIVE_MOVE_SKIP role=265`（及 target=玩家）  
  - `NPC_HATE_PLOT`  
  - Ops 多窗目标 xyz 是否向玩家收敛  
- **无命中证据时不得判 chase=NO**（记 OPEN/stimulus-blocked）。  
- HP=1 或 death-stage 会挡；城卫可能不追。刀测可用 hook front-client（`restore-web-hook.sh hook`）；测完可 pristine。证据样例：`docs/smoke/CAPTURE-LIVE-r*.md`。

### 9.4 踢图

- **另批** TRACE/重启与取证；**不挡**溜达/物法/仇恨三目标结论文档。  
- 连断 `MAP_CONNECTION_CLOSED` / transferring 与踢图根因分开记 OPEN。

---

## 10. 网页 / WASM / computerUse 坐标坑

| 坑 | 正确做法 |
|---|---|
| computerUse 桌面 | **1280×800**；勿用 1920 PNG 像素点 |
| canvas CSS | **800×600** |
| 导航 | 须开 **Size200 / mode1** 小地图，否则 `NAV_INPUT_CHART_MODE` |
| URL | `http://127.0.0.1:63497/front-client.html?asyncReads=1&worldStart=1&coldArchive=1`；硬刷新清旧 wasm |
| 导航 API | `front_navigate_route(x,z)` → `0x07ea`→规划→`0x07eb`+BODY；**不是** 0280 寻路 |
| 自动导航 `/nav` | tip≥`e81172ece`：聊天 `/nav 小地图X Z` / `/nav stop`（自动 Size200→navigate_route）；`?navigationControl=1` 为 raw 桥；详见 `docs/smoke/CHAT-NAV-AUTO-20261031.md` |
| WASM | 完整 `front_probe` 四导出 + `client-build-version.json` 对齐；只 cherry-pick 导航会 version mismatch |
| 页内 evaluate | 常被拦 → 用 **DevTools 控制台** ccall |
| 测号 | 非 GM；Ops 只观测，不走路不攻击 |

细节闭环见 **`swdol-box-onboard-inject`**。

---

## 11. 原版注入

- 只走 collector / attach（LoadLibrary）；**不操作 VM GUI**。  
- 同 PID 同制品 → **`ALREADY_ACTIVE` 复用**（一人 attach owner，余人复用）。  
- putFile 覆盖 cohort 后再 attach（LoadLibrary=0 多为旧文件尺寸错）。  
- **用完 detach / 卸 payload**。  
- 双人规程：`skill-1793274920988`；摘要见 onboard §H。

---

## 12. 约束红线（摘要；全文 `docs/constraints/`）

编号便于引用；冲突时以用户最新裁决 + 仿真机全局 AGENTS 为准。kit 副本：

- `docs/constraints/COMPLETE-REPORT.md` — 最高入口定位  
- `docs/constraints/sim-codex-AGENTS.md` / `sim-swdol-AGENTS.md` — 全局/项目 AGENTS 箱副本  
- `docs/constraints/sim-最高约束-原版兼容与高风险数值扩容.md`  
- `docs/constraints/sim-第一原则-任何修复不得破坏已验收功能.md`  
- `docs/constraints/sim-problem-class-closure.md`  
- `docs/constraints/LOCAL-ENVIRONMENT-RULES.md` — **控制机 ORICO 第二层**；箱上勿照搬外盘路径  

| # | 红线 |
|---|---|
| C1 | **未完成用户任务不得主动停工**（无必须阻塞则继续） |
| C2 | **真实报告**：不虚报/缩减目标；通过\|失败\|已修待验\|未测分记 |
| C3 | **先抓主要矛盾**：先解首阻断，支线让位 |
| C4 | **同一 Bug 复现后半小时两轮必须解决，事不过三** |
| C5 | **问题类闭环**：「所有」须冻结完整分母再集中修 |
| C6 | **修复不得破坏已验收功能**；回归保留分母 |
| C7 | **唯一独立 DB 服务**；禁业务直开库 |
| C8 | **未知行为先到原版**找答案；禁臆造兼容语义 |
| C9 | **测试环境谁用谁清场**；删文件先列清单报批 |
| C10 | **不烦同事、不重复索批**；一次授权继续执行 |
| C11 | **完成目标效率优先**；成熟路线；禁止节外生枝 |
| C12 | **CONFIRMED_BUG=0 前不改业务 `.cpp/.h`**；调试日志可加且不上主线 |
| C13 | **先公办后算法**；未对齐不作算法结论 |
| C14 | 原版注入用完 **detach**；禁盲注、禁口令进聊天 |
| C15 | 结论分层：观测/派生/推断/未知；完成码见 §13 |
| C16 | 开发阶段我方 **Master/Map/DB 同批**；客户端只认 Master 声明版本 |
| C17 | 箱实验 **密钥明文在 kit**（用户 2026-10-31）；技能/聊天仍避免无谓粘贴密钥值 |

---

## 13. 验收清单与完成码

### 栈/环境

1. `ss -ltnp | rg '63418|63419|63499|63497'` 四口 LISTEN  
2. Map sha tip live `7f500368…`（见 docs/smoke/PIN-f11d1f6a.txt）  
3. Map environ 含 §6 TRACE/ENABLE 全集（含 HATE / PLAYER_PHYSICAL）  
4. stderr：`NARRATIVE_MOVE_INSTALL` section0/1 count>0（常见 14/67）  
5. 无 `SERVER_FATAL` / `ownership lost` / catalog FATAL  
6. NPCCombat `507bbaca…`；Object.dbf `cb2e1c69…`；bindings → `binding.private.active.tsv`  
7. 烟测 URL §10  

### 业务（分记）

| 目标 | 最低通过证据 |
|---|---|
| 溜达 | INSTALL>0 + Ops XYZ 运动样本 |
| 物法 | 协议进链；真扣血靶另证 HPΔ |
| 仇恨 | 命中后 SKIP265/NPC_HATE_PLOT 或视觉 xyz 收敛；无刺激则 OPEN |
| 踢图 | 另单；不否决上三项 |

### 完成码

`STATIC_ONLY` | `S0R` | `PARTIAL` | `OPEN` | `PROVEN`

---

## 14. 与 onboard-inject 分工

| 技能 | 职责 |
|---|---|
| **本技能 `swdol-box-cloud-stack`** | 还原 kit、deferred/公办/DB/TRACE、三目标测法纲要、约束、验收门 |
| **`swdol-box-onboard-inject`** | Ops 定靶 → Size200 导航 → 选中攻击 → 源码/stderr 对照逐步手册 |
| **`skill-1793274920988`** | 多人同 Map 探针：owner attach + ALREADY_ACTIVE |

安装本技能到 bot：见 kit `skills/README.md`（拷到 `/home/box/agent-data/workflows/<id>/SKILL.md`）。

---

## 快速索引

| 需要 | 打开 |
|---|---|
| 云电脑从零（深度分析） | `docs/smoke/CLOUD-BOX-SETUP-FOR-SWDOL-DEEP-ANALYSIS.md` |
| 逐步还原 | `RESTORE.md` |
| sha/端口/制品 | `MANIFEST.json` |
| 打包说明 | `PACKAGING-REPORT.md` |
| 清 lease | `docs/smoke/CLEAR-PLAN-stack-restart-20261031-0357.txt` |
| 重编 | `docs/smoke/REBUILD-PLAN.md` · `CUTOVER.md` |
| 仇恨刀 hook | `docs/smoke/FRONT-FC-HOOK-HATE-R3.md` |
| 约束全文 | `docs/constraints/` |


## 踩坑速查（3934 后）

- computerUse：**1280×800**；勿用 1920 截图像素当点击坐标；普攻优先键盘 Q。
- web-audio：站点根硬链 `native/web-audio`；加载层「cache 0」≠缺文件（地图七字段键时序）。
- all_map2.act：tip≥3934 客户端 `readRuntimeAsset(.../ACT/all_map2.act)` 不走 map pack；须松散文件 + `world-bootstrap-resources.json` 登记大写 `ACT/all_map*.act`（WASM declare；仅 HTTP 200不够）。`SITE_ROOT=… bash gork.bot/scripts/apply-web-resource-overlays.sh` + 见 docs/smoke/ALL-MAP2-ACT-FIX.md。
- 追击靶用**城卫**；毛毛不打人、不追、不掉血，仅闲逛。
- 踢图另批；不挡三目标「一样/不一样」结论。
