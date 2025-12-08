---
trigger: glob
globs: **/*.ts, **/*.js, **/package.json
---

# TypeScript MCP Server開發

## 指引說明

- 使用 **@modelcontextprotocol/sdk** pnpm 套件：`pnpm install @modelcontextprotocol/sdk`
- 從特定路徑匯入：`@modelcontextprotocol/sdk/server/mcp.js`、`@modelcontextprotocol/sdk/server/stdio.js` 等。
- 使用 `McpServer` 類別來實作具有自動通訊協定處理的高階伺服器。
- 使用 `Server` 類別進行手動請求處理的低階控制。
- 使用 **zod** 進行輸入/輸出架構（schema）驗證：`pnpm install zod@3`
- 務必為工具 (tools)、資源 (resources) 和提示詞 (prompts) 提供 `title` 欄位，以優化 UI 顯示效果。
- 使用 `registerTool()`、`registerResource()` 和 `registerPrompt()` 方法（推薦使用這些而非舊版 API）。
- 使用 zod 定義架構：`{ inputSchema: { param: z.string() }, outputSchema: { result: z.string() } }`
- 工具回傳時需同時包含 `content`（供顯示用）與 `structuredContent`（供結構化資料用）。
- 對於 HTTP 伺服器，使用 `StreamableHTTPServerTransport` 搭配 Express 或類似框架。
- 對於本地端整合，使用 `StdioServerTransport` 進行基於 stdio 的通訊。
- 每個請求請建立新的傳輸實例 (transport instances) 以防止請求 ID 衝突（無狀態模式）。
- 對於有狀態伺服器，使用 `sessionIdGenerator` 進行工作階段 (session) 管理。
- 為本地伺服器啟用 DNS 重綁定保護：`enableDnsRebindingProtection: true`
- 設定 CORS 標頭並為瀏覽器端客戶端暴露 `Mcp-Session-Id`。
- 使用 `ResourceTemplate` 處理帶有 URI 參數的動態資源：`new ResourceTemplate('resource://{param}', { list: undefined })`
- 使用 `@modelcontextprotocol/sdk/server/completable.js` 中的 `completable()` 包裝器來支援自動完成，以提升使用者體驗 (UX)。
- 使用 `server.server.createMessage()` 實作採樣 (sampling)，向客戶端請求 LLM 完成。
- 使用 `server.server.elicitInput()` 在工具執行期間請求使用者額外輸入。
- 啟用大量更新的通知防抖動 (debouncing)：`debouncedNotificationMethods: ['notifications/tools/list_changed']`
- 動態更新：在已註冊的項目上呼叫 `.enable()`、`.disable()`、`.update()` 或 `.remove()` 以發送 `listChanged` 通知。
- 使用 `@modelcontextprotocol/sdk/shared/metadataUtils.js` 中的 `getDisplayName()` 來取得 UI 顯示名稱。
- 使用 MCP Inspector 測試伺服器：`npx @modelcontextprotocol/inspector`

## 最佳實踐

- 保持工具的實作專注於單一職責。
- 提供清晰、描述性的標題與說明，以便 LLM 理解。
- 為所有參數和回傳值使用正確的 TypeScript 型別。
- 使用 try-catch 區塊實作全面的錯誤處理。
- 發生錯誤狀況時，在工具結果中回傳 `isError: true`。
- 所有非同步操作皆使用 async/await。
- 正確關閉資料庫連線並清理資源。
- 在處理前先驗證輸入參數。
- 使用結構化日誌進行除錯，避免污染標準輸出/標準錯誤 (stdout/stderr)。
- 當暴露檔案系統或網路存取權限時，請考量安全性影響。
- 在傳輸關閉事件 (transport close events) 中實作正確的資源清理。
- 使用環境變數進行配置（連接埠、API 金鑰等）。
- 清楚記錄工具的功能與限制。
- 使用多種客戶端進行測試以確保相容性。

## 常見模式

### 基礎伺服器設定 (HTTP)
```typescript
import { McpServer } from '@modelcontextprotocol/sdk/server/mcp.js';
import { StreamableHTTPServerTransport } from '@modelcontextprotocol/sdk/server/streamableHttp.js';
import express from 'express';

const server = new McpServer({
    name: 'my-server',
    version: '1.0.0'
});

const app = express();
app.use(express.json());

app.post('/mcp', async (req, res) => {
    const transport = new StreamableHTTPServerTransport({
        sessionIdGenerator: undefined,
        enableJsonResponse: true
    });
    
    res.on('close', () => transport.close());
    
    await server.connect(transport);
    await transport.handleRequest(req, res, req.body);
});

app.listen(3000);
```

### 基礎伺服器設定 (stdio)
```typescript
import { McpServer } from '@modelcontextprotocol/sdk/server/mcp.js';
import { StdioServerTransport } from '@modelcontextprotocol/sdk/server/stdio.js';

const server = new McpServer({
    name: 'my-server',
    version: '1.0.0'
});

// ... 註冊工具、資源、提示詞 ...

const transport = new StdioServerTransport();
await server.connect(transport);
```

### 簡易工具 (Simple Tool)
```typescript
import { z } from 'zod';

server.registerTool(
    'calculate',
    {
        title: 'Calculator', // 計算機
        description: 'Perform basic calculations', // 執行基本運算
        inputSchema: { a: z.number(), b: z.number(), op: z.enum(['+', '-', '*', '/']) },
        outputSchema: { result: z.number() }
    },
    async ({ a, b, op }) => {
        const result = op === '+' ? a + b : op === '-' ? a - b : 
                      op === '*' ? a * b : a / b;
        const output = { result };
        return {
            content: [{ type: 'text', text: JSON.stringify(output) }],
            structuredContent: output
        };
    }
);
```

### 動態資源 (Dynamic Resource)
```typescript
import { ResourceTemplate } from '@modelcontextprotocol/sdk/server/mcp.js';

server.registerResource(
    'user',
    new ResourceTemplate('users://{userId}', { list: undefined }),
    {
        title: 'User Profile', // 使用者個人資料
        description: 'Fetch user profile data' // 取得使用者個人資料數據
    },
    async (uri, { userId }) => ({
        contents: [{
            uri: uri.href,
            text: `User ${userId} data here`
        }]
    })
);
```

### 帶有採樣功能的工具 (Tool with Sampling)
```typescript
server.registerTool(
    'summarize',
    {
        title: 'Text Summarizer',
        description: 'Summarize text using LLM',
        inputSchema: { text: z.string() },
        outputSchema: { summary: z.string() }
    },
    async ({ text }) => {
        const response = await server.server.createMessage({
            messages: [{
                role: 'user',
                content: { type: 'text', text: `Summarize: ${text}` }
            }],
            maxTokens: 500
        });
        
        const summary = response.content.type === 'text' ? 
            response.content.text : 'Unable to summarize';
        const output = { summary };
        return {
            content: [{ type: 'text', text: JSON.stringify(output) }],
            structuredContent: output
        };
    }
);
```

### 帶有自動完成的提示詞 (Prompt with Completion)
```typescript
import { completable } from '@modelcontextprotocol/sdk/server/completable.js';

server.registerPrompt(
    'review',
    {
        title: 'Code Review',
        description: 'Review code with specific focus',
        argsSchema: {
            language: completable(z.string(), value => 
                ['typescript', 'python', 'javascript', 'java']
                    .filter(l => l.startsWith(value))
            ),
            code: z.string()
        }
    },
    ({ language, code }) => ({
        messages: [{
            role: 'user',
            content: {
                type: 'text',
                text: `Review this ${language} code:\n\n${code}`
            }
        }]
    })
);
```

### 錯誤處理 (Error Handling)
```typescript
server.registerTool(
    'risky-operation',
    {
        title: 'Risky Operation',
        description: 'An operation that might fail',
        inputSchema: { input: z.string() },
        outputSchema: { result: z.string() }
    },
    async ({ input }) => {
        try {
            const result = await performRiskyOperation(input);
            const output = { result };
            return {
                content: [{ type: 'text', text: JSON.stringify(output) }],
                structuredContent: output
            };
        } catch (err: unknown) {
            const error = err as Error;
            return {
                content: [{ type: 'text', text: `Error: ${error.message}` }],
                isError: true
            };
        }
    }
);
```