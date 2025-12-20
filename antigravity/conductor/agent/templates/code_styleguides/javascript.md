# Google JavaScript 風格指南摘要

本文件總結了 Google JavaScript 風格指南中的關鍵規則與最佳實踐。

## 1. 原始檔案基礎
- **檔案命名：** 一律小寫，使用底線 (`_`) 或連字號 (`-`)。副檔名必須為 `.js`。
- **檔案編碼：** UTF-8。
- **空白字元：** 僅使用 ASCII 水平空格 (0x20)。禁止使用 tab 進行縮排。

## 2. 原始檔案結構
- 新檔案應為 ES 模組 (`import`/`export`)。
- **匯出 (Exports)：** 使用具名匯出 (`export {MyClass};`)。**不使用預設匯出 (default exports)。**
- **匯入 (Imports)：** 不要對匯入語句進行換行。匯入路徑中的 `.js` 副檔名是強制性的。

## 3. 格式化
- **大括號：** 所有控制結構 (`if`、`for`、`while` 等) 皆必須使用大括號，即使是單行區塊。使用 K&R 風格 (「埃及括號」)。
- **縮排：** 每個新區塊縮排 2 個空格。
- **分號：** 每個陳述式都必須以分號結尾。
- **資料行限制：** 80 個字元。
- **換行：** 接續行至少縮排 4 個空格。
- **空白：** 方法之間使用單個空行。不留結尾空白。

## 4. 語言特性
- **變數宣告：** 預設使用 `const`，若需要重新賦值則使用 `let`。**禁止使用 `var`。**
- **陣列實字 (Array Literals)：** 使用尾隨逗號。不要使用 `Array` 建構函式。
- **物件實字 (Object Literals)：** 使用尾隨逗號與簡寫屬性。不要使用 `Object` 建構函式。
- **類別 (Classes)：** 不要使用 JavaScript 的 getter/setter 屬性 (`get name()`)。請改用普通方法。
- **函式 (Functions)：** 巢狀函式偏好使用箭頭函式，以保留 `this` 上下文。
- **字串實字 (String Literals)：** 使用單引號 (`'`)。多行字串或複雜插值請使用樣板字面值 (`` ` ``)。
- **控制結構：** 偏好 `for-of` 迴圈。`for-in` 迴圈僅應在字典風格的物件上使用。
- **`this`：** 僅在類別建構函式、方法或其中定義的箭頭函式中使用 `this`。
- **相等檢查：** 始終使用全等運算子 (`===` / `!==`)。

## 5. 禁用特性
- `with` 關鍵字。
- `eval()` 或 `Function(...string)`。
- 自動分號插入 (ASI)。
- 修改內建物件 (例如：`Array.prototype.foo = ...`)。

## 6. 命名
- **類別：** `UpperCamelCase` (大駝峰式)。
- **方法與函式：** `lowerCamelCase` (小駝峰式)。
- **常量：** `CONSTANT_CASE` (全大寫加底線)。
- **非常量欄位與變數：** `lowerCamelCase` (小駝峰式)。

## 7. JSDoc
- 所有類別、欄位和方法皆應使用 JSDoc。
- 使用 `@param`、`@return`、`@override`、`@deprecated`。
- 型別註解須括在大括號中 (例如：`/** @param {string} userName */`)。

*來源：[Google JavaScript Style Guide](https://google.github.io/styleguide/jsguide.html)*
