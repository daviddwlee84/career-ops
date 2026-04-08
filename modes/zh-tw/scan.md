# 模式：scan -- Portal Scanner（職缺發現）

掃描已配置的平台與公司 career 頁面，依標題相關性過濾，並把新職缺加入後續待評估的 pipeline。

## 建議執行方式

建議作為子 agent 執行，避免占用主上下文：

```
Agent(
    subagent_type="general-purpose",
    prompt="[本檔案內容 + 具體設定]",
    run_in_background=True
)
```

## 設定

讀取 `portals.yml`，其中包含：

- `search_queries`：帶 `site:` 過濾的平台搜尋查詢（廣覆蓋發現）
- `tracked_companies`：有 `careers_url` 的重點公司，可直接存取 career 頁面
- `title_filter`：用於標題過濾的 `positive` / `negative` / `seniority_boost`

## 三層發現策略

### Level 1 -- 直接用 Playwright 掃公司 career 頁面（主路徑）

**對 `tracked_companies` 中的每家公司：** 直接打開它的 `careers_url`，讀取頁面上所有職缺，並擷取標題 + URL。這是最可靠的方法，因為：

- 看到的是即時頁面，不是搜尋引擎快取
- 對 Ashby、Lever、Workday 這類 SPA 較穩
- 能更早發現新職缺
- 不依賴 Google 是否收錄

**每家公司都必須有 `careers_url`。** 如果暫時沒有，就先找出來並寫回 `portals.yml`。

### Level 2 -- Greenhouse API（補充）

對使用 Greenhouse 的公司，可直接呼叫 `boards-api.greenhouse.io/v1/boards/{slug}/jobs` 取得結構化 JSON。它更快，但只適用於 Greenhouse。

### Level 3 -- WebSearch queries（廣覆蓋發現）

`search_queries` 裡的 `site:` 查詢可以橫向覆蓋多個平台，例如全部 Ashby、全部 Greenhouse，適合發現還沒加入 `tracked_companies` 的新公司，但結果可能過期。

**執行優先順序：**

1. Level 1：Playwright -> 所有有 `careers_url` 的 `tracked_companies`
2. Level 2：API -> 所有設定了 `api:` 的 `tracked_companies`
3. Level 3：WebSearch -> 所有 `enabled: true` 的 `search_queries`

這三層是疊加關係。全部執行，再統一去重。

## Workflow

1. **讀取設定**：`portals.yml`
2. **讀取歷史**：`data/scan-history.tsv` -> 已看過的 URL
3. **讀取去重來源**：`data/applications.md` + `data/pipeline.md`

4. **Level 1 -- Playwright scan**（以 3-5 家為一批並行）  
   對每個 `enabled: true` 且有 `careers_url` 的公司：
   a. `browser_navigate` 到 `careers_url`  
   b. `browser_snapshot` 讀取所有職缺  
   c. 如果頁面有部門篩選 / tab，切到相關區域  
   d. 擷取每個職缺的 `{title, url, company}`  
   e. 如果頁面有分頁，繼續翻頁  
   f. 匯總到候選清單  
   g. 如果 `careers_url` 失效（404 / redirect），改用 `scan_query` fallback，並在總結裡提示應更新 URL

5. **Level 2 -- Greenhouse API**（可並行）  
   對每個 `enabled: true` 且有 `api:` 的公司：
   a. 用 WebFetch 讀取 API URL  
   b. 從 JSON 擷取每個職缺的 `{title, url, company}`  
   c. 加入候選清單（與 Level 1 去重）

6. **Level 3 -- WebSearch queries**（盡量並行）  
   對每個 `enabled: true` 的 query：
   a. 執行 WebSearch  
   b. 從結果中擷取 `{title, url, company}`  
      - **title**：通常取搜尋結果標題中 `@` 或 `|` 前面的部分  
      - **url**：結果 URL  
      - **company**：通常取 `@` 後面的部分，或從網域 / path 推斷  
   c. 加入候選清單（與前兩層去重）

6. **依標題過濾**，使用 `title_filter`：  
   - 至少匹配 1 個 `positive`  
   - 不匹配任何 `negative`  
   - `seniority_boost` 只加優先級，不是硬條件

7. **對 3 個來源去重：**
   - `scan-history.tsv` -> 完全相同的 URL
   - `applications.md` -> 同公司 + 同類職缺已評估
   - `pipeline.md` -> URL 已在待處理或已處理區

7.5. **在寫入 pipeline 前，先做 WebSearch 結果活性檢查（只針對 Level 3）**

WebSearch 結果可能過期。為避免把已下線職缺加進 pipeline，對 Level 3 的新 URL 必須再用 Playwright 逐一檢查。

對每個來自 Level 3 的新 URL（順序執行，**不要並行 Playwright**）：

a. `browser_navigate` 到 URL  
b. `browser_snapshot` 讀取頁面  
c. 判斷狀態：  
   - **有效**：能看到職缺標題、內容，以及 Apply / Submit / 申請類按鈕  
   - **失效**：出現以下任一訊號  
     - 最終 URL 帶 `?error=true`  
     - 頁面出現 “job no longer available” / “no longer open” / “position has been filled” / “this job has expired” / “page not found”  
     - 只有 navbar + footer，沒有實質 JD 內容（正文 < ~300 字）  
d. 如果失效 -> 在 `scan-history.tsv` 記為 `skipped_expired` 並丟棄  
e. 如果有效 -> 繼續下一步

**不要因為單一 URL 失敗就中斷整個 scan。** 如果 `browser_navigate` timeout、403 或其他錯誤，也記成 `skipped_expired` 然後繼續。

8. **對每個通過檢查且過濾通過的新職缺：**
   a. 加到 `pipeline.md` 的「待處理」區：`- [ ] {url} | {company} | {title}`  
   b. 在 `scan-history.tsv` 記一行：`{url}\t{date}\t{query_name}\t{title}\t{company}\tadded`

9. **被標題過濾掉的職缺**：記 `skipped_title`
10. **重複職缺**：記 `skipped_dup`
11. **失效職缺（Level 3）**：記 `skipped_expired`

## 從 WebSearch 結果擷取 title 和 company

常見搜尋結果格式：

- `"Job Title @ Company"`
- `"Job Title | Company"`
- `"Job Title -- Company"`

現有平台的擷取示例：

- **Ashby**：`"Senior AI PM (Remote) @ EverAI"` -> title: `Senior AI PM`，company: `EverAI`
- **Greenhouse**：`"AI Engineer at Anthropic"` -> title: `AI Engineer`，company: `Anthropic`
- **Lever**：`"Product Manager - AI @ Temporal"` -> title: `Product Manager - AI`，company: `Temporal`

本地市場示例（僅作為搜尋標題示例，不代表內建專用抓取器）：

- **104**：搜尋結果常含職稱、公司與站點裝飾字，先去掉站點名再判斷
- **Cake**：如果結果標題偏品牌導向，可從 snippet 補公司資訊
- **Yourator**：職稱通常較乾淨，但仍要檢查是否為職缺列表頁而非單一 JD

通用 regex：

`(.+?)(?:\s*[@|—–-]\s*|\s+at\s+)(.+?)$`

## 私有 URL

如果遇到公開不可存取的 URL：

1. 把 JD 儲存到 `jds/{company}-{role-slug}.md`
2. 往 `pipeline.md` 裡寫：`- [ ] local:jds/{company}-{role-slug}.md | {company} | {title}`

## Scan History

`data/scan-history.tsv` 記錄所有看過的 URL：

```
url	first_seen	portal	title	company	status
https://...	2026-02-10	Ashby — AI PM	PM AI	Acme	added
https://...	2026-02-10	Greenhouse — SA	Junior Dev	BigCo	skipped_title
https://...	2026-02-10	Ashby — AI PM	SA AI	OldCo	skipped_dup
https://...	2026-02-10	WebSearch — AI PM	PM AI	ClosedCo	skipped_expired
```

## 輸出總結

```
Portal Scan -- {YYYY-MM-DD}
━━━━━━━━━━━━━━━━━━━━━━━━━━
執行的 queries: N
發現職缺: N total
標題過濾後保留: N
重複職缺: N
失效職缺: N（Level 3）
新增到 pipeline.md: N

  + {company} | {title} | {query_name}
  ...

-> 執行 /career-ops pipeline 繼續評估這些新職缺。
```

## `careers_url` 的維護

`tracked_companies` 裡的每家公司都應該保存 `careers_url`，避免每次重找。

**常見平台 URL 規則：**

- **Ashby**：`https://jobs.ashbyhq.com/{slug}`
- **Greenhouse**：`https://job-boards.greenhouse.io/{slug}` 或 `https://job-boards.eu.greenhouse.io/{slug}`
- **Lever**：`https://jobs.lever.co/{slug}`
- **Custom**：公司自己的 career 頁，例如 `https://openai.com/careers`

**如果公司還沒有 `careers_url`：**

1. 先按已知 ATS 模式嘗試
2. 不行就快速做 WebSearch：`"{company}" careers jobs`
3. 用 Playwright 驗證頁面可用
4. **把找到的 URL 寫回 `portals.yml`**，供之後重用

**如果 `careers_url` 回傳 404 或 redirect：**

1. 在輸出總結裡記一條
2. 用 `scan_query` 當 fallback
3. 標記為待人工更新

## `portals.yml` 維護規則

- 新增公司時，**始終保存 `careers_url`**
- 發現值得長期追蹤的公司，就加到 `tracked_companies`
- 發現噪音很高的 query，就把 `enabled` 設成 `false`
- 隨目標職缺變化，持續微調 `title_filter`
- 對 104 / Cake / Yourator 這類平台，只在文件裡作為市場語境與搜尋示例引用，不表示 repo 已有專用 scraper
