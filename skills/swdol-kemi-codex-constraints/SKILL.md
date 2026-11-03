---
name: swdol-kemi-codex-constraints
description: >-
  SWDOL/怀旧OL 任何任务开工前必用：导入并遵守仿真机 kemi Codex 全局约束与项目 AGENTS（含最高约束与第一原则；含
  2026-11-01 完成任务/见错必修/箱可重启；含探针采样用完必清）。
---
# SWDOL kemi Codex 约束（本 bot 强制入口）

权威来源是仿真机 kemi，不是控制机 newlink 的通用 AGENTS。箱内已导入全文副本；动手前按序打开正文，禁止只读标题或凭记忆。

## 权威路径（箱）

1. **全局最高**：`/workspace/codex-constraints-live/kemi-codex-AGENTS.md`（镜像 `/Users/kemi/.codex/AGENTS.md`）
2. **项目层**：`/workspace/codex-constraints-live/swdol2026-AGENTS.md`（镜像 `/Users/kemi/coding/swdol2026/AGENTS.md`）
3. **具名最高约束**：`/workspace/codex-constraints-live/最高约束-原版兼容与高风险数值扩容.md`
4. **第一原则红线**：`/workspace/codex-constraints-live/第一原则-任何修复不得破坏已验收功能.md`
5. 本 bot 持久副本：`/home/box/agent-data/agents/2d8a7216-a700-4f35-9566-5c917a0f81b9/constraints/`（同名全文）
6. **2026-11-01 用户最高准则摘要**：`/workspace/codex-constraints-live/最高准则-完成任务见错必修箱可重启-20261101.md`（全文段 `BOX-NO-WAIT-FIX-ERRORS-RESTART-OK-20261101`）
7. **探针/采样必清摘要**：`/workspace/codex-constraints-live/最高准则-探针采样用完必清-20261101.md`（全文段 `PROBE-SAMPLE-CLEANUP-ON-EXIT-20261101`）

全局技能目录：`/workspace/skills-onhand/`（从 kemi `~/.codex/skills` 拉齐）。

刷新：经 vibekits `5298938227` 自拉 kemi 上述文件覆盖 `codex-constraints-live/`；**未经用户允许不麻烦其他 agent**。

## 箱实验最高工作准则（2026-11-01 t387u，强制）

1. **完成用户任务优先，禁止干等。** 不得以同事 hold/放行/ping 停工；队友延时当 FYI；只有用户叫停才停。
2. **见后台/界面错误立刻修**，不得攒着、不得只记 journal 代替修复、不得拿 STACK_OWNED 当免修。
3. **箱后台可随时在最新原版/main 上改并重启**，无需征求深度解析同意；尽量 FYI，但不以其回复为前置。
4. **别人占某一 Map pid**：只避直接冲突，立刻换其他账号/会话继续主线。
5. 与「未完成不得自停」「先抓主要矛盾」「半小时两轮」同执；冲突时本条优先于协作礼貌。

## 仿真机探针与采样必清（2026-11-01 t465u/t466u，强制）

- 仿真机上加的探针/collector/采样 BIN·JSONL，**用完当场清**；禁止堆到百 GB 留给别人收拾。
- 开注前设有界采集（时长/事件数/字节/轮转/最低余量）；结束停写 → 归档必要证据并回读哈希 → 精确删本方冗余 → 核 df 净变化。
- 不热卸他人活跃探针；不通配删源码/原版/DB/VM/现役 DLL/唯一未归档证据；谁挂谁清。
- 成熟入口：`disk-and-scene-cleanup.md`、`swdol-static-dynamic-analysis` 磁盘巡检、`swdol-remote-sim-inject` 收尾。规则落盘 ≠ 已清场。

## 每次开工必守（摘录，细节以全文为准）

- **同一 main**：全员同一产品仓 main；禁个人长期分支代替交付；环境谁用谁清；禁另建回滚平行体系。
- **第一性原则**：先回到用户真实业务结果；核已给办法/授权、全局技能与成功记录、是否最优可行、错了怎么恢复、是否真解决。
- **半小时两轮**：同一 Bug 复现后半小时两轮必须解决，事不过三。
- **先抓主要矛盾**：首阻断优先；支线让位。
- **未完成不得自停**；**真实报告**（通过|失败|已修待验|未测）。
- **一个问题一类问题**：用户说「所有」先冻完整分母。
- **唯一独立 DB 服务**；禁直开库瞎改死亡回执；走技能主线读路径。
- **修复不得破坏已验收功能**；原版未知立即超级深度分析。
- **CONFIRMED_BUG=0 前不改业务码**；成熟路线优先，禁舍近求远。
- **不额外联系同事 / 不重复索批**（用户另允除外）。

## 与箱实验

箱栈/定靶导航攻击：先 `swdol-box-cloud-stack` / `swdol-box-onboard-inject`；死亡/三实验：`swdol-three-experiments` + `swdol-resurrection-acceptance` + `swdol-feature-extension`。合主线时先读 main 改动记录并用箱真核，不无脑全认可也不无脑反对。
