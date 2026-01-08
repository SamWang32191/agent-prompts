# Skills 🤹

本區收錄符合 [Agent Skills](https://agentskills.io/) 標準的 Skills。這些 Skills 是擴充 AI Agent 能力的輕量級開放格式，提供專門的知識與工作流程。

| Skill                                             | 說明                                                                   |
| ------------------------------------------------- | ---------------------------------------------------------------------- |
| [gen-commit-msg](./gen-commit-msg/SKILL.md)       | 自動分析 staged changes 並產生符合 Conventional Commits 規範的提交訊息 |
| [skill-creator](./skill-creator/SKILL.md)         | 建立新 Antigravity Skill 的 meta-skill 工具                            |
| [slack-gif-creator](./slack-gif-creator/SKILL.md) | 製作針對 Slack 最佳化的動態 GIF                                        |


### 使用方式
將skills 目錄複製到工具對應的skills目錄。

#### Claude Code
- 個人（跨專案）：`~/.claude/skills/`
- 專案（repo 內共享）：`./.claude/skills/`

#### Codex CLI（OpenAI）
- 個人（跨專案）：`~/.codex/skills/`
- 專案（repo 內共享）：`./.codex/skills/`

#### GitHub Copilot（VS Code 與 copilot-cli）
VS Code 文件把「Agent Skills」定位成可跨 GitHub Copilot in VS Code、GitHub Copilot CLI 等共用的 open standard，並明確說 VS Code 會讀兩個位置：
- 推薦（新技能共享）：`./.github/skills/` 
- 相容（legacy）：`./.claude/skills/` 

#### Gemini CLI
Gemini CLI 的 skills 探索有優先順序 專案 > 個人： 
- 專案（repo 內共享）：`./.gemini/skills/` 
- 個人（跨專案）：`~/.gemini/skills/` 