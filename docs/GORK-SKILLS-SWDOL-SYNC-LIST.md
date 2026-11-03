# gork.skills.swdol sync list（只列路径，不 push）

- **target**：https://github.com/caucy2026/gork.skills.swdol
- **stamp**：2026-11-03 Asia/Shanghai
- **actor**：executor 列清单；parent / CloudAgent 负责 GitHub

## A. Bot 本机 workflows（`/home/box/agent-data/workflows`）

### swdol-*（必推）
1. `/home/box/agent-data/workflows/swdol-allclass-magic-acceptance/SKILL.md`
2. `/home/box/agent-data/workflows/swdol-box-cloud-stack/SKILL.md`
3. `/home/box/agent-data/workflows/swdol-box-onboard-inject/SKILL.md`
4. `/home/box/agent-data/workflows/swdol-kemi-codex-constraints/SKILL.md`

### 相关非 swdol 前缀（建议同仓）
5. `/home/box/agent-data/workflows/bot-kemi-box/SKILL.md`
6. `/home/box/agent-data/workflows/gork/SKILL.md`

### 跳过
- `/home/box/agent-data/workflows/swdol-box-onboard-inject/SKILL.md.02c3d8263b2d.tmp`（临时文件，勿推）

## B. 约束副本（kemi 对齐，建议进仓 `constraints/`）

7. `/home/box/agent-data/agents/2d8a7216-a700-4f35-9566-5c917a0f81b9/constraints/kemi-codex-AGENTS.md`
8. `/home/box/agent-data/agents/2d8a7216-a700-4f35-9566-5c917a0f81b9/constraints/swdol2026-AGENTS.md`
9. `/home/box/agent-data/agents/2d8a7216-a700-4f35-9566-5c917a0f81b9/constraints/最高准则-探针采样用完必清-20261101.md`
10. `/home/box/agent-data/agents/2d8a7216-a700-4f35-9566-5c917a0f81b9/constraints/最高约束-原版兼容与高风险数值扩容.md`
11. `/home/box/agent-data/agents/2d8a7216-a700-4f35-9566-5c917a0f81b9/constraints/第一原则-任何修复不得破坏已验收功能.md`
12. （若存在）`/workspace/codex-constraints-live/最高准则-完成任务见错必修箱可重启-20261101.md`

## C. CLASS / 开团 / 神龙仿真文档（本次交付 + 权威源）

### 新交付（已存在，勿重复改写）
13. `/workspace/swdol-smoke/shenlong-sim-simpy/CLASS-OPEN-RAID-ROLES.md`

### 同目录配套
14. `/workspace/swdol-smoke/shenlong-sim-simpy/COMBO-PARALLEL.md`
15. `/workspace/swdol-smoke/shenlong-sim-simpy/HARD_GATE.md`
16. `/workspace/swdol-smoke/shenlong-sim-simpy/TEN-COMBOS-SCORED.md`
17. `/workspace/swdol-smoke/shenlong-sim-simpy/RESULT.md`
18. `/workspace/swdol-smoke/shenlong-sim-simpy/README.md`
19. `/workspace/swdol-smoke/shenlong-sim-simpy/GORK-SKILLS-SWDOL-SYNC-LIST.md`（本清单）

### raid-open-r1 权威（建议 `docs/raid-open-r1/`）
20. `/workspace/swdol-smoke/algo-hate-20261031-r10/fork6-inject-r58/allclass-magic/raid-open-r1/CLASS-SKILL-BRIEF.md`
21. `/workspace/swdol-smoke/algo-hate-20261031-r10/fork6-inject-r58/allclass-magic/raid-open-r1/FULL-BOSS-AI-EXTRACT-r1.md`
22. `/workspace/swdol-smoke/algo-hate-20261031-r10/fork6-inject-r58/allclass-magic/raid-open-r1/OPTIMAL-RAID-COMBO-r1.md`
23. `/workspace/swdol-smoke/algo-hate-20261031-r10/fork6-inject-r58/allclass-magic/raid-open-r1/ROSTER-TEN-MAN-r2.md`
24. `/workspace/swdol-smoke/algo-hate-20261031-r10/fork6-inject-r58/allclass-magic/raid-open-r1/ROSTER-TEN-MAN-r2.json`
25. `/workspace/swdol-smoke/algo-hate-20261031-r10/fork6-inject-r58/allclass-magic/raid-open-r1/SAFE-STAND-PROTOCOL.md`
26. `/workspace/swdol-smoke/algo-hate-20261031-r10/fork6-inject-r58/allclass-magic/raid-open-r1/SIMULATION-HONESTY-r1.md`

## D. 不在本 bot workflows、但 skills-onhand 有全套 swdol-*（可选大批量同步）

根：`/workspace/skills-onhand/swdol-*`（目录级，数量多；是否整仓镜像由 parent 定，不默认全推）。

## 动作边界
- executor：**不** `git push` / 不直接改远端
- parent：经 source-control / CloudAgent 推 `caucy2026/gork.skills.swdol`
