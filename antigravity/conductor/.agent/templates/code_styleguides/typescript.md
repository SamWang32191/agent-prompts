# Google TypeScript 風格指南摘要

本文件總結了 Google TypeScript 風格指南中的關鍵規則與最佳實踐，該指南由 `gts` 工具強制執行。

## 1. 語言特性
- **變數宣告：** 始終使用 `const` 或 `let`。**禁止使用 `var`。** 預設使用 `const`。
- **模組：** 使用 ES6 模組 (`import`/`export`)。**不要使用 `namespace`。**
- **匯出 (Exports)：** 使用具名匯出 (`export {MyClass};`)。**不使用預設匯出 (default exports)。**
- **類別 (Classes)：**
  - **不要使用 `#private` 欄位。** 請使用 TypeScript 的 `private` 可見性修飾符。
  - 對於在建構函式之外絕不重新賦值的屬性，請標記為 `readonly`。
  - **絕不使用 `public` 修飾符** (這是預設值)。盡可能使用 `private` 或 `protected` 限制可見性。
- **函式 (Functions)：** 具名函式優先使用函式宣告。匿名函式/回呼函式 (callbacks) 使用箭頭函式。
- **字串實字 (String Literals)：** 使用單引號 (`'`)。插值與多行字串使用樣板字面值 (`` ` ``)。
- **相等檢查：** 始終使用全等 (`===`) 與不全等 (`!==`)。
- **型別斷言 (Type Assertions)：** **避免使用型別斷言 (`x as SomeType`) 和非空值斷言 (`y!`)**。如果必須使用，請提供清晰的理由。

## 2. 禁用特性
- **`any` 型別：** **避免使用 `any`**。偏好使用 `unknown` 或更具體的型別。
- **包裝物件 (Wrapper Objects)：** 不要實例化 `String`、`Boolean` 或 `Number` 包裝類別。
- **自動分號插入 (ASI)：** 不要依賴它。**應明確地在所有陳述式結尾加上分號。**
- **`const enum`：** 不要使用 `const enum`。請改用普通 `enum`。
- **`eval()` 與 `Function(...string)`：** 禁止使用。

## 3. 命名
- **`UpperCamelCase`：** 用於類別、介面、型別、列舉 (enums) 和標籤 (decorators)。
- **`lowerCamelCase`：** 用於變數、參數、函式、方法和屬性。
- **`CONSTANT_CASE`：** 用於全域常量值，包括列舉值。
- **`_` 前綴/後綴：** **不要在識別字中使用 `_` 作為前綴或後綴**，包括私有屬性。

## 4. 型別系統
- **型別推論 (Type Inference)：** 對於簡單且明顯的型別，依賴型別推論。對於複雜型別，請明確宣告。
- **`undefined` 與 `null`：** 兩者皆支援。在你的專案中請保持一致。
- **選擇性參數 vs. `|undefined`：** 優先使用選擇性參數和欄位 (`?`)，而非在型別中加入 `|undefined`。
- **`Array<T>` 型別：** 簡單型別使用 `T[]`。對於複雜的聯合型別使用 `Array<T>` (例如：`Array<string | number>`)。
- **`{}` 型別：** **不要使用 `{}`**。偏好使用 `unknown`、`Record<string, unknown>` 或 `object`。

## 5. 註解與文件
- **JSDoc：** 文件註解使用 `/** JSDoc */`，實作註解使用 `//`。
- **冗餘性：** **不要在 `@param` 或 `@return` 區塊中重複宣告型別** (例如：`/** @param {string} user */`)。這在 TypeScript 中是多餘的。
- **增加資訊：** 註解必須增加資訊，而不僅僅是重複描述程式碼。

*來源：[Google TypeScript Style Guide](https://google.github.io/styleguide/tsguide.html)*
