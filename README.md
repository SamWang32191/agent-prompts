# Agent Prompts Collection 🤖

> 一個收集 AI Agent Prompts 的開源倉庫，專為 **Google Gemini** 和 **Antigravity (Gemini CLI)** 設計。

本倉庫旨在分享和整理高品質的 AI 提示詞模板，包含各種專用 Agent 規則、工作流程，以及可直接在 Gemini 中使用的 Gems。

---

## 📁 目錄結構

```
agent-prompts/
├── antigravity/        # Antigravity (Gemini CLI) 專用資源
│   ├── conductor/      # Conductor 規則與工作流程合集
│   └── rules/          # 各種 Agent 規則
└── gemini/             # Google Gemini Gems 提示詞
```

---

## 🚀 Antigravity

[Antigravity](https://antigravity.google/) 是 Google 推出的 AI 編程工具。本區收錄適用於 Antigravity 的各種規則和工作流程。

### Conductor 🎼

> **兩次測量，一次開發 (Measure twice, code once.)**

Conductor 是一個基於 [gemini-cli-extensions/conductor](https://github.com/gemini-cli-extensions/conductor) Fork 而來的 Antigravity 規則與工作流程合集，實現 **上下文驅動開發 (Context-Driven Development)**。

**核心功能：**
- 🎯 **先計畫再開發** — 為新舊程式庫建立引導代理的規格與計畫
- 📋 **維護上下文** — 確保 AI 遵循風格指南、技術棧選擇與產品目標
- 🔒 **安全迭代** — 在編寫程式碼前審查計畫，讓開發者保持掌控
- 👥 **團隊協作** — 為產品、技術棧與工作流程偏好設定專案層級的上下文

**可用指令：**

| 指令 | 說明 |
|------|------|
| `/conductor-setup` | 建構專案骨架並設定 Conductor 環境 |
| `/conductor-setup-track` | 配置程式碼風格、工作流並生成初始 Track |
| `/conductor-new-track` | 啟動新功能或錯誤修復 Track |
| `/conductor-implement` | 執行目前 Track 計畫中定義的任務 |
| `/conductor-status` | 顯示目前進度 |
| `/conductor-revert` | 還原 track、phase 或 task |

📖 詳細說明請參閱 [Conductor README](./antigravity/conductor/README.md)

---

### Agent 規則 📜

| 檔案 | 說明 |
|------|------|
| [planner.md](./antigravity/rules/planner.md) | 實作計畫生成模式，產出可由 AI 或人類執行的結構化計畫 |
| [prompt-engineer.agent.md](./antigravity/rules/prompt-engineer.agent.md) | 提示詞工程師模式，分析並改進輸入的提示詞 |
| [prompt-builder.agent.md](./antigravity/rules/prompt-builder.agent.md) | 提示詞建構器 Agent |
| [cli-agent.md](./antigravity/rules/cli-agent.md) | CLI Agent 規則 |
| [context7.md](./antigravity/rules/context7.md) | Context7 MCP Server 規則 |
| [java.md](./antigravity/rules/java.md) | Java 開發專用規則 |
| [typescript-mcp-server-expert.md](./antigravity/rules/typescript-mcp-server-expert.md) | TypeScript MCP Server 專家規則 |
| [zh-tw.md](./antigravity/rules/zh-tw.md) | 繁體中文輸出規則 |

---

## 💎 Gemini Gems

本區收錄可直接在 [Google Gemini](https://gemini.google.com/) 中使用的 Gem 提示詞。

| Gem | 說明 |
|-----|------|
| [nano-banana-gem.md](./gemini/nano-banana-gem.md) | NBP 一鍵圖像生成器 — 輸入內容後選擇風格，即可生成專業簡報圖表 |
| [nano-banana-ppt-prompt.md](./gemini/nano-banana-ppt-prompt.md) | NBP 簡報提示詞 |
| [git-version-control-expert.md](./gemini/git-version-control-expert.md) | Git 版本控制專家 — 協助解決各種 Git 問題 |
| [海龜湯gem.md](./gemini/海龜湯gem.md) | 海龜湯情境推理遊戲 Gem |

---

## 🛠️ 使用方式

### Antigravity 規則

1. 將所需的規則檔案複製到你的專案 `.agent/rules/` 目錄
2. Antigravity 會自動載入這些規則

```bash
# 範例：複製 planner 規則
cp antigravity/rules/planner.md your-project/.agent/rules/
```

### Gemini Gems

1. 開啟 [Google Gemini](https://gemini.google.com/)
2. 建立新的 Gem
3. 將 `.md` 檔案中的內容貼入 Gem 的系統指令中
4. 儲存並開始使用

---

## 🤝 貢獻

歡迎提交 PR 來新增或改進提示詞！

1. Fork 本倉庫
2. 建立您的功能分支 (`git checkout -b feature/amazing-prompt`)
3. 提交您的修改 (`git commit -m 'Add some amazing prompt'`)
4. 推送到分支 (`git push origin feature/amazing-prompt`)
5. 開啟一個 Pull Request

---

## 📄 授權

本專案採用 Apache License 2.0 — 詳見 [LICENSE](./LICENSE) 檔案。
