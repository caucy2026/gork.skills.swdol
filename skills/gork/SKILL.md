---
name: 远程协助-仿真投Gork
description: 经 vibekits 仿真渠道给异账号 Grok Bot 发消息、请同事帮忙，或对齐本机 Codex 会话时使用。
---
# 远程协助：仿真投异账号 Gork / 请同事帮忙

## 何时用

- 给**另一台 Mac、异账号**的 Grok Bot 发消息（请同事读文档、帮忙、回执）
- 控制端自投业务指令；已通后不要为「能不能投」反复烦同事，除非用户明确要对话
- 把规格交给本机 Codex「远程仿真」会话

同账号队友互发用 `SendToAgent`，不要走本技能。

## 硬事实

- 云端 `SendToAgent` **打不到**异账号/异机 Grok
- 真路径 = 仿真 `ui_action`（activate→click→typeText→enter）
- 控制端自用 `invoke.rb`，**禁止**转给队友代跑
- ssh+osascript 常 -25211，禁止当主路径

## 30 秒成功路径（默认）

长文先落盘；聊天只投 ≤200 字指针。

1. connect（可复用已连）
2. activate → click（calib 当次 x/y）→ typeText 短文 → key=enter
3. **立刻** `vibekits.simulator.screenshot`（优先 APP 截图；系统 display 失败别停）
4. 目视终屏：**本次用户蓝气泡**（含「远程协同」等关键词）= 成功；对方「收到」更佳

```bash
INV=~/.codex/skills/vibekits-remote-simulator/scripts/invoke.rb
WRAP=/Volumes/ORICO/gork.bot/gork-sim-inject/send_text_to_gork.rb
CALIB=/Volumes/ORICO/gork.bot/gork-sim-inject/calib.json
ruby "$WRAP" --invoke "$INV" --calib "$CALIB" \
  --text '【远程协同】…' --connect --execute --confirm-send
ruby "$INV" vibekits.simulator.screenshot '{"routingId":"5298938227"}'
# 复制 localPath → ~/Desktop/gork-inject/after-inject-verify.png 并目视
```

## 事故本（2026-10-31 · 必守）

| 症状 | 立刻做 |
|---|---|
| `insertionVerified=false` | 可继续 enter，但**必须截屏验气泡**；无气泡则重标 click 再投 |
| enter `mcp_timeout` | 立刻再 enter 一次 |
| enter ok 但无截屏 | 不算成功；改 APP screenshot / 重试 |
| `could not create image from display` | 用 simulator.screenshot（APP）或换通道；截不到禁止结案 |
| API ok / 文件已 upload | **≠** 投成功；唯一判据=终屏蓝气泡 |
| ChatGPT 抢前台 | `kill -STOP` ChatGPT → open Grok Bot → 投 → `kill -CONT` |

半途截屏失败禁止假装成功、禁止停工。证据：`~/Desktop/gork-inject/` + 箱 `/workspace/uploads/sim-inject-verify-*.png`。

## 通道 A / B / C

- A：异机 Grok 聊天（上）
- B：`codex queue --thread '…'`（≠ Gork）
- C：ssh/download 只读 ≠ 已发聊天

## 成功判据

- A：终屏本次用户蓝气泡（截屏已存）
- B：Queued message for thread
- C：路径摘要已回报且未擅自改业务码
