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
├── gemini/             # Google Gemini Gems 提示詞
├── skills/             # Antigravity Skills
└── jetbrains-ai/       # JetBrains AI Assistant 提示詞
```

---

## 🚀 Antigravity

[Antigravity](https://antigravity.google/) 是 Google 推出的 AI 編程工具。本區收錄適用於 Antigravity 的各種規則和工作流程。

### Agent Rules 📜

詳細規則列表請參閱 [Antigravity Rules](./antigravity/rules/README.md)。

### Conductor 🎼

> **兩次測量，一次開發 (Measure twice, code once.)**

Conductor 是一個基於 [gemini-cli-extensions/conductor](https://github.com/gemini-cli-extensions/conductor) Fork 而來的 Antigravity 規則與工作流程合集，實現 **上下文驅動開發 (Context-Driven Development)**。


📖 詳細說明請參閱 [Conductor README](./antigravity/conductor/README.md)

---

## 💎 Gemini Gems

本區收錄可直接在 [Google Gemini](https://gemini.google.com/) 中使用的 Gem 提示詞。

詳細 Gem 列表請參閱 [Gemini Gems](./gemini/README.md)。

---

## 🤹 Skills

本區收錄符合 [Agent Skills](https://agentskills.io/) 標準的 Skills。這些 Skills 是擴充 AI Agent 能力的輕量級開放格式，提供專門的知識與工作流程。

詳細 Skill 列表請參閱 [Skills](./skills/README.md)。

---

## 🤖 JetBrains AI

本區收錄適用於 JetBrains AI Assistant 的提示詞。

詳細提示詞列表請參閱 [JetBrains AI](./jetbrains-ai/README.md)。

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
