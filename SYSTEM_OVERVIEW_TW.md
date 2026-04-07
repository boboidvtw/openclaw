# OpenClaw 系統功能分析與架構說明

**專案名稱**: OpenClaw (Personal AI Assistant)  
**分析日期**: 2026-02-04  
**報告類型**: 架構分析報告

---

## 1. 專案概述 (Executive Summary)

OpenClaw 是一個高度整合的「個人 AI 助理 Gateway」。與一般單純的 Chatbot 不同，OpenClaw 定位為一個 **Operation System for Agents** (Agent 的作業系統)，它運行在使用者自己的設備上 (Local-first)，作為 AI 模型與外部世界（通訊軟體、作業系統、瀏覽器）之間的橋樑。

其核心價值在於 **"Bring AI to where you are"** —— 你不需要打開 ChatGPT 網頁，而是讓 AI 直接滲透進你常用的 WhatsApp, Telegram, Discord 或 Slack 中，並具備操作電腦的能力。

## 2. 核心架構 (Core Architecture)

系統採用 **Monorepo** 結構 (基於 pnpm workspaces)，主要由以下幾個層次組成：

### 2.1 控制平面 (The Gateway)

- **角色**: 整個系統的大腦與神經中樞。
- **技術**: Node.js (>=22), WebSocket。
- **功能**:
  - 維護所有活躍的對話 Session。
  - 路由訊息 (從 Channel 到 Agent，再從 Agent 回傳)。
  - 管理工具執行 (Tool Execution) 與權限控制。
  - 提供 HTTP API 與 WebSocket 介面供 UI 與 Client 連接。

### 2.2 通路層 (Channels Integration)

OpenClaw 實作了驚人的多樣化通訊協定適配：

- **Web-based Clients**: WhatsApp (透過 Baileys 模擬 Web 版), Google Chat.
- **Bot APIs**: Telegram (grammY), Slack (Bolt), Discord (discord.js).
- **Native/Bridge**: iMessage (透過 BlueBubbles 或 macOS 原生 `imsg`), Signal.
- **Extensions**: Microsoft Teams, Matrix, Zalo.

### 2.3 代理執行層 (Agent Runtime)

- 支援 **RPC (Remote Procedure Call)** 模式，讓 Agent 可以遠端調用本地工具。
- 支援多種 Agent 隔離策略 (Group Isolation)，可針對不同群組或對話開啟獨立的 Context。
- 深度整合 **Anthropic Claude** (推薦) 與 **OpenAI** 模型。
- **Sandboxing**: 支援 Docker 沙盒環境，確保 Agent 執行敏感指令 (如 Bash) 時的安全性。

### 2.4 客戶端與 UI (Clients & UI)

- **WebChat**: 內建於 Gateway 的網頁版聊天介面。
- **macOS App**: 位於 Menu bar 的原生應用，支援語音喚醒與螢幕與 Canvas 互動。
- **Mobile Nodes**: iOS 與 Android 的配套 App，可作為「感測器節點」提供相機、位置、螢幕錄製等功能給中央 Agent 使用。

## 3. 關鍵功能模組 (Key Modules)

### 🔊 Voice & Talk Mode (語音與通話)

- 實作了 **Voice Wake** (語音喚醒) 與 **Continuous Conversation** (連續對話)。
- 結合了 TTS (Text-to-Speech) 與 STT (Speech-to-Text)，讓 AI 能像講電話一樣與使用者溝通。

### 🎨 Live Canvas (即時畫布)

- 一個由 Agent 驅動的視覺化工作區 (基於 A2UI 協議)。
- Agent 可以在此畫布上繪製 UI、顯示數據圖表或建立互動式元件，而不僅僅是輸出文字。

### 🌐 Browser Control (瀏覽器控制)

- 內建受控的 Chrome/Chromium 瀏覽器實例。
- Agent 可以「看見」網頁 (Snapshots)、操作網頁元素、讀取內容，實現網頁自動化任務。

### 🛡️ Security & Permissions (安全機制)

- **Human-in-the-loop**: 對於敏感操作 (如 Shell 指令)，預設需要使用者批准。
- **DM Pairing**: 對於未知的私訊 (Direct Message)，採用配對碼 (Pairing Code) 機制，防止未經授權的存取。
- **Role-Based Access**: 區分 Main Session (擁有者，全權限) 與 Non-main Sessions (群組/訪客，受限權限)。

## 4. 技術堆疊 (Tech Stack)

- **Runtime**: Node.js v22+
- **Language**: TypeScript (全專案)
- **Package Manager**: pnpm (Workspaces)
- **Web Framework**: Hono (輕量級 Web 框架), Express (部分 legacy 模組)
- **Frontend**: Lit (Web Components), TailwindCSS (推測)
- **Testing**: Vitest
- **Build Tools**: tsup/tsdown, oxlint (高效能 Linter)
- **Mobile**: Swift (iOS), Kotlin/Java (Android)

## 5. 目錄結構導覽

- `apps/`: 包含各個平台的應用程式原始碼 (ios, macos, android)。
- `packages/`: 共享的函式庫與核心邏輯模組 (如 clawdbot, moltbot)。
- `src/`: Gateway 與核心 Agent 邏輯的主要原始碼。
- `ui/`: Web 前端介面 (Control UI, WebChat)。
- `skills/`: 擴充技能的存放目錄 (如搜尋、日曆 integration 等)。
- `docs/`: 完整的官方文檔。
- `scripts/`: 開發與維運自動化腳本。

## 6. 使用者建議 (Architect's Note)

OpenClaw 是一個架構非常成熟且野心勃勃的專案。

1.  **初次上手**: 建議使用 `openclaw onboard` 精靈指令，這會引導完成最複雜的 Config 設定。
2.  **安全性**: 若要將 Gateway 暴露於公網 (Public Internet)，強烈建議搭配 **Tailscale** 使用，而非直接透過 Port Forwarding，以確保連線安全。
3.  **模型選擇**: 由於其設計依賴長 Context 與複雜指令遵循，強烈建議搭配 **Claude 3.5 Sonnet / Opus** 等級的模型以獲得最佳體驗。
