# 輸出模式

當 Skills 需要產生一致、高品質的輸出時，使用這些模式。

## 模板模式

提供輸出格式的模板。根據你的需求調整嚴格程度。

**對於嚴格要求（如 API 回應或資料格式）：**

```markdown
## 報告結構

務必使用此確切的模板結構：

# [分析標題]

## 執行摘要
[一段關於關鍵發現的概覽]

## 關鍵發現
- 發現 1 及佐證資料
- 發現 2 及佐證資料
- 發現 3 及佐證資料

## 建議
1. 具體可行的建議
2. 具體可行的建議
```

**對於彈性指引（當適應性很有用時）：**

```markdown
## 報告結構

這是一個合理的預設格式，但請運用你的判斷力：

# [分析標題]

## 執行摘要
[概覽]

## 關鍵發現
[根據你的發現調整章節]

## 建議
[針對具體情境量身打造]

依據特定分析類型需要調整章節。
```

## 範例模式

對於輸出品質取決於即視範例的 Skills，提供輸入/輸出對：

```markdown
## Commit 訊息格式

遵循這些範例產生 commit 訊息：

**範例 1：**
輸入：Added user authentication with JWT tokens
輸出：
    ```
    feat(auth): implement JWT-based authentication

    Add login endpoint and token validation middleware
    ```

**範例 2：**

輸入：Fixed bug where dates displayed incorrectly in reports
輸出：
    ```
    fix(reports): correct date formatting in timezone conversion

    Use UTC timestamps consistently across report generation
    ```

遵循此風格：type(scope): 簡短描述，然後是詳細解釋。
```
