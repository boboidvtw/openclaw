# 🦞 Clawdbot — 個人 AI 助理

<p align="center">
  <img src="https://raw.githubusercontent.com/clawdbot/clawdbot/main/docs/whatsapp-clawd.jpg" alt="Clawdbot" width="400">
</p>

<p align="center">
  <strong>EXFOLIATE! EXFOLIATE!</strong>
</p>

<p align="center">
  <a href="https://github.com/clawdbot/clawdbot/actions/workflows/ci.yml?branch=main"><img src="https://img.shields.io/github/actions/workflow/status/clawdbot/clawdbot/ci.yml?branch=main&style=for-the-badge" alt="CI status"></a>
  <a href="https://github.com/clawdbot/clawdbot/releases"><img src="https://img.shields.io/github/v/release/clawdbot/clawdbot?include_prereleases&style=for-the-badge" alt="GitHub release"></a>
  <a href="https://deepwiki.com/clawdbot/clawdbot"><img src="https://img.shields.io/badge/DeepWiki-clawdbot-111111?style=for-the-badge" alt="DeepWiki"></a>
  <a href="https://discord.gg/clawd"><img src="https://img.shields.io/discord/1456350064065904867?label=Discord&logo=discord&logoColor=white&color=5865F2&style=for-the-badge" alt="Discord"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-blue.svg?style=for-the-badge" alt="MIT License"></a>
</p>

**Clawdbot** 是一個在你自己的設備上運行的 _個人 AI 助理_。
它會在你已經使用的頻道上回應你（WhatsApp, Telegram, Slack, Discord, Google Chat, Signal, iMessage, Microsoft Teams, WebChat），還包括像 BlueBubbles, Matrix, Zalo 和 Zalo Personal 等擴充頻道。它可以在 macOS/iOS/Android 上進行語音對話，並且可以渲染一個你控制的即時 Canvas。Gateway 只是控制平面 (Control Plane) —— 助理本身才是產品。

如果你想要一個感覺本地端運行、快速且隨時待命的個人單用戶助理，這就是你的選擇。

[網站](https://clawdbot.com) · [文件](https://docs.clawd.bot) · [入門](https://docs.clawd.bot/start/getting-started) · [更新](https://docs.clawd.bot/install/updating) · [展示](https://docs.clawd.bot/start/showcase) · [常見問題](https://docs.clawd.bot/start/faq) · [嚮導](https://docs.clawd.bot/start/wizard) · [Nix](https://github.com/clawdbot/nix-clawdbot) · [Docker](https://docs.clawd.bot/install/docker) · [Discord](https://discord.gg/clawd)

建議設置：運行初始化嚮導 (`clawdbot onboard`)。它會引導你完成 Gateway、Workspace、Channels 和 Skills 的設置。CLI 嚮導是推薦的路徑，適用於 **macOS, Linux 和 Windows (via WSL2; 強烈推薦)**。
支援 npm, pnpm, 或 bun。
新安裝？從這裡開始：[入門指南](https://docs.clawd.bot/start/getting-started)

**訂閱 (OAuth):**

- **[Anthropic](https://www.anthropic.com/)** (Claude Pro/Max)
- **[OpenAI](https://openai.com/)** (ChatGPT/Codex)

模型說明：雖然支援任何模型，但我強烈建議使用 **Anthropic Pro/Max (100/200) + Opus 4.5** 以獲得長上下文優勢和更好的 Prompt Injection 防禦。詳見 [Onboarding](https://docs.clawd.bot/start/onboarding)。

## Models (選擇 + 驗證)

- Config 模型 + CLI: [Models](https://docs.clawd.bot/concepts/models)
- Auth profile 輪替 (OAuth vs API keys) + failbacks: [Model failover](https://docs.clawd.bot/concepts/model-failover)

## Install (推薦)

Runtime: **Node ≥22**。

```bash
npm install -g clawdbot@latest
# 或: pnpm add -g clawdbot@latest

clawdbot onboard --install-daemon
```

該嚮導會安裝 Gateway daemon (launchd/systemd user service)，使其保持運行。

## Quick start (TL;DR)

Runtime: **Node ≥22**。

完整新手指南 (auth, pairing, channels): [入門指南](https://docs.clawd.bot/start/getting-started)

```bash
clawdbot onboard --install-daemon

clawdbot gateway --port 18789 --verbose

# 發送訊息
clawdbot message send --to +1234567890 --message "Hello from Clawdbot"

# 與助理對話 (可選傳送回任何已連接的頻道: WhatsApp/Telegram/Slack/Discord/Google Chat/Signal/iMessage/BlueBubbles/Microsoft Teams/Matrix/Zalo/Zalo Personal/WebChat)
clawdbot agent --message "Ship checklist" --thinking high
```

升級？[更新指南](https://docs.clawd.bot/install/updating) (並運行 `clawdbot doctor`)。

## Development channels

- **stable**: 標記版本 (`vYYYY.M.D` 或 `vYYYY.M.D-<patch>`)，npm dist-tag `latest`。
- **beta**: 預發布標籤 (`vYYYY.M.D-beta.N`)，npm dist-tag `beta` (macOS app 可能缺失)。
- **dev**: `main` 分支的移動頭部，npm dist-tag `dev` (發布時)。

切換頻道 (git + npm): `clawdbot update --channel stable|beta|dev`。
詳情：[開發頻道](https://docs.clawd.bot/install/development-channels)。

## From source (開發)

從源碼構建請優先使用 `pnpm`。Bun 可選用於直接運行 TypeScript。

```bash
git clone https://github.com/clawdbot/clawdbot.git
cd clawdbot

pnpm install
pnpm ui:build # 首次運行時自動安裝 UI 依賴
pnpm build

pnpm clawdbot onboard --install-daemon

# Dev loop (TS 變更自動重載)
pnpm gateway:watch
```

注意：`pnpm clawdbot ...` 直接運行 TypeScript (via `tsx`)。`pnpm build` 產生 `dist/` 用於透過 Node / 打包好的 `clawdbot` binary 運行。

## Security defaults (DM 存取)

Clawdbot 連接到真實的訊息介面。請將傳入的 DM 視為 **不可信輸入 (Untrusted Input)**。

完整安全指南：[Security](https://docs.clawd.bot/gateway/security)

Telegram/WhatsApp/Signal/iMessage/Microsoft Teams/Discord/Google Chat/Slack 上的預設行為：

- **DM pairing** (`dmPolicy="pairing"` / `channels.discord.dm.policy="pairing"` / `channels.slack.dm.policy="pairing"`): 未知發送者會收到一個簡短的配對碼，Bot 不會處理他們的訊息。
- 批准指令：`clawdbot pairing approve <channel> <code>` (然後發送者會被加入本地白名單 store)。
- 公開傳入 DM 需要明確 opt-in：設置 `dmPolicy="open"` 並在頻道白名單中包含 `"*"` (`allowFrom` / `channels.discord.dm.allowFrom` / `channels.slack.dm.allowFrom`)。

運行 `clawdbot doctor` 來顯示有風險/配置錯誤的 DM 策略。

## Highlights

- **[Local-first Gateway](https://docs.clawd.bot/gateway)** — 用於 sessions, channels, tools 和 events 的單一控制平面 (Control Plane)。
- **[Multi-channel inbox](https://docs.clawd.bot/channels)** — 支援 WhatsApp, Telegram, Slack, Discord, Google Chat, Signal, iMessage, BlueBubbles, Microsoft Teams, Matrix, Zalo, Zalo Personal, WebChat, macOS, iOS/Android。
- **[Multi-agent routing](https://docs.clawd.bot/gateway/configuration)** — 將傳入 channels/accounts/peers 路由到隔離的 agents (workspaces + per-agent sessions)。
- **[Voice Wake](https://docs.clawd.bot/nodes/voicewake) + [Talk Mode](https://docs.clawd.bot/nodes/talk)** — macOS/iOS/Android 的全時語音與 ElevenLabs 集成。
- **[Live Canvas](https://docs.clawd.bot/platforms/mac/canvas)** — 基於 [A2UI](https://docs.clawd.bot/platforms/mac/canvas#canvas-a2ui) 的 Agent 驅動視覺化工作區。
- **[First-class tools](https://docs.clawd.bot/tools)** — browser, canvas, nodes, cron, sessions, 以及 Discord/Slack actions。
- **[Companion apps](https://docs.clawd.bot/platforms/macos)** — macOS 選單列應用程式 + iOS/Android [nodes](https://docs.clawd.bot/nodes)。
- **[Onboarding](https://docs.clawd.bot/start/wizard) + [skills](https://docs.clawd.bot/tools/skills)** — 嚮導式設定與 bundled/managed/workspace skills。

## Star History

[![Star History Chart](https://api.star-history.com/svg?repos=clawdbot/clawdbot&type=date&legend=top-left)](https://www.star-history.com/#clawdbot/clawdbot&type=date&legend=top-left)

## Everything we built so far

### Core platform

- [Gateway WS control plane](https://docs.clawd.bot/gateway) — 具備 sessions, presence, config, cron, webhooks, [Control UI](https://docs.clawd.bot/web), 和 [Canvas host](https://docs.clawd.bot/platforms/mac/canvas#canvas-a2ui)。
- [CLI surface](https://docs.clawd.bot/tools/agent-send): gateway, agent, send, [wizard](https://docs.clawd.bot/start/wizard), 和 [doctor](https://docs.clawd.bot/gateway/doctor)。
- [Pi agent runtime](https://docs.clawd.bot/concepts/agent) — RPC 模式，支援 tool streaming 和 block streaming。
- [Session model](https://docs.clawd.bot/concepts/session): `main` 用於直接聊天, group isolation, activation modes, queue modes, reply-back。群組規則：[Groups](https://docs.clawd.bot/concepts/groups)。
- [Media pipeline](https://docs.clawd.bot/nodes/images): images/audio/video, transcription hooks, size caps, temp file lifecycle。音訊詳情：[Audio](https://docs.clawd.bot/nodes/audio)。

### Channels

- [Channels](https://docs.clawd.bot/channels): [WhatsApp](https://docs.clawd.bot/channels/whatsapp) (Baileys), [Telegram](https://docs.clawd.bot/channels/telegram) (grammY), [Slack](https://docs.clawd.bot/channels/slack) (Bolt), [Discord](https://docs.clawd.bot/channels/discord) (discord.js), [Google Chat](https://docs.clawd.bot/channels/googlechat) (Chat API), [Signal](https://docs.clawd.bot/channels/signal) (signal-cli), [iMessage](https://docs.clawd.bot/channels/imessage) (imsg), [BlueBubbles](https://docs.clawd.bot/channels/bluebubbles) (extension), [Microsoft Teams](https://docs.clawd.bot/channels/msteams) (extension), [Matrix](https://docs.clawd.bot/channels/matrix) (extension), [Zalo](https://docs.clawd.bot/channels/zalo) (extension), [Zalo Personal](https://docs.clawd.bot/channels/zalouser) (extension), [WebChat](https://docs.clawd.bot/web/webchat)。
- [Group routing](https://docs.clawd.bot/concepts/group-messages): mention gating, reply tags, per-channel chunking 和 routing。頻道規則：[Channels](https://docs.clawd.bot/channels)。

### Apps + nodes

- [macOS app](https://docs.clawd.bot/platforms/macos): 選單列控制平面, [Voice Wake](https://docs.clawd.bot/nodes/voicewake)/PTT, [Talk Mode](https://docs.clawd.bot/nodes/talk) overlay, [WebChat](https://docs.clawd.bot/web/webchat), debug tools, [remote gateway](https://docs.clawd.bot/gateway/remote) control。
- [iOS node](https://docs.clawd.bot/platforms/ios): [Canvas](https://docs.clawd.bot/platforms/mac/canvas), [Voice Wake](https://docs.clawd.bot/nodes/voicewake), [Talk Mode](https://docs.clawd.bot/nodes/talk), camera, screen recording, Bonjour pairing。
- [Android node](https://docs.clawd.bot/platforms/android): [Canvas](https://docs.clawd.bot/platforms/mac/canvas), [Talk Mode](https://docs.clawd.bot/nodes/talk), camera, screen recording, 可選 SMS。
- [macOS node mode](https://docs.clawd.bot/nodes): system.run/notify + canvas/camera exposure。

### Tools + automation

- [Browser control](https://docs.clawd.bot/tools/browser): 專用 clawd Chrome/Chromium, snapshots, actions, uploads, profiles。
- [Canvas](https://docs.clawd.bot/platforms/mac/canvas): [A2UI](https://docs.clawd.bot/platforms/mac/canvas#canvas-a2ui) push/reset, eval, snapshot。
- [Nodes](https://docs.clawd.bot/nodes): camera snap/clip, screen record, [location.get](https://docs.clawd.bot/nodes/location-command), notifications。
- [Cron + wakeups](https://docs.clawd.bot/automation/cron-jobs); [webhooks](https://docs.clawd.bot/automation/webhook); [Gmail Pub/Sub](https://docs.clawd.bot/automation/gmail-pubsub)。
- [Skills platform](https://docs.clawd.bot/tools/skills): bundled, managed, 和 workspace skills，具備安裝閘道 + UI。

### Runtime + safety

- [Channel routing](https://docs.clawd.bot/concepts/channel-routing), [retry policy](https://docs.clawd.bot/concepts/retry), 和 [streaming/chunking](https://docs.clawd.bot/concepts/streaming)。
- [Presence](https://docs.clawd.bot/concepts/presence), [typing indicators](https://docs.clawd.bot/concepts/typing-indicators), 和 [usage tracking](https://docs.clawd.bot/concepts/usage-tracking)。
- [Models](https://docs.clawd.bot/concepts/models), [model failover](https://docs.clawd.bot/concepts/model-failover), 和 [session pruning](https://docs.clawd.bot/concepts/session-pruning)。
- [Security](https://docs.clawd.bot/gateway/security) 和 [troubleshooting](https://docs.clawd.bot/channels/troubleshooting)。

### Ops + packaging

- [Control UI](https://docs.clawd.bot/web) + [WebChat](https://docs.clawd.bot/web/webchat) 直接由 Gateway 提供服務。
- [Tailscale Serve/Funnel](https://docs.clawd.bot/gateway/tailscale) 或 [SSH tunnels](https://docs.clawd.bot/gateway/remote) (支援 token/password auth)。
- [Nix mode](https://docs.clawd.bot/install/nix) 用於聲明式配置; [Docker](https://docs.clawd.bot/install/docker) 基礎安裝。
- [Doctor](https://docs.clawd.bot/gateway/doctor) 遷移, [logging](https://docs.clawd.bot/logging)。

## How it works (簡述)

```
WhatsApp / Telegram / Slack / Discord / Google Chat / Signal / iMessage / BlueBubbles / Microsoft Teams / Matrix / Zalo / Zalo Personal / WebChat
               │
               ▼
┌───────────────────────────────┐
│            Gateway            │
│       (control plane)         │
│     ws://127.0.0.1:18789      │
└──────────────┬────────────────┘
               │
               ├─ Pi agent (RPC)
               ├─ CLI (clawdbot …)
               ├─ WebChat UI
               ├─ macOS app
               └─ iOS / Android nodes
```

## Key subsystems

- **[Gateway WebSocket network](https://docs.clawd.bot/concepts/architecture)** — 客戶端、工具和事件的單一 WS 控制平面 (加上 ops: [Gateway runbook](https://docs.clawd.bot/gateway))。
- **[Tailscale exposure](https://docs.clawd.bot/gateway/tailscale)** — Serve/Funnel 用於 Gateway dashboard + WS (遠端訪問: [Remote](https://docs.clawd.bot/gateway/remote))。
- **[Browser control](https://docs.clawd.bot/tools/browser)** — clawd 管理的 Chrome/Chromium (CDP control)。
- **[Canvas + A2UI](https://docs.clawd.bot/platforms/mac/canvas)** — Agent 驅動的視覺化工作區 (A2UI host: [Canvas/A2UI](https://docs.clawd.bot/platforms/mac/canvas#canvas-a2ui))。
- **[Voice Wake](https://docs.clawd.bot/nodes/voicewake) + [Talk Mode](https://docs.clawd.bot/nodes/talk)** — 隨時待命的語音與連續對話功能。
- **[Nodes](https://docs.clawd.bot/nodes)** — Canvas, camera snap/clip, screen record, `location.get`, notifications, 加上 macOS 專用的 `system.run`/`system.notify`。

## Tailscale access (Gateway dashboard)

Clawdbot 可以自動配置 Tailscale **Serve** (僅 tailnet) 或 **Funnel** (公開)，同時 Gateway 保持綁定到 loopback。配置 `gateway.tailscale.mode`:

- `off`: 無 Tailscale 自動化 (預設)。
- `serve`: 通過 `tailscale serve` 提供僅限 tailnet 的 HTTPS (預設使用 Tailscale identity headers)。
- `funnel`: 通過 `tailscale funnel` 提供公開 HTTPS (需要共享密碼驗證)。

注意：

- 當啟用 Serve/Funnel 時，`gateway.bind` 必須保持為 `loopback` (Clawdbot 會強制執行)。
- Serve 可以通過設置 `gateway.auth.mode: "password"` 或 `gateway.auth.allowTailscale: false` 強制要求密碼。
- 除非設置了 `gateway.auth.mode: "password"`，否則 Funnel 拒絕啟動。
- 可選: `gateway.tailscale.resetOnExit` 在關閉時撤銷 Serve/Funnel。

詳情：[Tailscale guide](https://docs.clawd.bot/gateway/tailscale) · [Web surfaces](https://docs.clawd.bot/web)

## Remote Gateway (Linux 也很棒)

在小型 Linux 實例上運行 Gateway 是完全可行的。客戶端 (macOS app, CLI, WebChat) 可以通過 **Tailscale Serve/Funnel** 或 **SSH tunnels** 連接，你仍然可以配對設備節點 (macOS/iOS/Android) 以在需要時執行設備本地操作。

- **Gateway host** 預設運行 exec tool 和 channel connections。
- **Device nodes** 通過 `node.invoke` 運行設備本地操作 (`system.run`, camera, screen recording, notifications)。
  簡而言之：exec 在 Gateway 所在處運行；設備操作在設備所在處運行。

詳情：[Remote access](https://docs.clawd.bot/gateway/remote) · [Nodes](https://docs.clawd.bot/nodes) · [Security](https://docs.clawd.bot/gateway/security)

## macOS permissions via the Gateway protocol

macOS app 可以運行在 **node mode** 並通過 Gateway WebSocket 廣播其 capabilities + permission map (`node.list` / `node.describe`)。客戶端隨後可以通過 `node.invoke` 執行本地操作：

- `system.run` 運行本地命令並回傳 stdout/stderr/exit code; 設置 `needsScreenRecording: true` 以要求螢幕錄製權限 (否則會獲得 `PERMISSION_MISSING`)。
- `system.notify` 發送用戶通知，如果通知被拒絕則失敗。
- `canvas.*`, `camera.*`, `screen.record`, 和 `location.get` 也是通過 `node.invoke` 路由並遵循 TCC 權限狀態。

Elevated bash (host permissions) 與 macOS TCC 是分開的：

- 使用 `/elevated on|off` 來切換 per‑session elevated access (當启用 + allowlisted 時)。
- Gateway 通過 `sessions.patch` (WS method) 持久化 per‑session toggle，與 `thinkingLevel`, `verboseLevel`, `model`, `sendPolicy`, 和 `groupActivation` 一起。

詳情：[Nodes](https://docs.clawd.bot/nodes) · [macOS app](https://docs.clawd.bot/platforms/macos) · [Gateway protocol](https://docs.clawd.bot/concepts/architecture)

## Agent to Agent (sessions\_\* tools)

- 使用這些工具在 sessions 之間協作，無需在聊天介面之間跳轉。
- `sessions_list` — 發現活躍的 sessions (agents) 及其 metadata。
- `sessions_history` — 獲取 session 的 transcript logs。
- `sessions_send` — 向另一個 session 發送訊息; 可選 reply‑back ping‑pong + announce step (`REPLY_SKIP`, `ANNOUNCE_SKIP`)。

詳情：[Session tools](https://docs.clawd.bot/concepts/session-tool)

## Skills registry (ClawdHub)

ClawdHub 是一個極簡的 skill registry。啟用 ClawdHub 後，agent 可以自動搜尋 skills 並按需拉取新的 skills。

[ClawdHub](https://ClawdHub.com)

## Chat commands

在 WhatsApp/Telegram/Slack/Google Chat/Microsoft Teams/WebChat 中發送這些指令 (群組指令僅限 owner)：

- `/status` — 精簡的 session 狀態 (model + tokens, cost [如有])
- `/new` 或 `/reset` — 重置 session
- `/compact` — 精簡 session context (summary)
- `/think <level>` — off|minimal|low|medium|high|xhigh (僅限 GPT-5.2 + Codex 模型)
- `/verbose on|off`
- `/usage off|tokens|full` — 每個回應的用量 footer
- `/restart` — 重啟 gateway (群組中僅限 owner)
- `/activation mention|always` — 群組觸發切換 (僅限群組)

## Apps (可選)

僅 Gateway 就能提供出色的體驗。所有 Apps 都是可選的，並增加額外功能。

如果你打算構建/運行 companion apps，請遵循下方的平台 runbooks。

### macOS (Clawdbot.app) (可選)

- Gateway 和 health 的選單列控制。
- Voice Wake + push-to-talk overlay。
- WebChat + debug tools。
- 通過 SSH 的 Remote gateway control。

注意：需要簽名構建 (signed builds) 以使 macOS 權限在重建後保持有效 (見 `docs/mac/permissions.md`)。

### iOS node (可選)

- 通過 Bridge 配對為 node。
- Voice trigger forwarding + Canvas surface。
- 通過 `clawdbot nodes …` 控制。

Runbook: [iOS connect](https://docs.clawd.bot/platforms/ios)。

### Android node (可選)

- 通過與 iOS 相同的 Bridge + 配對流程進行配對。
- 暴露 Canvas, Camera, 和 Screen capture 指令。
- Runbook: [Android connect](https://docs.clawd.bot/platforms/android)。

## Agent workspace + skills

- Workspace root: `~/clawd` (可通過 `agents.defaults.workspace` 配置)。
- Injected prompt files: `AGENTS.md`, `SOUL.md`, `TOOLS.md`。
- Skills: `~/clawd/skills/<skill>/SKILL.md`。

## Configuration

極簡 `~/.clawdbot/clawdbot.json` (model + 預設值):

```json5
{
  agent: {
    model: "anthropic/claude-opus-4-5",
  },
}
```

[完整配置參考 (所有 keys + 範例).](https://docs.clawd.bot/gateway/configuration)

## Security model (重要)

- **預設:** 工具在 **main** session 的主機上運行，因此當只有你使用時，agent 擁有完全訪問權限。
- **Group/channel 安全:** 設置 `agents.defaults.sandbox.mode: "non-main"` 以在 per‑session Docker sandboxes 中運行 **非 main sessions** (群組/頻道); bash 將在 Docker 中為這些 sessions 運行。
- **Sandbox 預設:** 白名單 `bash`, `process`, `read`, `write`, `edit`, `sessions_list`, `sessions_history`, `sessions_send`, `sessions_spawn`; 黑名單 `browser`, `canvas`, `nodes`, `cron`, `discord`, `gateway`。

詳情：[Security guide](https://docs.clawd.bot/gateway/security) · [Docker + sandboxing](https://docs.clawd.bot/install/docker) · [Sandbox config](https://docs.clawd.bot/gateway/configuration)

### [WhatsApp](https://docs.clawd.bot/channels/whatsapp)

- 連結設備：`pnpm clawdbot channels login` (憑證存儲在 `~/.clawdbot/credentials`)。
- 通過 `channels.whatsapp.allowFrom` 白名單允許誰與助理交談。
- 如果設置了 `channels.whatsapp.groups`，它將成為群組白名單; 包含 `"*"` 以允許所有。

### [Telegram](https://docs.clawd.bot/channels/telegram)

- 設置 `TELEGRAM_BOT_TOKEN` 或 `channels.telegram.botToken` (env 優先)。
- 可選：設置 `channels.telegram.groups` (搭配 `channels.telegram.groups."*".requireMention`); 設定後，它將成為群組白名單 (包含 `"*"` 以允許所有)。根據需要設置 `channels.telegram.allowFrom` 或 `channels.telegram.webhookUrl`。

```json5
{
  channels: {
    telegram: {
      botToken: "123456:ABCDEF",
    },
  },
}
```

### [Slack](https://docs.clawd.bot/channels/slack)

- 設置 `SLACK_BOT_TOKEN` + `SLACK_APP_TOKEN` (或 `channels.slack.botToken` + `channels.slack.appToken`)。

### [Discord](https://docs.clawd.bot/channels/discord)

- 設置 `DISCORD_BOT_TOKEN` 或 `channels.discord.token` (env 優先)。
- 可選：設置 `commands.native`, `commands.text`, 或 `commands.useAccessGroups`, 以及根據需要設置 `channels.discord.dm.allowFrom`, `channels.discord.guilds`, 或 `channels.discord.mediaMaxMb`。

```json5
{
  channels: {
    discord: {
      token: "1234abcd",
    },
  },
}
```

### [Signal](https://docs.clawd.bot/channels/signal)

- 需要 `signal-cli` 和 `channels.signal` 配置區塊。

### [iMessage](https://docs.clawd.bot/channels/imessage)

- 僅限 macOS; Messages 必須登入。
- 如果設置了 `channels.imessage.groups`，它將成為群組白名單; 包含 `"*"` 以允許所有。

### [Microsoft Teams](https://docs.clawd.bot/channels/msteams)

- 配置 Teams app + Bot Framework，然後添加 `msteams` 配置區塊。
- 通過 `msteams.allowFrom` 白名單允許誰交談; 群組訪問通過 `msteams.groupAllowFrom` 或 `msteams.groupPolicy: "open"`。

### [WebChat](https://docs.clawd.bot/web/webchat)

- 使用 Gateway WebSocket; 無單獨的 WebChat 端口/配置。

Browser control (可選):

```json5
{
  browser: {
    enabled: true,
    color: "#FF4500",
  },
}
```

## Docs

當你完成引導流程並想要更深入的參考時，請使用這些文件。

- [從文件索引開始導航和了解“什麼在哪裡”。](https://docs.clawd.bot)
- [閱讀架構概覽以了解 gateway + protocol model。](https://docs.clawd.bot/concepts/architecture)
- [當你需要每個 key 和 example 時，使用完整的配置參考。](https://docs.clawd.bot/gateway/configuration)
- [參照操作手冊 (runbook) 運行 Gateway。](https://docs.clawd.bot/gateway)
- [了解 Control UI/Web 介面如何工作以及如何安全地暴露它們。](https://docs.clawd.bot/web)
- [了解通過 SSH tunnels 或 tailnets 的遠端訪問。](https://docs.clawd.bot/gateway/remote)
- [跟隨引導嚮導流程進行指導安裝。](https://docs.clawd.bot/start/wizard)
- [通過 webhook 介面連接外部觸發器。](https://docs.clawd.bot/automation/webhook)
- [設置 Gmail Pub/Sub 觸發器。](https://docs.clawd.bot/automation/gmail-pubsub)
- [了解 macOS 選單列 companion 的細節。](https://docs.clawd.bot/platforms/mac/menu-bar)
- [平台指南: Windows (WSL2)](https://docs.clawd.bot/platforms/windows), [Linux](https://docs.clawd.bot/platforms/linux), [macOS](https://docs.clawd.bot/platforms/macos), [iOS](https://docs.clawd.bot/platforms/ios), [Android](https://docs.clawd.bot/platforms/android)
- [使用故障排除指南除錯常見失敗。](https://docs.clawd.bot/channels/troubleshooting)
- [在暴露任何內容之前查看安全指南。](https://docs.clawd.bot/gateway/security)

## Advanced docs (discovery + control)

- [Discovery + transports](https://docs.clawd.bot/gateway/discovery)
- [Bonjour/mDNS](https://docs.clawd.bot/gateway/bonjour)
- [Gateway pairing](https://docs.clawd.bot/gateway/pairing)
- [Remote gateway README](https://docs.clawd.bot/gateway/remote-gateway-readme)
- [Control UI](https://docs.clawd.bot/web/control-ui)
- [Dashboard](https://docs.clawd.bot/web/dashboard)

## Operations & troubleshooting

- [Health checks](https://docs.clawd.bot/gateway/health)
- [Gateway lock](https://docs.clawd.bot/gateway/gateway-lock)
- [Background process](https://docs.clawd.bot/gateway/background-process)
- [Browser troubleshooting (Linux)](https://docs.clawd.bot/tools/browser-linux-troubleshooting)
- [Logging](https://docs.clawd.bot/logging)

## Deep dives

- [Agent loop](https://docs.clawd.bot/concepts/agent-loop)
- [Presence](https://docs.clawd.bot/concepts/presence)
- [TypeBox schemas](https://docs.clawd.bot/concepts/typebox)
- [RPC adapters](https://docs.clawd.bot/reference/rpc)
- [Queue](https://docs.clawd.bot/concepts/queue)

## Workspace & skills

- [Skills config](https://docs.clawd.bot/tools/skills-config)
- [Default AGENTS](https://docs.clawd.bot/reference/AGENTS.default)
- [Templates: AGENTS](https://docs.clawd.bot/reference/templates/AGENTS)
- [Templates: BOOTSTRAP](https://docs.clawd.bot/reference/templates/BOOTSTRAP)
- [Templates: IDENTITY](https://docs.clawd.bot/reference/templates/IDENTITY)
- [Templates: SOUL](https://docs.clawd.bot/reference/templates/SOUL)
- [Templates: TOOLS](https://docs.clawd.bot/reference/templates/TOOLS)
- [Templates: USER](https://docs.clawd.bot/reference/templates/USER)

## Platform internals

- [macOS dev setup](https://docs.clawd.bot/platforms/mac/dev-setup)
- [macOS menu bar](https://docs.clawd.bot/platforms/mac/menu-bar)
- [macOS voice wake](https://docs.clawd.bot/platforms/mac/voicewake)
- [iOS node](https://docs.clawd.bot/platforms/ios)
- [Android node](https://docs.clawd.bot/platforms/android)
- [Windows (WSL2)](https://docs.clawd.bot/platforms/windows)
- [Linux app](https://docs.clawd.bot/platforms/linux)

## Email hooks (Gmail)

- [docs.clawd.bot/gmail-pubsub](https://docs.clawd.bot/automation/gmail-pubsub)

## Clawd

Clawdbot 是專為 **Clawd** 打造的，一隻太空龍蝦 AI 助理。 🦞
由 Peter Steinberger 和社群共同製作。

- [clawd.me](https://clawd.me)
- [soul.md](https://soul.md)
- [steipete.me](https://steipete.me)

## Community

請參閱 [CONTRIBUTING.md](CONTRIBUTING.md) 了解指南、維護者以及如何提交 PR。
歡迎 AI/vibe-coded PRs！ 🤖

感謝 [Mario Zechner](https://mariozechner.at/) 的支持以及 [pi-mono](https://github.com/badlogic/pi-mono)。

感謝所有 clawtributors (貢獻者):
(名單保留原文，不進行翻譯)
