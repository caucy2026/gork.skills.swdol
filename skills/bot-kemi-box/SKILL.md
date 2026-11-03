---
name: bot云机编译，kemi box真机验证
description: >-
  在 Grok Bot 云电脑（box）按 KEMI 真机（默认 Linux 101）探测到的系统/编译环境搭镜像构建客机、编译 aarch64 产物，再经
  newlink VibeKits 仿真推真机冷启验证时使用。含摸底清单、环境对齐、glibc/Qt 策略、仿真链接与验收边界。亦覆盖云机本机跑
  vibekits：沙箱 DNS 假 IP（198.18）、ABCD hosts 双钉、Harness
  注册、consent_pending、openssh-client，以及同意后 identity_mismatch / 控制器与目标 protocol
  代差对齐。
---
# bot云机编译，kemi box真机验证

标准口径：**云机（Grok Bot box）编译 → 控制机仿真通道 → KEMI 真机验证**。默认真机为 Linux **101**（`mp730-156` / routingId `7634801763`）。用户点名其他 routingId 时，先对那台机重做「摸底」，再按同一套路搭云端编译环境。

合主线注明「gork.bot协助开发」。**验证 ≠ 编译**。禁止 `SendToAgent` 甩锅；禁止为推包停真机上的 **vibekits**。

**仿真通道优先序**：有控制机 `invoke.rb` 时优先用控制机发仿真；仅当必须在 bot 云机（常为 Docker Ubuntu20 amd64 冒烟/运行时）本机起 vibekits 时，走 **§6 云机本机跑 vibekits**（ABCD hosts / 注册 / consent）。冒烟运行时 ≠ 正式商场 amd64 `.deb` 上架包。

---

## 1. 三角色

| 角色 | 身份 | 职责 |
|---|---|---|
| **云机 / box** | Grok Bot 云电脑 | 搭与真机对齐的编译客机、编译、打包、探针；必要时也可本机跑 vibekits 作**控制端**（须先按 §6 钉 ABCD、完成注册） |
| **控制机 newlink** | `machineId=92c2f165-4f49-452d-b60c-ae24cb8eb990` | VibeKits bridge + `invoke.rb`（首选仿真入口）；大包中转；lima 备用客机 |
| **KEMI 真机 / box（设备）** | 默认 101=`mp730-156`，`7634801763` | 只部署、冷启、取证；**同意仿真请求**；其上 vibekits **禁止误杀** |

说明：口语里的「kemi box」= 真机设备；「bot 云机」= 本助手云电脑。二者都叫 box 时以本表为准。

**routingId 示例（仅示意，一律以当次 harness status 实读为准）**：

| 角色 | 常见示例 ID | 备注 |
|---|---|---|
| 默认真机 101 | `7634801763` | `mp730-156` |
| 控制机 newlink | `1554650784` | Mac 侧 vibekits，常作首选控制器 |
| bot 云机控制器 | 如 `9245390203` | 容器内新起 Harness 会换 ID；勿写死 |

---

## 2. 怎么知道真机的系统与编译相关环境（摸底）

不要猜发行版/glibc。一律经仿真连上后 **只读采集**，写成当次 `TARGET_PROFILE`（markdown 或 env）。

### 2.1 连接

**首选（控制机）**：

```bash
INV=/Users/newlink/.codex/skills/vibekits-remote-simulator/scripts/invoke.rb
RID=7634801763   # 或用户给的 ID
ruby "$INV" vibekits.simulator.connect "{\"routingId\":\"$RID\"}"
```

验收字段：`ok`、`connected:true`、`routingId` 匹配、`hostname`、`targetPlatform`、`transport`（常见 `p2p_or_relay`/`direct`）、`sshReady:true`、`sshHostKeyFingerprint`。不对账不往下推包。

若必须从 **bot 云机本机** 发仿真：先完成 **§6**（ABCD 双钉 + 注册 + 目标机同意），再用当次 tool-bridge / 协议入口 `connect`；出现 `consent_pending` 时在**目标设备 UI** 点同意，**禁止旁路**。

### 2.2 必采清单（ssh_exec 一次或拆条）

**身份 / OS**

- `hostname`；`uname -a`；`uname -m`
- `/etc/os-release`（`ID`/`VERSION_ID`/`PRETTY_NAME`）
- `/proc/version`；`cat /proc/cmdline`（看 `mem=`、厂商分区等）

**CPU / 内存 / 负载**

- `lscpu`（架构、核数、型号如 Cortex-A73、最大 MHz）
- `free -h`；`/proc/meminfo` 的 MemTotal/SwapTotal（101：**无 swap**）
- `uptime` / `loadavg`（验收时对照，避免在过载时误判卡死）

**libc / 编译器基线（决定云端客机版本）**

- `ldd --version` → **max-glibc 上限**（101：**2.31**）
- `ls -la /lib/ld-linux-aarch64.so.1`
- `strings $(find /usr/lib -name 'libstdc++.so.6' | head -1) | grep GLIBCXX | tail` → 宿主 GLIBCXX 上限（101 常到 **3.4.28**；更高需捆绑）
- 有则记：`gcc --version` / `g++ --version`（真机未必装编译器；没有就只记运行时）

**磁盘**

- `df -hT / /opt /tmp /data /home`
- `lsblk -o NAME,SIZE,TYPE,FSTYPE,MOUNTPOINT,ROTA,MODEL`
- 部署前确认目标目录可用字节 > 包体×2

**显示 / GPU（GUI 应用必采）**

- `echo DISPLAY/XDG_RUNTIME_DIR/WAYLAND`；`ls /tmp/.X11-unix`；`who`
- `ps` 找 `Xorg`/`gdm`/`lightdm`；记下 `XAUTHORITY`（101 常见 `/run/user/0/gdm/Xauthority`，`:0`）
- `ls -la /dev/dri`；`/sys/class/drm/*/status`；DRM 驱动 uevent（101：`huanglong,plat-drm`）
- `dpkg -l | grep -iE 'mesa|libegl|libgbm'`（或等价）

**网络与已有部署**

- `ip -br a`
- `ls /opt`；是否已有本项目 runtime；**vibekits 是否在跑**（PID 记入回执，禁止误杀）

### 2.3 产出物

写 `hw/<target>-profile.env` + 短证据 md。101 参考已有：

- `/Volumes/ORICO/kemi-build-cache/koffice-linux-ctrl-20261101/hw/101-koffice-opt-profile.env`
- 摸底类：`work-notes/101-df.json`、`101-probe-display.json`、`101-x11-snap.json`

101 冻结基线（2026-11 实测，改机需重采）：

- aarch64 · Ubuntu 20.04.5 · glibc **2.31** · 8×A73 · RAM≈5.5G · swap=0 · gdm3+Xorg:0 · Mesa/EGL≈21.2 · 根盘偏满

采完 `disconnect`（后续推包再连）。

---

## 3. 怎么在云端搭建「对应」的编译环境

原则：**运行 ABI 对齐真机；工具链可以更新，但产物不得要求比真机更新的 glibc（除非自带 loader+libs）。**

### 3.1 先看云机自身

在 box 上执行并记入回执：

- `uname -m`（常见 **x86_64**）
- `ldd --version`（常 **远高于** 真机，如 2.41）
- 内存/磁盘；是否已有 `docker`/`podman`/`qemu-user-static`/`binfmt`

结论：box **不能**把原生编出的 ELF 直接当 101 产物，除非已是交叉/客机输出。

### 3.2 选型矩阵（按摸底结果）

| 真机约束 | 云端编译客机 | 产物形态 |
|---|---|---|
| Ubuntu 20.04 + glibc≤2.31 + 无强 Qt 新版本 | **Ubuntu 20.04 arm64** 客机（与真机同大版本） | 普通动态链接；可捆绑更高 `libstdc++/libgcc_s` |
| 需要 Qt≥6.6 等新依赖，但真机仍 glibc 2.31 | **较新 arm64** 客机编应用 + **自包含 runtime**（捆绑 `ld-linux-aarch64.so.1` + 全部 `.so`） | 真机用 `--library-path` 启，不吃宿主 libc |
| 仅 Rust 服务等 | 按 `rust-linux-cross-build`：`cargo-zigbuild` 钉 `aarch64-unknown-linux-gnu.2.31` 一类 | 探针过即推 |

### 3.3 在 bot 云机（box）上搭客机（首选）

目标：容器或 rootfs **arch=arm64**，**用户态 glibc≤真机**（对 101 即 ≤2.31 → Ubuntu focal）。

推荐步骤：

1. 安装/启用 `qemu-user-static` + binfmt（或等价）使 x86_64 宿主能跑 arm64 容器。
2. 拉镜像：`ubuntu:20.04` 的 **arm64** 变体（勿拉错 amd64）。
3. 容器内核对：`uname -m`→`aarch64`；`ldd --version`→2.31.x；`cat /etc/os-release`→20.04。
4. 装项目构建依赖（编译器、meson/cmake、Qt 等按项目；U20 引擎常用 `gcc-12/g++-12`）。
5. 工作目录放 `/workspace/<project>-build`；源码从控制机同步或 git clone；**大量写盘前查可用字节**，结束按清理技能收尾。
6. 并发按真机内存保守设（101 参考 `KOFFICE_ENGINE_JOBS=3`），勿按云机 8 核抄满。

容器内最小自检：

```bash
uname -m
ldd --version | head -1
g++ --version | head -1   # 或项目指定编译器
free -h | head -2
```

### 3.4 控制机 lima 备用客机（客机暂缺时）

当 box 上 arm64 客机未就绪，允许在 newlink lima 代编，但回执必须写「实际编译宿主=newlink/lima/…」，编排仍属本技能。

已验证实例（`LIMA_HOME=/Volumes/ORICO/lh`）：

- **`ko-u20`**：`arch: aarch64`，Ubuntu 20.04 cloudimg arm64，挂载 `/koffice-src`、`/koffice-cache`，为 **glibc 2.31 ABI 对齐 101**。
- **`ko-r26`**：Ubuntu 26.04 arm64，给 **coda-qt / Qt≥6.6**；产物必须打成自带 loader 的 runtime。

参考 yaml：`/Volumes/ORICO/lh/ko-u20/lima.yaml`、`…/ko-r26/lima.yaml`。脚本：`…/koffice-linux-ctrl-20261101/scripts/u20-build-koffice.sh` 等。

### 3.5 环境对齐检查表（开编前勾）

- [ ] 客机 arch == 真机 arch（aarch64）
- [ ] 客机 `ldd` ≤ 真机 glibc，**或**已规划自包含 runtime
- [ ] 已知真机 GLIBCXX 上限；超出则打包进 `program/` 或 `runtime/lib`
- [ ] GUI：已知 DISPLAY/XAUTH/DRM；运行 env 含 `QT_QPA_PLATFORM=xcb`、`QT_OPENGL=egl`（101）
- [ ] 代理/apt：按控制机网络习惯配置（lima 曾用 `http://192.168.5.2:7897`）；box 按当次可用代理
- [ ] 输出目录与 SHA 旁路文件路径已定

---

## 4. 编译 → 探针 → 打包

1. 冻结源码 commit / 脏文件列表。
2. 在对齐客机内编译。
3. **推真机前探针**（失败禁止上传）：

```bash
python3 <probe_elf.py> --target-arch arm64 --max-glibc 2.31 \
  --elf <main> --elf <key.so...> --report /tmp/probe.json
```

探针：`~/.codex/skills/linux-desktop-build-acceptance/scripts/probe_elf.py` 或缓存 `…/scripts/probe_elf.py`。

4. `tar` + `zstd` + `.sha256`。

koffice 参考：

- LOKit lean：U20 + `--disable-gui`；101 部署 `/opt/koffice-lokit-r5b-20261101/instdir-lean`
- coda-qt runtime：`out/coda-qt-runtime-*.tar.zst` → `/opt/koffice-coda-qt-20261102/coda-qt-runtime`
- 证据：`KOFFICE-CODA-QT-101-COLD-20261102.md`（窗口级 cold pass；WebEngine 内页未宣称）

---

## 5. 产物送达控制机 → 仿真推真机

`upload_file.localPath` 在 **newlink** 解析。

- ≤100MiB：`CopyFromBox` → newlink 缓存目录
- 更大：分卷 / 临时可拉通道 / 在 newlink 卷上完成终包；回执写中转与 SHA256

```bash
RID=7634801763
ruby "$INV" vibekits.simulator.connect "{\"routingId\":\"$RID\"}"
ruby "$INV" vibekits.simulator.upload_file "{\"routingId\":\"$RID\",\"localPath\":\"/abs/on/newlink/pkg.tar.zst\"}"
ruby "$INV" vibekits.simulator.ssh_exec "{\"routingId\":\"$RID\",\"command\":\"...\"}"
ruby "$INV" vibekits.simulator.disconnect "{\"routingId\":\"$RID\"}"
```

备选：已有 `vibekits-harness-tunnel $RID` 时用 `scp -P LPORT`（见 `push-koffice-to-101.sh`）；断隧回 invoke。

通道是 **VibeKits 仿真**，不是公网手写 SSH / 远程桌面。

若仿真入口改在 **bot 云机本机**（无控制机 invoke、或专门验证云端控制器），先按 **§6** 打通注册与 consent，再等价执行 connect / upload / ssh_exec；验收标准不变。

---

## 6. 云机本机跑 vibekits（ABCD DNS · 注册 · consent）

适用：必须在 Grok Bot box 上起 vibekits / harness-relay（常见 Docker Ubuntu 20.04 **amd64** 冒烟或运行时容器）再连 KEMI 真机。同事可按下列清单顺序做；**验证 ≠ 编译**；**正式商场 amd64 `.deb` 暂存/上架与冒烟运行时分开**，勿把 smoke 拼装当上架包。

### 6.1 根因速记（为何会卡死）

bot 侧常开 `SAND_EGRESS`：容器 DNS（如 `nameserver 10.0.0.2`）把 KEMI 集群主机名解析成假 IP **`198.18.0.1`**。Harness ESTAB 到 `198.18.0.1:21116` 看起来「通」，但 HBBS 永不成功 → `state=offline` → Dart 侧 `HARNESS_REGISTRATION_PENDING` / 控制器 offline。

关键点：**容器有独立 `/etc/hosts`，不继承宿主机 pins**。只钉宿主机不够；容器重建时须 `--add-host` 或启动脚本重写容器内 hosts。

### 6.2 清单 A — 钉 ABCD（宿主机 + 容器各一份）

| 节点 | 主机名 | 真 IP | HBBS（常见） |
|---|---|---|---|
| **A** | `kemi-chat1.newlinksz.com`（兼 `kemi-chat.newlinksz.com`） | `119.96.24.110` | `:21116`（relay 常 `:21117`） |
| **B** | `kemi.newlinksz.com` | `171.43.138.47` | `:23116` |
| **C** | `kemi-sh1.newlinksz.com` | `159.27.117.207` | `:24116` |
| **D** | `kemi-sg1.newlinksz.com` | `172.188.240.186` | `:21116` |

容器/宿主机 `/etc/hosts` 片段示例（加注释标记便于复跑识别）：

```
# vibekits-kemi-abcd
119.96.24.110 kemi-chat1.newlinksz.com kemi-chat.newlinksz.com
171.43.138.47 kemi.newlinksz.com
159.27.117.207 kemi-sh1.newlinksz.com
172.188.240.186 kemi-sg1.newlinksz.com
```

勾选：

- [ ] 宿主机 `/etc/hosts` 已含 ABCD 四套
- [ ] **容器内** `/etc/hosts` 已含同一套（`getent hosts kemi-chat1.newlinksz.com` 等不得再出 `198.18.0.1`）
- [ ] 四节点关键端口探测 OK（至少 A 的 21116）
- [ ] 当前集群配置与预期一致（例：`kemi-cluster-node=a`，`custom-rendezvous-server=kemi-chat1.newlinksz.com:21116`）

### 6.3 清单 B — 重启 bot 侧并验收注册

只重启 **bot 侧** `vibekits` + `vibekits-harness-relay`（带 simulator-access）；**不要**为调试杀目标真机上的 vibekits。

建议验收（字段名以当次 status JSON 为准）：

- [ ] `state=registered`（不是 `offline` / 长期 `registration_pending`）
- [ ] `rendezvousOnline=true` · `callable=true` · `registrationKeyConfirmed=true`（或等价）
- [ ] `dnsRegion` / `registeredNode` 合理（国内常见挂 **A**）
- [ ] 本机 `routingId` 已记下（例示 `9245390203`，**以实读为准**）
- [ ] 出站 ESTAB 指向**真 IP**（如 `119.96.24.110:21116`），**禁止**仍 ESTAB 到 `198.18.0.1`
- [ ] 协议侧 simulator-access 已开（如 `simulatorAccessEnabled=true`）；记下 `protocol_vN`，与目标机代差过大时先对齐再连

冒烟拼装注意：relay 子进程可能需单独带 `LD_LIBRARY_PATH=<runtime>/lib` 拉起 `--vibekits-harness-service`；钥匙环早期 `prompting failed` 在 DNS 修通后常可自愈，勿先当主因。

### 6.4 清单 C — 容器依赖 `openssh-client`

仿真 connect 需要 `ssh-keygen`。精简 Ubuntu 容器常缺：

```bash
# 容器内
apt-get update && apt-get install -y openssh-client
command -v ssh-keygen
```

缺则报 `missing_ssh_keygen`（或同类），与注册无关，装上即可。

### 6.5 清单 D — 首次仿真与 `consent_pending`

新控制器 `routingId` 第一次连目标机时，目标常返回：

- `code=consent_pending` / 「等待对方确认仿真请求」
- context 含 `controllerRoutingId`、`targetRoutingId`、`requestId`、`authorizationRevision`

同事动作：

1. **在目标设备 UI**（如 101 / 控制机 Mac）对来自该 controller 的仿真请求点**同意**。
2. **禁止旁路 consent**；禁止改目标授权库「骗过」；禁止杀目标 vibekits。
3. 若用户说「目标机没收到申请」：先看目标机 vibekits 是否被**其它模态挡住**（ConsentHost 仅在 `ModalRoute.isCurrent` 时弹「允许此设备仿真访问？」；应用中心详情等弹窗会挡住）。关掉挡板或切到前台后再等 pending；也可先连**控制机本机** UI 验通同意链路。
4. `consent_pending`  alone ≠ 弹窗已出现：用目标侧 `--vibekits-harness-consent-presence` / protocol 核对 `pendingConsentRequests` 与 ledger。
5. 同意后再 `connect` 同一 `routingId`；ledger 应出现该 controller 的 `decision=allow`。

### 6.5.1 同意后仍 `identity_mismatch`（停盲目重试）

若 ledger 已 allow，但 `connect` 报 `identity_mismatch`（常见阶段文案含 `notifications/initialized`）：

- **立即停止盲目重试**（合同：身份回归不得靠反复 connect / 清数据 / 重授权掩盖）。
- 对照两侧 harness：`protocol_vN`、appBuild、native 公钥指纹、routingId。云机 **冒烟旧包**（如 protocol_v3 / 2772）连较新本机（如 protocol_v4 / 2832）时，易在 MCP 初始化后身份校验失败。
- 处置：把云机控制器升到与目标**同代或更新**的 vibekits/relay 后再连；升包前后重读 status，确认 ESTAB 仍为真 IP、ABCD 仍钉在容器内。
- 回执写清：同意已成功 vs 仿针未通、两侧 protocol/build、是否已升包。

### 6.6 验收（云机控制器路径）

```text
connect → ok / connected=true / sshReady=true / hostname 与目标一致
ssh_exec → hostname; uname -m
```

通过后即可按 §2 摸底或 §5/§7 推包冷启。`disconnect` 收尾。回执写：控制器 routingId、目标 routingId、registeredNode、是否经 consent、ESTAB 真 IP。

### 6.7 与正式包的边界

- 云机 Docker **冒烟/拼装运行时**（如某次 AOT + 商场 relay 混用）只用于通路与编译验证。
- **正式市场 amd64 `.deb` 暂存/同步是另一条线**；未做则回执写明「未上架 / 非正式包」。

---

## 7. 真机部署与冷启验证

1. 解压到 `/opt/<app>-<YYYYMMDD>/...`，勿动 vibekits 树。
2. source 硬件优化 env；GUI：`DISPLAY=:0` + 真实 `XAUTHORITY`。
3. 启动：
   - ABI 已对齐：直接跑主二进制；
   - 自包含 runtime：

```bash
"$APP/lib/ld-linux-aarch64.so.1" --library-path "$APP/lib" "$APP/bin/<main>"
```

4. 最低验收：进程存活（如 ≥20s）+（GUI）X11 窗口；记 PID、RSS、标题、主二进制 SHA256。
5. 证据写入控制机与继承包 `work-notes/`；`disconnect`。
6. 回执区分 **passed（冷启）** vs 未测项（内页/发布/流畅度等）。

传测试文件/测 app：同一 `upload_file` + `ssh_exec`；安装范围以当次授权为准。

---

## 8. 硬件优化默认（101）

```bash
export KOFFICE_TARGET_HOSTNAME=mp730-156
export KOFFICE_TARGET_ROUTING_ID=7634801763
export KOFFICE_TARGET_ARCH=aarch64
export KOFFICE_ENGINE_JOBS=3
export KOFFICE_ONLINE_JOBS=2
export KOFFICE_RENDER_WORKERS=2
export KOFFICE_CACHE_BUDGET_MB=256
export KOFFICE_DISABLE_SWAP_ASSUMPTION=1
export KOFFICE_PREFER_EGL=1
export QT_OPENGL=egl
export QT_QPA_PLATFORM=xcb
export KOFFICE_LO_DISTRO=CPLinux-LOKit
```

---

## 9. 清理与最短口令

谁产生构建树谁收尾：`kemi-storage-cleanup` / `cloud-build-cleanup`。无清理回执不称全部完成。

1. 仿真摸底 → 冻结真机 profile（尤其 arch/glibc/显示）。**优先控制机 `invoke.rb`**；若从 bot 云机本机连 → 先走 **§6**（宿主机+容器钉 ABCD → 重启注册到真 IP → 装 `openssh-client` → 目标 UI 同意 `consent_pending` → `connect` + `ssh_exec 'hostname; uname -m'`）。
2. 在 **bot 云机** 搭对齐客机（或回执声明的 lima 备用）并自检。
3. 编译 → probe_elf → 打包。
4. 产物到 newlink（或已打通的云机控制器通道）→ 仿真推 101 → 冷启取证 → disconnect。
5. 不停目标 vibekits；不旁路 consent；`identity_mismatch` 先对齐 protocol/build 再连；不宣称未测项；冒烟 runtime ≠ 正式 deb；合主线「gork.bot协助开发」。
