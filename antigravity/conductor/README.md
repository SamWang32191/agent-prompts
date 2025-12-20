# Conductor Antigravity Rules Collection

**兩次測量，一次開發 (Measure twice, code once.)**

Conductor 是一個 Antigravity 規則與工作流程合集，旨在實現 **上下文驅動開發 (Context-Driven Development)**。它將 Antigravity 變成一個主動的專案經理，遵循嚴格的協定來規範、計畫並實作軟體功能與 Bug 修復。

Conductor 不僅僅是寫程式碼，它確保每項任務都有一個一致且高品質的生命週期：**上下文 (Context) -> 規格與計畫 (Spec & Plan) -> 實作 (Implement)**。

---

## 核心功能

- **先計畫再開發**：為新舊程式庫建立引導代理的規格與計畫。
- **維護上下文**：確保 AI 遵循風格指南、技術棧選擇與產品目標。
- **安全迭代**：在編寫程式碼前審查計畫，讓開發者保持掌控。
- **團隊協作**：為產品、技術棧與工作流程偏好設定專案層級的上下文，成為團隊共享的基礎。

---

## 如何使用

這些規則與工作流程需與 Antigravity 搭配使用。

### 1. 初始設定
執行以下工作流程以建立專案的基礎架構（產品定義、技術棧、編碼風格）：
```bash
/workflows/conductor-setup
```

### 2. 開啟新任務 (Track)
當準備開始新功能或修復 Bug 時：
```bash
/workflows/conductor-new-track
```
這會產生該任務專屬的 `spec.md` 與 `plan.md`。

### 3. 實作任務
核准計畫後，開始自動實作：
```bash
/workflows/conductor-implement
```
代理將根據計畫逐步執行，並遵循 `conductor/workflow.md` 中定義的規範。

---

## 工作流程一覽

| 工作流程 | 描述 | 產出物 |
| :--- | :--- | :--- |
| `conductor-setup` | 初始化專案環境與基礎文件。 | `conductor/product.md`, `tech-stack.md`, 等 |
| `conductor-new-track` | 開始新的功能或 Bug 修復任務。 | `conductor/tracks/<id>/spec.md`, `plan.md` |
| `conductor-implement` | 執行計畫中定義的各項任務。 | 更新檔案與計畫狀態 |
| `conductor-status` | 顯示專案整體開發進度。 | 狀態總結報告 |
| `conductor-revert` | 復原特定的任務或階段（Git 感知）。 | 復原變更並重置計畫狀態 |

---

## 資源與法律

- **授權協議**：[Apache License 2.0](LICENSE)