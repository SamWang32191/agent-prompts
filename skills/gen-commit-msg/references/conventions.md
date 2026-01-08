# Git Commit Message Conventions

本 Skill 遵循 **Conventional Commits** 規範，以確保 Commit Message 的一致性與可讀性。

## 訊息格式 (Message Format)

每條 Commit Message 應包含 **Header**，並視情況包含 **Body** 與 **Footer**。

```
<type>(<scope>): <subject>

<body>

<footer>
```

### 1. Header (必要)

Header 只有一行，包含三個部分，且不應超過 50 個字元：

*   **Type** (必要)：變更的類型。
*   **Scope** (可選)：變更的範圍（例如：模組名稱、檔案名稱）。
*   **Subject** (必要)：變更的簡短描述。

#### 常用的 Types

| Type | 描述 (Description) |
| :--- | :--- |
| **feat** | 新增功能 (A new feature) |
| **fix** | 修復 Bug (A bug fix) |
| **docs** | 僅修改文件 (Documentation only changes) |
| **style** | 不影響程式碼運作的格式修改 (White-space, formatting, missing semi-colons, etc) |
| **refactor** | 重構，既不是修復 Bug 也不是新增功能 (A code change that neither fixes a bug nor adds a feature) |
| **perf** | 改善效能 (A code change that improves performance) |
| **test** | 新增或修改測試 (Adding missing tests or correcting existing tests) |
| **build** | 影響建置系統或外部依賴的變更 (Changes that affect the build system or external dependencies) |
| **ci** | CI 設定檔或腳本的變更 (Changes to our CI configuration files and scripts) |
| **chore** | 其他不修改 src 或測試檔案的變更 (Other changes that don't modify src or test files) |
| **revert** | 還原先前的 Commit (Reverts a previous commit) |

#### Subject 撰寫原則

*   使用祈使句，現在式 (例如："change" 而不是 "changed" 或 "changes")。
*   第一個字母**不要**大寫。
*   結尾**不要**加句號 (.)。
*   **繁體中文使用者**：請優先使用**繁體中文**撰寫 Subject，除非專案規定使用英文。

### 2. Body (可選)

*   當 Header 無法完整說明變更原因或細節時使用。
*   使用祈使句，現在式。
*   說明 **為什麼 (Why)** 要做這個變更，以及 **與之前有什麼不同 (How)**。

### 3. Footer (可選)

*   用於 **Breaking Changes** 或參考的 **Issues**。
*   Breaking Change 應以 `BREAKING CHANGE:` 開頭，後面接空格或換行。
*   參考 Issue 例如：`Closes #123`。

## 範例 (Examples)

### 簡單功能新增

```
feat: 新增使用者登入 API
```

### 修復 Bug 並關聯 Issue

```
fix(auth): 修正 token 過期時間計算錯誤

Closes #28
```

### 包含 Body 的重構

```
refactor: 重構訂單計算邏輯

將計算邏輯從 Controller 移至 Service 層，以提升可測試性並減少重複程式碼。
```

### Breaking Change

```
feat: 升級至 API v2

BREAKING CHANGE: API v2 變更了回傳格式，所有客戶端需要更新解析邏輯。
```
