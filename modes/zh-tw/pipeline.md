# 模式：pipeline -- URL Inbox（Second Brain）

處理累積在 `data/pipeline.md` 裡的職缺 URL。使用者平常看到合適職缺就丟進 inbox，之後再統一執行 `/career-ops pipeline` 批次處理。

## Workflow

1. **讀取** `data/pipeline.md` -> 找到「待處理」區塊中的 `- [ ]`
2. **對每個待處理 URL**：
   a. 計算下一個連續 `REPORT_NUM`（讀取 `reports/`，取最大編號 + 1）
   b. 用 Playwright（`browser_navigate` + `browser_snapshot`）-> WebFetch -> WebSearch 擷取 JD
   c. 如果 URL 不可存取 -> 標記為 `- [!]` 並附註解，然後繼續
   d. **執行完整 auto-pipeline**：A-F 評估 -> report `.md` -> PDF（如果 score >= 3.0）-> tracker
   e. **從「待處理」移到「已處理」**：`- [x] #NNN | URL | 公司 | 職缺 | Score/5 | PDF ✅/❌`
3. **如果待處理 URL >= 3 筆**，用背景 agent 並行跑，提高速度
4. **結束時**輸出彙總表：

```
| # | 公司 | 職缺 | Score | PDF | 建議動作 |
```

## `pipeline.md` 格式

```markdown
## 待處理
- [ ] https://jobs.example.com/posting/123
- [ ] https://boards.greenhouse.io/company/jobs/456 | Company Inc | Senior PM
- [!] https://private.url/job -- 需要登入

## 已處理
- [x] #143 | https://jobs.example.com/posting/789 | Acme Corp | AI PM | 4.2/5 | PDF ✅
- [x] #144 | https://boards.greenhouse.io/xyz/jobs/012 | BigCo | SA | 2.1/5 | PDF ❌
```

> 注意：讀取時要兼容多語標題。常見區塊標題可能是 EN 的 `Pending` / `Processed`，ES 的 `Pendientes` / `Procesadas`，DE 的 `Offen` / `Verarbeitet`，FR 的 `En attente` / `Traitees`，ZH-TW 的 `待處理` / `已處理`。寫回時盡量保持原檔風格。

## 從 URL 智慧擷取 JD

1. **Playwright（首選）**：`browser_navigate` + `browser_snapshot`，適合各種 SPA
2. **WebFetch（fallback）**：適合靜態頁或 Playwright 不可用時
3. **WebSearch（最後手段）**：去次級平台或快取來源裡找 JD

**特殊情況：**

- **LinkedIn**：可能需要登入 -> 標記 `[!]`，讓使用者直接貼文字
- **104 / Cake / Yourator**：可作為市場示例與 URL 來源，但不代表 repo 已有對它們的專用 scraper。若頁面需要登入、反爬或內容不完整，應標 `[!]` 或讓使用者貼 JD
- **PDF**：如果 URL 指向 PDF，直接用 Read tool 讀
- **`local:` 前綴**：讀取本機檔案。例如：`local:jds/linkedin-pm-ai.md` -> 讀取 `jds/linkedin-pm-ai.md`

## 自動編號

1. 列出 `reports/` 下全部檔案
2. 擷取檔名前綴編號（例如 `142-medispend...` -> 142）
3. 新編號 = 目前最大值 + 1

## 來源同步檢查

處理任何 URL 之前都先檢查 sync：

```bash
node cv-sync-check.mjs
```

如果發現不同步，先提醒使用者，再繼續。
