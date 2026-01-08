---
name: gen-commit-msg
description: 自動分析 staged changes 並產生符合 Conventional Commits 規範的 git commit message。當使用者想要「git commit」、「產生 commit message」或需要協助撰寫提交訊息時使用此 Skill。
---

# Generate Commit Message (gen-commit-msg)

## 總覽

本 Skill 協助使用者根據 Git 的 staged changes 自動產生符合標準規範的 Commit Message。它能分析程式碼變更的內容與語意，並依照 [Conventional Commits](references/conventions.md) 格式產出清晰、描述性強的訊息。

## 使用流程

當觸發此 Skill 時，請依照以下步驟進行：

### 1. 檢查 Git 狀態

首先，確認目前有哪些檔案已暫存 (staged) 準備提交。

```bash
git status
```

如果沒有檔案被 staged (即 `Changes to be committed` 為空)，請引導使用者先使用 `git add` 加入檔案，或詢問是否要針對 unstaged changes 產生訊息 (需警告使用者這些變更尚未加入)。

### 2. 取得變更內容 (Diff)

讀取 staged 檔案的詳細變更內容，以便理解修改了什麼。

```bash
git diff --cached
```

**注意**：如果 diff 內容過長，請優先讀取關鍵檔案 (如程式碼邏輯)，忽略自動產生的檔案 (如 `package-lock.json` 或 `yarn.lock`，除非變更是關於依賴升級)。

### 3. 分析與產生訊息

根據 `git diff --cached` 的輸出內容，分析變更的意圖：

1.  **判斷 Type**：是新增功能 (`feat`)、修復 (`fix`)、文件 (`docs`) 還是其他？請參考 [references/conventions.md](references/conventions.md) 中的定義。
2.  **判斷 Scope**：變更集中在哪個模組或檔案？
3.  **撰寫 Subject**：用一句簡短的話描述「做了什麼」。
4.  **撰寫 Body (如有需要)**：如果變更複雜，解釋「為什麼」這麼做。

### 4. 呈現與確認

將產生的 Commit Message 呈現給使用者確認。

**格式範例：**

```
建議的 Commit Message:

feat(user): 新增使用者大頭貼上傳功能

- 實作上傳 API endpoint
- 新增圖片格式驗證
```

詢問使用者是否滿意，或是否需要修改。

### 5. 執行提交 (可選)

如果使用者確認滿意，可以使用以下指令協助提交：

```bash
git commit -m "feat(user): 新增使用者大頭貼上傳功能" -m "- 實作上傳 API endpoint" -m "- 新增圖片格式驗證"
```

## 參考資料

*   **Commit 規範**：詳細的 Type 定義與格式範例，請見 [references/conventions.md](references/conventions.md)。