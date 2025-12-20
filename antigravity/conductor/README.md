# Conductor Antigravity Rules Collection

**兩次測量，一次開發 (Measure twice, code once.)**

Conductor 是一個 Antigravity 規則與工作流程合集，旨在實現 **上下文驅動開發 (Context-Driven Development)**。它將 Antigravity 變成一個主動的專案經理，遵循嚴格的協定來規範、計畫並實作軟體功能與 Bug 修復。

Conductor 不僅僅是寫程式碼，它確保每項任務都有一個一致且高品質的生命週期：**上下文 (Context) -> 規格與計畫 (Spec & Plan) -> 實作 (Implement)**。

## Fork from [Conductor](https://github.com/gemini-cli-extensions/conductor)

---

## 核心功能

- **先計畫再開發**：為新舊程式庫建立引導代理的規格與計畫。
- **維護上下文**：確保 AI 遵循風格指南、技術棧選擇與產品目標。
- **安全迭代**：在編寫程式碼前審查計畫，讓開發者保持掌控。
- **團隊協作**：為產品、技術棧與工作流程偏好設定專案層級的上下文，成為團隊共享的基礎。

---

## 使用方式

Conductor 旨在管理您開發任務的整個生命週期。

### 1. 設定專案 (執行一次)

當您執行 `/conductor-setup` 時，Conductor 會協助您定義專案上下文的核心元件。此上下文隨後將用於您或您團隊中的任何人建立新元件或功能。

- **產品 (Product)：** 定義專案上下文（例如：使用者、產品目標、高階功能）。
- **產品準則 (Product Guidelines)：** 定義標準（例如：文風、品牌訊息、視覺識別）。
- **技術堆疊 (Tech Stack)：** 配置技術偏好（例如：語言、資料庫、框架）。
- **工作流 (Workflow)：** 設定團隊偏好（例如：TDD、提交策略）。使用 [workflow.md](templates/workflow.md) 作為可自定義的模板。

**生成的產出物：**
- `conductor/product.md`
- `conductor/product-guidelines.md`
- `conductor/tech-stack.md`
- `conductor/workflow.md`
- `conductor/code_styleguides/`
- `conductor/tracks.md`

```bash
/conductor-setup
```

### 2. 開始一個新 Track (功能或 Bug)

當您準備好處理新功能或錯誤修復時，請執行 `/conductor-new-track`。這會初始化一個 **Track** — 一個高階的工作單元。Conductor 協助您生成兩個關鍵產出物：

- **規格 (Specs)：** 特定工作的詳細需求。我們要建立什麼以及為什麼？
- **計畫 (Plan)：** 包含階段、任務和子任務的可執行待辦清單。

**生成的產出物：**
- `conductor/tracks/<track_id>/spec.md`
- `conductor/tracks/<track_id>/plan.md`
- `conductor/tracks/<track_id>/metadata.json`

```bash
/conductor-new-track
# 或者帶有描述
/conductor-new-track "Add a dark mode toggle to the settings page"
```

### 3. 實作 Track

一旦您核准了計畫，請執行 `/conductor-implement`。您的寫碼代理隨後會執行 `plan.md` 檔案，在完成任務時將其勾選。

**更新的產出物：**
- `conductor/tracks.md` (狀態更新)
- `conductor/tracks/<track_id>/plan.md` (狀態更新)
- 專案上下文檔案 (完成時同步)

```bash
/conductor-implement
```

Conductor 將會：
1.  選擇下一個待辦任務。
2.  遵循定義的工作流（例如 TDD：撰寫測試 -> 失敗 -> 實作 -> 通過）。
3.  隨著進展更新計畫中的狀態。
4.  **驗證進度：** 在每個階段結束時引導您進行手動驗證步驟，以確保一切按預期運作。

在實作過程中，您還可以：

- **檢查狀態：** 取得專案進度的高階概覽。
  ```bash
  /conductor-status
  ```
- **還原工作：** 如果需要，復原功能或特定任務。
  ```bash
  /conductor-revert
  ```

## 命令參考

| 命令 | 描述 | 產出物 |
| :--- | :--- | :--- |
| `/conductor-setup` | 建構專案骨架並設定 Conductor 環境。每個專案執行一次。 | `conductor/product.md`<br>`conductor/tech-stack.md`<br>`conductor/workflow.md`<br>`conductor/tracks.md` |
| `/conductor-new-track` | 啟動新功能或錯誤修復 Track。生成 `spec.md` 和 `plan.md`。 | `conductor/tracks/<id>/spec.md`<br>`conductor/tracks/<id>/plan.md`<br>`conductor/tracks.md` |
| `/conductor-implement` | 執行目前 Track 計畫中定義的任務。 | `conductor/tracks.md`<br>`conductor/tracks/<id>/plan.md` |
| `/conductor-status` | 顯示 tracks 檔案和進行中 Track 的目前進度。 | 讀取 `conductor/tracks.md` |
| `/conductor-revert` | 透過分析 git 歷史記錄來還原 track、phase 或 task。 | 還原 git 歷史記錄 |