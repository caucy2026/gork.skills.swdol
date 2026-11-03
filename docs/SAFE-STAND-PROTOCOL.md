# SAFE-STAND-PROTOCOL

- 时间：2026-11-01 17:58:17 Asia/Shanghai
- 承接：`PRE-SIM-MANA-DRAIN.md` VERDICT=PASS_FOR_SAFE_STAND_PROTOCOL
- CONFIRMED_BUG=0
- **VERDICT: PASS**

## 0. 目标与禁令

- 多号登录 → Zone/合法手段进 section17 → 落 SAFE≈(62500,58800) 距Boss≈51790
- 验证全员不在四点/招唤结算圈（d>12000）
- 九月风脉冲骨架：进 900<d≤2000 再撤（本轮不诱满125）
- **禁贴脸开团、禁直冲 oid=20748**

## 1. 几何

| Boss | [11571.0, 49362.0] |
| SAFE | [62500.0, 6239.0, 58800.0] |
| LURE(1900.0) | [13439.191765877733, 49708.2073452523] |
| Zone SAFE | `~Zone 17 625 588` |
| Zone LURE | `~Zone 17 134 497` |
| 圈 | {'longben': 6000, 'longxiao_area': 2000, 'zhaohuan': 12000, 'touch': 900} |

## 2. Preflight

```json
{
  "refuse": {
    "ok": true,
    "refuse_in_tail": 0,
    "threshold": 50
  }
}
```

## 3. 队员（act / dist / 圈外）

| 账号 | 职 | act | distBoss | 圈外(>12000) | bossAOI |
|---|---|---|---|---|---|
| helen1 | 药师 | 17 | 51796.1 | True | False |
| jiuyue | 武术家 | 17 | 51796.1 | True | False |
| nana | 阴阳 | 17 | 51953.5 | True | False |
| helen | 修罗 | 17 | 51810.8 | True | False |
| A88888 | 刺客 | 17 | 51889.4 | True | False |
| nana1 | 祭符 | 17 | 51968.1 | True | False |

## 4. Zone / 脉冲

- pulse: `{"plan": {"actor": "jiuyue/九月风", "enter": {"d_window": "900<d<=2000", "target_d": 1900.0, "xz": [13439.191765877733, 49708.2073452523], "zone": "~Zone 17 134 497"}, "exit": {"d": ">12000.0", "zone": "~Zone 17 625 588"}, "note": "本轮只证骨架1次脉冲，不诱满125；禁边缘长站"}}`
- pulse: `{"step": "enter", "label": "pulse-enter-lure", "ok": true, "means": "handoff-0023", "cmd": "~Zone 17 134 497", "before": {"section": "(17, 17)", "pos": [62580, 6238, 58800]}, "section": "(17, 17)", "pos": [13400, 5824, 49700], "dist_boss": 1859.9690857645994, "boss_in_aoi": true, "tells": [], "zone_admission": ["ZONE_ADMISSION_GROUND_418FB0 section=17 in=13400,0,49700 out=13400,5824,49700 samples=`
- pulse: `{"step": "withdraw", "label": "pulse-withdraw-safe", "ok": true, "means": "handoff-0023", "cmd": "~Zone 17 625 588", "before": {"section": "(17, 17)", "pos": [13400, 5824, 49700]}, "section": "(17, 17)", "pos": [62500, 6016, 58800], "dist_boss": 51796.12808888325, "boss_in_aoi": false, "tells": [], "zone_admission": ["ZONE_ADMISSION_GROUND_418FB0 section=17 in=62500,0,58800 out=62500,6016,58800 sa`
- pulse: `{"step": "verdict", "enter_in_window": true, "enter_dist": 1859.9690857645994, "withdraw_dist": 51796.12808888325, "withdraw_outside_zhaohuan": true, "boss_attacked": false, "face_rush_forbidden": true, "note": "patched: safe() substring raw stripped withdraw_* keys; values from enter/withdraw steps"}`

## 5. 金刚护脉 / 见招拆招

- `{"label": "金刚护脉-self", "slot": 50, "magic": 10628, "target": 148, "status": "sent", "req": ["SPELL_REQUEST_0283 principal=1 roleSlot=0 learnedSlot=50 target=148 pending=0"], "rej": [], "ok_lines": ["SPELL_REQUEST_0283 principal=1 roleSlot=0 learnedSlot=50 target=148 pending=0", "SPELL_TIMING magic=1`
- `{"stage": "见招拆招", "status": "OPEN", "reason": "id-not-in-denom"}`

## 6. 计数

```json
{
  "logged_ok": 6,
  "act17": 6,
  "safe_outside_zhaohuan": 6,
  "boss_aoi": 0
}
```

## 7. 证据

- `summary.json`（本轮机器摘要）
- `park-safe-stand.json`
- `PRE-SIM-MANA-DRAIN.md` / `PRE-SIM-SURVIVAL.md`
- `run_safe_stand_protocol.py`
