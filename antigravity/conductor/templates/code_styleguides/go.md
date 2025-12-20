# Effective Go 風格指南摘要

本文件總結了官方「Effective Go」指南中編寫道地 Go 程式碼的關鍵規則與最佳實踐。

## 1. 格式化 (Formatting)
- **`gofmt`：** 所有 Go 程式碼**必須**使用 `gofmt` (或 `go fmt`) 進行格式化。這是不可商榷的自動化標準。
- **縮排 (Indentation)：** 使用 tab 進行縮排 (`gofmt` 會自動處理)。
- **行長度 (Line Length)：** Go 沒有嚴格的行長度限制。讓 `gofmt` 處理換行。

## 2. 命名 (Naming)
- **`MixedCaps`：** 多個單字組成的名稱使用 `MixedCaps` 或 `mixedCaps`。不要使用底線。
- **匯出與未匯出 (Exported vs. Unexported)：** 大寫字母開頭的名稱為匯出 (公開/public)；小寫字母開頭的名稱不匯出 (私有/private)。
- **套件名稱 (Package Names)：** 使用簡短、精煉、單一單字的小寫名稱。
- **Getter：** 不要為 Getter 加入 `Get` 前綴。名為 `owner` 的欄位，其 Getter 應命名為 `Owner()`。
- **介面名稱 (Interface Names)：** 只有一個方法的介面，通常以該方法名稱加上 `-er` 字尾命名 (例如：`Reader`、`Writer`)。

## 3. 控制結構 (Control Structures)
- **`if`：** 條件語句不加括號。大括號是強制性的。可以包含初始化陳述式 (例如：`if err := file.Chmod(0664); err != nil`)。
- **`for`：** Go 唯一的一種迴圈結構，統一了 `for` 和 `while`。使用 `for...range` 來迭代切片 (slice)、映射 (map)、字串和通道 (channel)。
- **`switch`：** 比 C 語言更通用。Case 預設不會貫穿 (若需要請明確使用 `fallthrough`)。可以不帶運算式，作為更整潔的 `if-else-if` 鏈。

## 4. 函式 (Functions)
- **多重回傳值 (Multiple Returns)：** 函式可以回傳多個值。這是回傳結果與錯誤的標準方式 (例如：`value, err`)。
- **具名回傳參數 (Named Result Parameters)：** 回傳參數可以命名，這能使程式碼更清晰精簡。
- **`defer`：** 排定函式呼叫在執行 `defer` 的函式回傳前立即執行。用於關閉檔案等清理任務。

## 5. 資料 (Data)
- **`new` 與 `make`：**
  - `new(T)`：為型別 `T` 的新項目分配記憶體並將其歸零，回傳指標 (`*T`)。
  - `make(T, ...)`：僅用於建立和初始化切片、映射與通道。回傳型別 `T` 的初始化值 (而非指標)。
- **切片 (Slices)：** 處理序列的首選方式。比陣列 (array) 更有彈性。
- **映射 (Maps)：** 使用 "comma ok" 慣用語來檢查鍵 (key) 是否存在：`value, ok := myMap[key]`。

## 6. 介面 (Interfaces)
- **隱式實作 (Implicit Implementation)：** 型別只要實作介面定義的方法，即表示實作了該介面，無需 `implements` 關鍵字。
- **小型介面 (Small Interfaces)：** 優先考慮多個小型介面而非單一大型介面。標準函式庫中充滿了單一方法的介面 (例如：`io.Reader`)。

## 7. 並行性 (Concurrency)
- **透過溝通分享記憶體 (Share Memory By Communicating)：** 這是核心哲學。不要透過共享記憶體來溝通，而是透過溝通來共享記憶體。
- **Goroutines：** 輕量級、並行執行的函式。使用 `go` 關鍵字啟動。
- **通道 (Channels)：** 用於 Goroutines 之間溝通的具型別管道。使用 `make` 建立。

## 8. 錯誤處理 (Errors)
- **`error` 型別：** 內建的 `error` 介面是處理錯誤的標準方式。
- **顯式錯誤處理：** 不要用空白識別字 (`_`) 捨棄錯誤。應明確檢查錯誤。
- **`panic`：** 僅保留給真正異常、無法恢復的情況。通常函式庫不應該使用 panic。

*來源：[Effective Go](https://go.dev/doc/effective_go)*
