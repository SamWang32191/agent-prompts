---
name: skill-creator
description: 建立有效 Skill 的指南。當使用者想要建立新的 Skill（或更新現有的 Skill），以透過專門的知識、工作流程或工具整合來擴充 Claude 的功能時，應使用此 Skill。
license: 完整條款請見 LICENSE.txt
---

# Skill 建立器

本 Skill 提供建立有效 Skill 的指南。

## 關於 Skills

Skills 是模組化、獨立的套件，透過提供專門的知識、工作流程和工具來擴充 Claude 的功能。可以將它們視為特定領域或任務的「入門指南」——它們將 Claude 從通用代理人轉變為具備特定程序性知識的專門代理人，這是任何模型都無法完全具備的。

### Skills 提供什麼

1. 專門的工作流程 - 針對特定領域的多步驟程序
2. 工具整合 - 針對特定檔案格式或 API 的操作說明
3. 領域專業知識 - 公司特定的知識、架構、商業邏輯
4. 隨附資源 - 用於複雜和重複任務的腳本、參考資料和資產

## 核心原則

### 簡潔為要

Context Window（上下文視窗）是公共財。Skills 與 Claude 需要的其他所有內容共享 Context Window：系統提示詞、對話歷史記錄、其他 Skill 的 metadata，以及使用者的實際請求。

**預設假設：Claude 已經非常聰明。** 僅添加 Claude 尚未具備的上下文。質疑每一條資訊：「Claude 真的需要這個解釋嗎？」以及「這段文字值得它的 Token 成本嗎？」

偏好簡潔的範例，而非冗長的解釋。

### 設定適當的自由度

根據任務的脆弱性和變異性來調整具體程度：

**高自由度（基於文字的指令）**：當有多種方法可行、決策取決於上下文，或依靠啟發式方法引導時使用。

**中自由度（虛擬碼或帶參數的腳本）**：當存在偏好的模式、允許某些變異，或設定會影響行為時使用。

**低自由度（特定腳本，極少參數）**：當操作脆弱且容易出錯、一致性至關重要，或必須遵循特定順序時使用。

把 Claude 想像成在探索路徑：狹窄的懸崖橋樑需要特定的護欄（低自由度），而開闊的原野則允許許多路線（高自由度）。

### Skill 的剖析

每個 Skill 包含一個必要的 SKILL.md 檔案和可選的隨附資源：

```
skill-name/
├── SKILL.md (必要)
│   ├── YAML frontmatter metadata (必要)
│   │   ├── name: (必要)
│   │   └── description: (必要)
│   └── Markdown instructions (必要)
└── Bundled Resources (可選)
    ├── scripts/          - 可執行程式碼 (Python/Bash/等)
    ├── references/       - 旨在依需求載入上下文的文件
    └── assets/           - 用於輸出的檔案 (模板、圖示、字型等)
```

#### SKILL.md (必要)

每個 SKILL.md 包含：

- **Frontmatter** (YAML)：包含 `name` 和 `description` 欄位。這是 Claude 讀取以決定何時使用該 Skill 的唯一欄位，因此清晰且全面地描述該 Skill 是什麼以及何時使用非常重要。
- **Body** (Markdown)：使用該 Skill 的指令和指南。僅在 Skill 被觸發後載入（如果有觸發）。

#### 隨附資源 (可選)

##### Scripts (`scripts/`)

用於需要確定性可靠度或重複編寫的任務的可執行程式碼（Python/Bash/等）。

- **何時包含**：當相同的程式碼被重複編寫或需要確定性可靠度時
- **範例**：用於 PDF 旋轉任務的 `scripts/rotate_pdf.py`
- **優點**：Token 使用效率高、具確定性，可在不載入上下文的情況下執行
- **注意**：腳本可能仍需要被 Claude 讀取以進行修補或環境特定的調整

##### References (`references/`)

旨在依需求載入上下文以告知 Claude 流程和思維的文件和參考資料。

- **何時包含**：當有 Claude 在工作時應參考的文件
- **範例**：用於財務架構的 `references/finance.md`、用於公司保密協議模板的 `references/mnda.md`、用於公司政策的 `references/policies.md`、用於 API 規格的 `references/api_docs.md`
- **使用案例**：資料庫架構、API 文件、領域知識、公司政策、詳細工作流程指南
- **優點**：保持 SKILL.md 精簡，僅在 Claude 決定需要時才載入
- **最佳實務**：如果檔案很大（>10k 字），請在 SKILL.md 中包含 grep 搜尋模式
- **避免重複**：資訊應存在於 SKILL.md 或 references 檔案中，不可兩者皆有。除非資訊對 Skill 確實核心，否則優先使用 references 檔案存放詳細資訊——這能保持 SKILL.md 精簡，同時讓資訊可被發現且不佔用 Context Window。僅在 SKILL.md 中保留必要的程序性指令和工作流程指南；將詳細的參考資料、架構和範例移至 references 檔案。

##### Assets (`assets/`)

不打算載入上下文，而是用於 Claude 產生的輸出中的檔案。

- **何時包含**：當 Skill 需要在最終輸出中使用的檔案時
- **範例**：用於品牌資產的 `assets/logo.png`、用於 PowerPoint 模板的 `assets/slides.pptx`、用於 HTML/React 樣板的 `assets/frontend-template/`、用於排版的 `assets/font.ttf`
- **使用案例**：模板、圖像、圖示、樣板程式碼、字型、被複製或修改的範例文件
- **優點**：將輸出資源與文件分離，使 Claude 無需將檔案載入上下文即可使用

#### 不應包含在 Skill 中的內容

一個 Skill 應僅包含直接支援其功能的基本檔案。**請勿**建立無關的文件或輔助檔案，包括：

- README.md
- INSTALLATION_GUIDE.md
- QUICK_REFERENCE.md
- CHANGELOG.md
- etc.

Skill 應僅包含 AI 代理人完成手頭工作所需的資訊。它不應包含關於建立過程、設定和測試程序、使用者面向的文件等輔助上下文。建立額外的文件檔案只會增加混亂和困惑。

### 漸進式揭露設計原則

Skills 使用三級載入系統來有效地管理上下文：

1. **Metadata (名稱 + 描述)** - 始終在上下文中 (~100 字)
2. **SKILL.md 本文** - 當 Skill 觸發時 (<5k 字)
3. **隨附資源** - 依 Claude 需求 (無限制，因為腳本可以在不讀入 Context Window 的情況下執行)

#### 漸進式揭露模式

將 SKILL.md 本文保持在基本內容且低於 500 行，以最小化上下文膨脹。接近此限制時將內容拆分到單獨的檔案中。當將內容拆分到其他檔案時，從 SKILL.md 引用它們並清楚描述何時讀取它們非常重要，以確保 Skill 的讀者知道它們的存在以及何時使用它們。

**關鍵原則：** 當一個 Skill 支援多種變體、框架或選項時，僅在 SKILL.md 中保留核心工作流程和選擇指南。將變體原本的詳細資訊（模式、範例、設定）移至單獨的參考檔案中。

**模式 1：高層次指南搭配參考資料**

```markdown
# PDF 處理

## 快速開始

使用 pdfplumber 提取文字：
[程式碼範例]

## 進階功能

- **表單填寫**：完整指南請見 [FORMS.md](FORMS.md)
- **API 參考**：所有方法請見 [REFERENCE.md](REFERENCE.md)
- **範例**：常見模式請見 [EXAMPLES.md](EXAMPLES.md)
```

Claude 僅在需要時才載入 FORMS.md、REFERENCE.md 或 EXAMPLES.md。

**模式 2：特定領域的組織**

對於具有多個領域的 Skills，按領域組織內容以避免載入不相關的上下文：

```
bigquery-skill/
├── SKILL.md (概覽和導航)
└── reference/
    ├── finance.md (營收、帳單指標)
    ├── sales.md (商機、銷售漏斗)
    ├── product.md (API 使用、功能)
    └── marketing.md (活動、歸因)
```

當使用者詢問銷售指標時，Claude 僅讀取 sales.md。

同樣地，對於支援多種框架或變體的 skills，按變體組織：

```
cloud-deploy/
├── SKILL.md (工作流程 + 供應商選擇)
└── references/
    ├── aws.md (AWS 部署模式)
    ├── gcp.md (GCP 部署模式)
    └── azure.md (Azure 部署模式)
```

當使用者選擇 AWS 時，Claude 僅讀取 aws.md。

**模式 3：條件式細節**

顯示基本內容，連結至進階內容：

```markdown
# DOCX 處理

## 建立文件

使用 docx-js 建立新文件。請見 [DOCX-JS.md](DOCX-JS.md)。

## 編輯文件

對於簡單編輯，直接修改 XML。

**關於修訂追蹤**：請見 [REDLINING.md](REDLINING.md)
**關於 OOXML 細節**：請見 [OOXML.md](OOXML.md)
```

Claude 僅在使用者需要這些功能時才讀取 REDLINING.md 或 OOXML.md。

**重要準則：**

- **避免深層巢狀參考** - 保持參考距離 SKILL.md 僅一層。所有參考檔案應直接從 SKILL.md 連結。
- **結構化較長的參考檔案** - 對於超過 100 行的檔案，在頂部包含目錄，以便 Claude 在預覽時能看到完整範圍。

## Skill 建立流程

Skill 建立包含以下步驟：

1. 用具體範例理解 Skill
2. 規劃可重複使用的 Skill 內容（腳本、參考資料、資產）
3. 初始化 Skill（執行 init_skill.py）
4. 編輯 Skill（實作資源並撰寫 SKILL.md）
5. 打包 Skill（執行 package_skill.py）
6. 根據實際使用情況進行迭代

請依序遵循這些步驟，除非有明確理由不適用才跳過。

### 步驟 1：用具體範例理解 Skill

僅在已經清楚理解 Skill 的使用模式時才跳過此步驟。即使是處理現有的 Skill，這一步驟仍然很有價值。

為了建立有效的 Skill，請清楚理解該 Skill 將如何被使用的具體範例。這種理解可以來自直接的使用者範例，或是經使用者回饋驗證過的生成範例。

例如，在建立 `image-editor` skill 時，相關問題包括：

- 「`image-editor` skill 應該支援什麼功能？編輯、旋轉，還有其他的嗎？」
- 「你能舉一些這個 skill 會被如何使用的範例嗎？」
- 「我可以想像使用者會要求像是『移除這張圖片的紅眼』或『旋轉這張圖片』。你還能想像這個 skill 有其他使用方式嗎？」
- 「使用者說什麼應該觸發這個 skill？」

為避免讓使用者感到不知所措，避免在單一訊息中詢問太多問題。從最重要的問題開始，並依需要後續追問以獲得更好的效果。

當對 Skill 應支援的功能有清晰的認知時，結束此步驟。

### 步驟 2：規劃可重複使用的 Skill 內容

為了將具體範例轉化為有效的 Skill，透過以下方式分析每個範例：

1. 考慮如何從頭開始執行該範例
2. 識別在重複執行這些工作流程時，哪些腳本、參考資料和資產會有所幫助

範例：當建立 `pdf-editor` skill 以處理如「幫我旋轉這個 PDF」的查詢時，分析顯示：

1. 旋轉 PDF 每次都需要重寫相同的程式碼
2. 將 `scripts/rotate_pdf.py` 腳本儲存在 skill 中會有所幫助

範例：當設計 `frontend-webapp-builder` skill 以處理如「幫我建立一個代辦事項 App」或「幫我建立一個儀表板來追蹤我的步數」的查詢時，分析顯示：

1. 撰寫前端 webapp 每次都需要相同的樣板 HTML/React
2. 將包含樣板 HTML/React 專案檔案的 `assets/hello-world/` 模板儲存在 skill 中會有所幫助

範例：當建立 `big-query` skill 以處理如「今天有多少使用者登入？」的查詢時，分析顯示：

1. 查詢 BigQuery 每次都需要重新探索資料表架構和關係
2. 將記錄資料表架構的 `references/schema.md` 檔案儲存在 skill 中會有所幫助

為了確立 Skill 的內容，分析每個具體範例以建立要包含的可重複使用資源清單：腳本、參考資料和資產。

### 步驟 3：初始化 Skill

至此，是時候實際建立 Skill 了。

僅在正在開發的 Skill 已經存在，且需要迭代或打包時才跳過此步驟。在這種情況下，請繼續進行下一步。

當從頭開始建立新的 Skill 時，務必執行 `init_skill.py` 腳本。該腳本方便地產生一個新的模板 Skill 目錄，自動包含 Skill 所需的一切，使 Skill 建立過程更有效率且可靠。

用法：

```bash
scripts/init_skill.py <skill-name> --path <output-directory>
```

該腳本會：

- 在指定路徑建立 Skill 目錄
- 產生帶有適當 frontmatter 和 TODO 佔位符的 SKILL.md 模版
- 建立範例資源目錄：`scripts/`、`references/` 和 `assets/`
- 在每個目錄中加入可自訂或刪除的範例檔案

初始化後，依需要自訂或移除產生的 SKILL.md 和範例檔案。

### 步驟 4：編輯 Skill

當編輯（新產生的或現有的）Skill 時，請記住該 Skill 是為了讓另一個 Claude 實例使用而建立的。包含對 Claude 有益且非顯而易見的資訊。考慮哪些程序性知識、領域特定細節或可重複使用的資產能幫助另一個 Claude 實例更有效地執行這些任務。

#### 學習經過驗證的設計模式

根據你的 Skill 需求參考這些有用的指南：

- **多步驟流程**：順序工作流程和條件邏輯請見 references/workflows.md
- **特定輸出格式或品質標準**：模板和範例模式請見 references/output-patterns.md

這些檔案包含有效 Skill 設計的既定最佳實務。

#### 從可重複使用的 Skill 內容開始

要開始實作，請從上面識別出的可重複使用資源開始：`scripts/`、`references/` 和 `assets/` 檔案。注意此步驟可能需要使用者輸入。例如，在實作 `brand-guidelines` skill 時，使用者可能需要提供品牌資產或模板以儲存在 `assets/` 中，或提供文件以儲存在 `references/` 中。

新增的腳本必須透過實際執行來測試，以確保沒有錯誤且輸出符合預期。如果有許多相似的腳本，只需測試具有代表性的樣本，以在確保它們都能運作的信心與完成時間之間取得平衡。

應刪除任何 Skill 不需要範例檔案和目錄。初始化腳本在 `scripts/`、`references/` 和 `assets/` 中建立範例檔案以展示結構，但大多數 Skill 不會需要全部。

#### 更新 SKILL.md

**撰寫準則：** 始終使用祈使句/不定詞形式。

##### Frontmatter

撰寫帶有 `name` 和 `description` 的 YAML frontmatter：

- `name`：Skill 名稱
- `description`：這是 Skill 的主要觸發機制，幫助 Claude 了解何時使用該 Skill。
  - 包含 Skill 做什麼以及何時使用它的具體觸發條件/上下文。
  - 在此包含所有「何時使用」的資訊 - 不要放在本文中。本文僅在觸發後載入，因此本文中的「何時使用此 Skill」章節對 Claude 沒有幫助。
  - `docx` skill 的範例描述：「包含追蹤修訂、註解、格式保留和文字提取支援的全面文件建立、編輯和分析。當 Claude 需要處理專業文件（.docx 檔案）以進行以下操作時使用：(1) 建立新文件，(2) 修改或編輯內容，(3) 處理追蹤修訂，(4) 新增註解，或任何其他文件任務」

請勿在 YAML frontmatter 中包含任何其他欄位。

##### Body

撰寫使用該 Skill 及其隨附資源的指令。

### 步驟 5：打包 Skill

一旦 Skill 開發完成，必須將其打包成可分發的 .skill 檔案以與使用者分享。打包過程會先自動驗證 Skill 以確保符合所有要求：

```bash
scripts/package_skill.py <path/to/skill-folder>
```

可選的輸出目錄指定：

```bash
scripts/package_skill.py <path/to/skill-folder> ./dist
```

打包腳本將會：

1. 自動 **驗證** Skill，檢查：

   - YAML frontmatter 格式和必要欄位
   - Skill 命名慣例和目錄結構
   - 描述的完整性和品質
   - 檔案組織和資源參考

2. 如果驗證通過，則 **打包** Skill，建立一個以 Skill 命名的 .skill 檔案（例如 `my-skill.skill`），其中包含所有檔案並維持正確的分發目錄結構。.skill 檔案是一個副檔名為 .skill 的 zip 檔案。

如果驗證失敗，腳本將報告錯誤並退出而不建立套件。修復任何驗證錯誤並再次執行打包指令。

### 步驟 6：迭代

測試 Skill 後，使用者可能會要求改進。這通常發生在使用 Skill 之後，對 Skill 的表現有新鮮的上下文印象。

**迭代工作流程：**

1. 在實際任務中使用 Skill
2. 注意掙扎或效率低下的地方
3. 識別應如何更新 SKILL.md 或隨附資源
4. 實作變更並再次測試
