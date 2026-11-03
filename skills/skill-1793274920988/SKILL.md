---
name: 双人远程仿真注入
description: >-
  两人或多同事要对同一原版 Map 同时用探针/注入时用：一人 attach owner、余人 ALREADY_ACTIVE 复用同一 collector；经
  VibeKits 仿真通道亦同。
---
# 双人远程仿真注入

成熟路线权威：仿真机 `~/.codex/skills/swdol-remote-sim-inject` §6；短文 `test/20261029-t793-dual-inject/README.md`。本技能是可复用口令，不替代 Map SAFE runbook。

## 一句话

**能两人同时用探针；不能各开一套 injector / collector。**

仿真 = 远程的手，不是第二套注入协议，也不是注入互斥锁。

## 角色

| 角色 | 谁 | 做什么 |
|---|---|---|
| **A injector owner** | 唯一允许 attach-only 的一方 | 门禁 → 唯一 collector → 一次 LoadLibrary → 写 `ATTACH-RECEIPT.json` → 通知 B |
| **B 复用方** | 其余同事 | 只读门禁 → 期望 `ALREADY_ACTIVE` / 同 PID → 读同一 collector JSONL；**禁止**第二 attach |

开工前用文字钉死 A/B；换 owner 必须先交接并改写 receipt。

## A（owner）步骤

1. 只读门禁：目标口（如 Map04=`5004`）唯一 `NewLocalServer.exe`；cmdline 对图；无第二 collector 抢 UDP（常 `27667`）。
2. 未挂：唯一 collector → MAP*-SAFE **一次** attach-only → `INJECT_OK` + `PAYLOAD_LOADED` + `PROBES_ACTIVE`。
3. 已挂同制品：返回/视为 `ALREADY_ACTIVE`，**禁止再 LoadLibrary**。
4. 写共享 `ATTACH-RECEIPT.json`：至少 `pid` / `port` / `dll` / `SHA` / `result` / collector `OUT`（JSONL 绝对路径）/ 时间。
5. 通知 B：receipt 路径 + PID + OUT；自己也可继续读同一 JSONL。
6. 缺探针：加**独立名**低频只读 addon（先静态核 RVA/原字节）；禁同址叠钩、禁整包重装「多挂几个点」。

## B（reuse）步骤

1. 只读核：端口 owner PID == receipt.pid；collector 健康；模块表已驻留。
2. PID 变 → 报 `NEW_PID_REQUIRED`，**停手**，仍由 A 重挂；勿自己硬注。
3. 期望 `ALREADY_ACTIVE`；scoop 同一 JSONL；业务刺激可自采。
4. 需要新叶：把规格（RVA/原字节/事件字段）交给 A 加独立 addon；**禁止**自己第二 injector / 第二 collector / 同址 INT3。

## 经仿真额外

1. 两边都可 `vibekits.simulator.connect` 跑与本机相同的 Map SAFE **只读/复用**步骤。
2. 若仿真 ssh/控制面互斥：错峰，或一人代跑 shell，另一人只读 receipt+JSONL。
3. 已挂 → 两边都走复用；未挂 → **仅 A** 经仿真一次 attach-only。
4. 勿把「往仿真机 Grok 打字」（gork-sim-inject）当成 Map `LoadLibrary`。

## 硬禁

- 第二竞争 injector / 并行 DEPLOY.BAT / 启动式注入
- 同 PID 再热挂一套基础 payload；健康载荷再 LoadLibrary 同 DLL
- 叠同址钩；第二 collector 抢同一 UDP 口
- 端口非唯一 owner 仍硬注；启停七服
- `INJECT_OK` 当业务成功 / D1

## 收口回报模板

```text
双人=owner=<A>|reuse=<B>
成熟路线=swdol-remote-sim-inject§6
receipt=<绝对路径>
collector=唯一复用|新建|FAIL
pid=<n> result=INJECT_OK|ALREADY_ACTIVE|NEW_PID_REQUIRED
禁项=未第二injector；未叠同址；未第二collector；未启停七服
本轮差异=<无|…>
```

## 与分析窗配合

- 结论分层：观测 / 派生 / 推断 / 未知；完成码 `STATIC_ONLY|S0R|PARTIAL|OPEN|PROVEN`。
- `CONFIRMED_BUG=0` 不改业务 `.cpp/.h`。
- 证据闭合前只产探针证据，不发明补丁。
