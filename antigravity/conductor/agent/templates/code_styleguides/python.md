# Google Python 風格指南摘要

本文件總結了 Google Python 風格指南中的關鍵規則與最佳實踐。

## 1. Python 語言規則
- **Linting：** 對程式碼執行 `pylint` 以找出 Bug 和風格問題。
- **匯入 (Imports)：** 套件/模組使用 `import x`。僅在 `y` 是子模組時使用 `from x import y`。
- **例外處理 (Exceptions)：** 使用內建的例外類別。不要使用不帶型別的 `except:` 子句。
- **全域狀態：** 避免可變的全域狀態。模組級常量是可以接受的，應採用 `ALL_CAPS_WITH_UNDERSCORES` 格式。
- **推導式 (Comprehensions)：** 僅用於簡單情況。避免用於邏輯複雜、使用完整迴圈更具可讀性的場合。
- **預設參數值：** 不要使用可變物件 (如 `[]` 或 `{}`) 作為預設值。
- **真假值評估：** 使用隱式假值 (例如：`if not my_list:`)。使用 `if foo is None:` 來檢查 `None`。
- **型別註解 (Type Annotations)：** 強烈建議為所有公開 API 提供型別註解。

## 2. Python 風格規則
- **行長度：** 最多 80 個字元。
- **縮排：** 每個縮排層級 4 個空格。絕不使用 tab。
- **空行：** 頂層定義 (類別、函式) 之間留兩個空行。方法定義之間留一個空行。
- **空白：** 避免多餘空白。二元運算子前後使用單個空格。
- **Docstrings：** 使用 `"""三個雙引號"""`。每個公開模組、函式、類別和方法都必須有 docstring。
  - **格式：** 以單行摘要開始。包含 `Args:`、`Returns:` 和 `Raises:` 區塊。
- **字串：** 使用 f-strings 進行格式化。單引號 (`'`) 或雙引號 (`"`) 的使用請保持一致。
- **`TODO` 註解：** 使用 `TODO(username): 修正此處。` 的格式。
- **匯入格式化：** 匯入內容應分行排列並分組：標準函式庫、第三方函式庫，以及你專案本身的匯入。

## 3. 命名
- **一般：** 模組、函式、方法和變數使用 `snake_case` (蛇形命名法)。
- **類別：** `PascalCase` (大駝峰式)。
- **常量：** `ALL_CAPS_WITH_UNDERSCORES` (全大寫加底線)。
- **內部使用：** 內部模組/類別成員使用單個前導底線 (`_internal_variable`)。

## 4. Main
- 所有可執行檔案都應有一個包含主要邏輯的 `main()` 函式，並從 `if __name__ == '__main__':` 區塊中進行呼叫。

**保持一致性。** 修改程式碼時，請配合現有風格。

*來源：[Google Python Style Guide](https://google.github.io/styleguide/pyguide.html)*
