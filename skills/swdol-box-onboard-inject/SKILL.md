---
name: swdol-box-onboard-inject
description: >-
  同事从零做箱全链路调试时用：Ops 定靶、网页导航/攻击、后台源码与 stderr 对照、公办对齐、WASM/版本闸、仿真 Map SAFE 注入。
---
# SWDOL 箱全链路调试（定靶 → 走到 → 攻击 → 后台）

给同事从零跑通一条成熟闭环：**发现地图对象 → 控制网页角色走到那里 → 触发物攻/法攻 → 对照后台源码与日志验收**。含编译、公办对齐、deferred 栈、WASM 版本闸、仿真注入。勿与生产云端 `/opt/kemi-gm/swdol`（`swdol-cloud-deployment`）混用。

相关：[swdol-box-cloud-stack](sand-workflow:swdol-box-cloud-stack)·仿真 `swdol-remote-sim-inject`·`swdol-web-game-deployment`·[skill-1793274920988](sand-workflow:skill-1793274920988)

工作根 `/workspace/swdol-box-backend/`；证据 `/workspace/swdol-box-backend/evidence-20261030/`；烟测 `/workspace/swdol-smoke/`。

---

## A. 全链路总览（先读这张表）

| 阶段 | 做什么 | 权威证据 | 常见坑 |
|---|---|---|---|
| **A1 发现对象** | Ops `map-detail` 读玩家/怪/NPC 池 | oid、xyz、HP、Mode、phase | 截图猜名；token 过期 |
| **A2 走到** | **优先聊天 `/nav 小地图X Z`**（自动 Size200）；或 DevTools `front_navigate_route(rawX,rawZ)` | `0x07ea`→规划→`0x07eb`→BODY；Ops xyz 靠近 / arrived | 当 0280 寻路；把小地图单位当 raw；tip 无 `/nav` 却盲用 |
| **A3 攻击** | 选中目标 → `front_physical_attack` / Q / 点选贴脸 | map `0280`/`SPELL_*`；Ops HPΔ | 未选中；城卫非敌对；毛毛 Mode `0x20` 不掉血 |
| **A4 后台调试** | 源码钉主链 KEY + stderr/Ops 对照 | 源码路径 + 日志关键字 + 完成码 | 未对齐公办就判算法；CONFIRMED_BUG=0 乱改业务码 |

口诀：**先 Ops 定靶，再导航贴脸，再攻击刺激，最后源码+日志收口；环境与公办一样样，再判算法。**

测号（如 `nana`）**不是 GM**；进服贴怪走正常客户端。Ops 观测口可读权威，**不走路、不攻击**。

---

## B. 阶段 A1 — 如何在地图中发现想要的对象

### B.1 为什么不用画面

客户端看见的是表现层；**权威在 Map 进程 / world.sqlite**。箱上用 Ops HTTP `127.0.0.1:63499` 的 `map-detail` **只读**接口扫当前图节点池。

### B.2 步骤

1. 栈听口：63418/63419/63499（及需要时 63497）。
2. Ops 登录拿 token（会过期；失败重登 boxop，刷新烟测目录 `login.json`）。
3. `POST /api/admin/map-detail/request` → `read`，拉玩家池 + 怪物/NPC 池。
4. 过滤字段：**oid / role / 显示名 / xyz / HP/max / Mode / phase / attackable**。
5. 按实验目的筛：
   - **溜达/剧情**：可挂后台多窗读坐标，不必登号刺激。
   - **伤害表现/协议**：可用毛毛（常 Mode **`0x20`**：可有跳字/0290 **但不扣 HP**）。
   - **真扣血**：必须 **Mode 无 `0x20`**，例长阳城男守卫 **oid=20337** role 265 Mode `0xc50f`。
6. 记下基线：目标 xyz、HP，以及玩家（若已在线）xyz → 写 `TARGETS.json` / `PRE.md`（例：`true-hp-nav-20261030/`）。

### B.3 完成标准

有一份机器可读靶点：至少 `oid`、`x`/`z`、`hp`/`max`、`mode`、采样时间、map section。距离可用玩家 xz 与目标 xz 估算。

---

## C. 阶段 A2 — 如何控制客户端走到对应位置


### C.0 优先：聊天自动导航 `/nav`（web1012 · e81172ece）

云端/main 已加共享聊天命令，**去任意点最省事**（自动开 Size200，再走同一套 `navigate_route`）：

| 输入 | 含义 |
|---|---|
| `/nav <x> <z>` | **小地图单位**（界面 trunc(X/100),trunc(Z/100)）；内部 ×`worldUnitsPerGridCell`（常 100）→ raw 后 `navigate_route` |
| `/nav stop` | 停止导航 |

例：Ops raw 目标 `(22600, 47602)` → 聊天发 `/nav 226 476.02`。成功时可见自动导航文案；`planning≠arrived`，用 Ops 玩家 xyz 或 `/nav` 到达回执核。

可选：URL `?navigationControl=1`（localhost/https）暴露 `__swdolNavigation.route({x,z,scope})`，此处 x/z 为 **raw**。证据：`gork.bot/docs/smoke/CHAT-NAV-AUTO-20261031.md` · 产品 `document/20261031-web1012-navigation-and-current-errors.md`。

**箱 tip `f11d1f6a` 早于该提交**；要用 `/nav` 须合 main≥`e81172ece`/`5324d7a1c` 并重编网页。未合入时用下方 §C.3 DevTools `front_navigate_route(rawX,rawZ)`。

### C.1 原理（同事必懂）

- **不是**助手逐步点地面跟路线。
- **不是**物攻 `0x0280` 超距 PathFind（超距只 range-reject）。
- **是**已有 **小地图导航** 控制命令：
  1. `navigate_route(x,z)` → 客户端发 **`0x07ea`**
  2. Map：`LegacyMapNavigationWorker` / `LegacyMlzoPathPlanner` 规划
  3. 回 **`0x07eb`** 路点 → **原 BODY 连续跟随**自动走完
- 网页 `front_*` 只是 `dispatchControl` 薄导出；原生 MCP / `SWDOL_CLIENT_CONTROL_SOCKET` 同名命令已存在。发现路径：源码/文档「去任意点 = MapNavigation」→ 对照 ClientControl → PR 加 EMSCRIPTEN 导出（**禁止新做 Ops 寻路 CLI**）。文档：`document/20261030-web-front-map-navigation.md`。

### C.2 网页前置

1. URL：`http://127.0.0.1:63497/front-client.html?asyncReads=1&worldStart=1&coldArchive=1`（硬刷新清旧 wasm）。
2. 登录测号 → 选角 → 入目标图（例娜娜/长阳）。
3. canvas CSS **800×600**（computerUse 坐标空间仍是桌面 1280×800）。
4. 打开 **Size200 / mode1** 小地图：大览图先点「地图」关掉，再点一次切到第一种尺寸；否则 `NAV_INPUT_CHART_MODE`。
5. WASM 须含四导出且版本闸通过（见 §F）。

### C.3 触发导航（DevTools）

页内 evaluate 常被拦 → 用 **DevTools 控制台**：

```javascript
function frontControl(module, name, types, values) {
  const text = module.ccall(name, 'string', types, values);
  if (!text) throw new Error(module.UTF8ToString(module.ccall('front_error', 'string', [], [])));
  return JSON.parse(text);
}
frontControl(module, 'front_navigate_route', ['number','number'], [goalX, goalZ]); // Ops 权威 xz，可微偏
frontControl(module, 'front_navigation_status', [], []);
```

`ok:true` + `planning` + `source:"server"` = **请求已进规划，不是到达**。轮询 status；并用 Ops 再读玩家 xyz 看是否靠近目标。

### C.4 点选怪的自动寻路

网页**点选界面目标**会客户端自动寻路贴怪，实打可优先点怪。远处/任意坐标仍用 `front_navigate_route`。

---

## D. 阶段 A3 — 如何触发攻击

### D.1 物攻

1. **先选中**目标（点身体；城卫可能非敌对，仍须选中）。
2. 任选其一：
   - `frontControl(module, 'front_physical_attack', [], [])` → 成功常 `{"queued":true}`（只表示 `0280` 排队）
   - 键盘 **Q**（`togglePhysicalAttackIntent`；聊天框占键会吞）
   - 右侧攻击/剑按钮（指针映射正确时；见 §F.3）
   - 敌对怪：身体第二下 Attack
3. **不寻路**：超距只拒绝；须先导航或点选贴脸。

### D.2 法攻

走技能栏/绑定表；角色须有 `--player-combat-bindings=` 行，改 bindings **必须重登**。看 map 侧 `SPELL_*` / 聊天 STAGE；缺 binding → `SPELL_BEGIN_REFUSED`。

### D.3 验收（分层）

| 目的 | 通过条件 |
|---|---|
| 协议进链 | map 有 `0280` / SPELL 请求；可有跳字/0290 |
| 真扣血 | Ops 同一 oid **HP 下降**；目标 Mode **无 0x20** |
| 毛毛协议靶 | 允许有伤害表现但不掉血；**不以掉血当通过** |

追击：攻击进链后再看目标 xyz 是否向玩家收敛（城卫可能不追）。

---

## E. 阶段 A4 — 如何调试后台代码

### E.1 工作方法（强制）

1. **超级深度对照源码**：钉主链与 KEY，差异清单先报用户。
2. **需要时再注入**验证差异是否成立；明确关键点与要 scoop 的字段；禁盲注、禁连续无目的注入。
3. `CONFIRMED_BUG=0` 前 **不改业务** `.cpp/.h`；调试日志可加且不上主线。
4. 修确认的自研问题：在最新 GitHub **main**（冻结提交/manifest）上修；不上脏分支。
5. 箱可随意加调试日志做实验，**不影响他人、不上主线**；动手前先规划实验面与接口。

### E.2 读哪里

| 层 | 路径/手段 |
|---|---|
| 源码树 | `srcbuild-main-<pin>/`（对齐 main pin）；禁 dirty WIP 当结论源 |
| Map/Master stderr | `runtime/logs/map-defer.stderr`、`ops-defer.stderr`、`db.stderr` |
| Ops 权威 | map-detail 多窗 xyz/HP |
| 网页 | DevTools 网络/控制台；WASM 导出 ccall 返回 JSON |
| 原版对照 | 仅 collector/attach 注入；用完 detach/卸 payload |

物攻主链关注：请求准入 → 距离/Plot0 → 算伤 → 写 HP；关键字例 `ATTACK_TRACE` / `0280` / `hpDelta` / `HP_WRITE`（以源码实际日志名为准；缺则先规划再加 stderr 探针）。

导航主链：`0x07ea` / `LegacyMapNavigationWorker` / `LegacyMlzoPathPlanner` / `0x07eb`。

### E.3 环境未齐时勿判算法

先对齐公办：world.sqlite、Object.dbf（sha `cb2e1c69...`）、catalog+experience 成对、NPCCombat、quest 五件套、bindings。catalog FATAL / lease ownership lost → 先修栈再测。live 二进制 sha 与源 commit 记入证据。

### E.4 完成码

`STATIC_ONLY` | `S0R` | `PARTIAL` | `OPEN` | `PROVEN`。结论分层：观测 / 派生 / 推断 / 未知。

---

## F. 箱基础设施（支撑全链路）

### F.0 机器角色

| 角色 | 用途 |
|---|---|
| Grok Bot 箱 | 自编后台/Web、deferred、网页实打 |
| 控制端 Mac | invoke.rb→仿真；口令 stdin |
| 仿真机 routingId 5298938227 | SMB/SCM；collector |
| Win 原版访客 | NewLocalServer |
| 生产云端 119.96.24.110 | **不是**箱替身 |

### F.1 后台编译

cmake `/workspace/swdol-box-backend/tools/cmake/bin/cmake`；ninja；g++14。`-S server -B .../build-box-main -G Ninja -DCMAKE_BUILD_TYPE=Release -DBUILD_TESTING=OFF`；目标含 `swdol_legacy_compat_server`、`swdol_world_database_service`、ops native 等。

### F.2 deferred 栈

顺序：deferred 占 map+ops → DB（inode=world.sqlite st_dev:st_ino）READY → 放 packet。一键 `runtime/bin/boot-stack-deferred.sh`。端口 63418/19/99/97。lease FATAL：清 kv + `map-owner-locks` 0700 再 boot。

### F.3 网页 WASM / 版本闸 / 指针

- 配方：`evidence-20261030/WEB-SELFBUILD.md`；目标 `swdol_browser_front_probe`。
- **完整** `front_probe.cpp`（含 `front_deployment_route_version` + 导航三导出）；`client-build-version.json` ≡ wasm 内嵌版本。只 cherry-pick 导航会 `WEB_DEPLOYMENT_ROUTE_ARTIFACT_MISMATCH` / `CLIENT_BUILD_VERSION_MISMATCH`。
- computerUse：**1280×800**；canvas CSS **800×600**；勿用 1920 PNG 像素点击。

### F.4 公办资源

入口仿真 `local-shared-current.json` / `database-current`；箱 `runtime/config/shared-from-sim/`。禁止个人拼库。

---

## G. 端到端操作清单（照做）

1. 编后台 + 公办导入 + boot-stack-deferred → LISTEN 无 FATAL
2. 完整 front_probe 部署 + health `webBuildReady` + 四导出
3. Ops 定靶 → `TARGETS.json`（Mode/HP/xyz）
4. 网页登录入图 → CSS 800×600 → Size200
5. DevTools `front_navigate_route` → status / Ops 玩家靠近
6. 选中 → `front_physical_attack`/Q → Ops HPΔ（真扣血靶）或协议关键字（表现靶）
7. 异常：源码钉 KEY → 差异清单 →（可选）注入验证 → 仅确认 bug 后上 main 修
8. 原版需要时：§H 注入；scoop 后卸净

---

## H. 远程注入（摘要）

attach-only LoadLibrary；同 PID 同制品 → `ALREADY_ACTIVE` 复用。putFile 覆盖 cohort DLL+EXE 后再 attach（LoadLibrary=0 多为旧文件尺寸错）。用完 detach/卸 payload。双人：[skill-1793274920988](sand-workflow:skill-1793274920988)。

---

## I. FAQ

**发现对象？** Ops map-detail，不是截图。
**聊天 `/nav`？** tip≥e81172：小地图单位 `/nav X Z` / `/nav stop`，自动 Size200；未合 tip 用 front_navigate_route(raw)。
**点小地图就能走？发现？** 体验像小地图去点；实现是 `navigate_route`→07ea/07eb+BODY；来自源码 MapNavigation + 已有 ClientControl。
**路线怎么走完？** 一次调用后客户端自动跟随路点；助手只观测。
**网页怎么调函数？** DevTools `module.ccall('front_navigate_route',...)`。
**0280 会寻路？** 不会。
**毛毛掉血？** 常不掉（0x20）；真扣血换无保护靶。
**后台怎么调？** 源码主链 KEY → stderr/Ops 对照 → 公办对齐后再判 → CONFIRMED_BUG 后才改业务上 main。

---

## J. 验收句

```text
栈=LISTEN 63418/19/99/97|无FATAL
公办=sqlite+Object+catalog/combat/bindings
WASM=完整导出+version对齐|CSS=800x600|Size200
定靶=Ops oid/Mode/HP/xyz
导航=07ea/server planning|玩家xyz靠近
攻击=选中+0280/SPELL|真扣血=HPΔ或协议靶分层
后台=主链KEY+日志|CONFIRMED_BUG策略|完成码=
禁=Ops寻路CLI;盲注;脏WIP当结论;口令进聊天
```

---

## K. 红线

先技能/手册；禁无 KEY 盲注；BIN 用完卸净；删文件先报批；测号非 GM；结论分层；不上无关 WIP。
