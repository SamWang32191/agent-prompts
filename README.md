<div align="center">

# Agent Prompts Collection 🤖

> 一個收集 AI Agent Prompts 的開源倉庫，專為 **Google Gemini** 和 **Antigravity (Gemini CLI)** 設計。

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](./LICENSE)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)

[開始使用](#-使用方式) • [貢獻](#-貢獻) • [詳細說明](#-目錄結構)

</div>

---

## 探索資源

| 🚀 **Antigravity**                                                                                                                                                                                                           | 💎 **Gemini Gems**                                                                                                              |
| :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------- |
| Google Antigravity (Gemini CLI) 的專用資源。<br><br><ul><li>📜 **[Agent Rules](./antigravity/rules/README.md)**: 各種角色扮演規則</li><li>🎼 **[Conductor](./antigravity/conductor/README.md)**: 上下文驅動開發流程</li></ul> | 可直接在 Google Gemini 中使用的提示詞。<br><br><ul><li>💡 **[Gemini Prompts](./gemini/README.md)**: 增強生產力的 Gems</li></ul> |

| 🤹 **Skills**                                                                                                                                                     | 🤖 **JetBrains AI**                                                                                                                     |
| :--------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------- |
| 符合 **[Agent Skills](https://agentskills.io/)** 標準的技能庫。<br><br><ul><li>🛠️ **[Explore Skills](./skills/README.md)**: 擴充 Agent 能力的標準化技能</li></ul> | 適用於 JetBrains IDE 的 AI Assistant。<br><br><ul><li>💻 **[Editor Prompts](./jetbrains-ai/README.md)**: 編碼輔助與重構提示詞</li></ul> |

---

## 目錄結構

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

## 使用方式

### Antigravity Rules
1. 將所需的規則檔案複製到你的專案 `.agent/rules/` 目錄，Antigravity 將自動載入。
    ```bash
    cp antigravity/rules/planner.md your-project/.agent/rules/
    ```

### Conductor (工作流)
> [!IMPORTANT]
> Conductor 的安裝方式與一般規則不同。它包含多個規則、模板與工作流遺產，必須複製整個 `agent/` 目錄。

1. 將 `antigravity/conductor/agent/` 下的所有內容複製到專案的 `.agent/` 目錄：
    ```bash
    cp -r antigravity/conductor/agent/* your-project/.agent/
    ```

### Gemini Gems
1. 前往 **[Google Gemini](https://gemini.google.com/)** > 建立新 Gem。
2. 複製本倉庫對應的 `.md` 內容貼入 Gem 系統指令中。

---

## 貢獻
歡迎提交 PR 來新增或改進提示詞！

1. Fork 本倉庫
2. 建立分支 (`git checkout -b feature/amazing-prompt`)
3. 提交 (`git commit -m 'Add amazing prompt'`)
4. 推送 (`git push origin feature/amazing-prompt`)
5. 開啟 Pull Request

---

<div align="center">

**[Apache License 2.0](./LICENSE)**
<br>
Copyright © 2026

</div>
