# 模式：scan -- Portal Scanner（职位发现）

扫描已配置的平台与公司 career 页面，按标题相关性过滤，并把新职位加入后续待评估的 pipeline。

## 推荐执行方式

建议作为子 agent 运行，避免占用主上下文：

```
Agent(
    subagent_type="general-purpose",
    prompt="[本文件内容 + 具体配置]",
    run_in_background=True
)
```

## 配置

读取 `portals.yml`，其中包含：

- `search_queries`：带 `site:` 过滤的平台搜索查询（广覆盖发现）
- `tracked_companies`：有 `careers_url` 的重点公司，可直接访问 career 页面
- `title_filter`：用于标题过滤的 `positive` / `negative` / `seniority_boost`

## 三层发现策略

### Level 1 -- 直接用 Playwright 扫公司 career 页面（主路径）

**对 `tracked_companies` 中的每家公司：** 直接访问它的 `careers_url`，读取页面上所有职位，并提取标题 + URL。这是最可靠的方法，因为：

- 看到的是实时页面，而不是搜索引擎缓存
- 对 Ashby、Lever、Workday 这类 SPA 更稳
- 能更早发现新职位
- 不依赖 Google 是否收录

**每家公司都必须有 `careers_url`。** 如果暂时没有，就先找出来并写回 `portals.yml`。

### Level 2 -- Greenhouse API（补充）

对使用 Greenhouse 的公司，可直接调用 `boards-api.greenhouse.io/v1/boards/{slug}/jobs` 获取结构化 JSON。它更快，但只适用于 Greenhouse。

### Level 3 -- WebSearch queries（广覆盖发现）

`search_queries` 里的 `site:` 查询可以横向覆盖多个平台，例如全部 Ashby、全部 Greenhouse，适合发现还没加入 `tracked_companies` 的新公司，但结果可能过期。

**执行优先级：**

1. Level 1：Playwright -> 所有有 `careers_url` 的 `tracked_companies`
2. Level 2：API -> 所有配置了 `api:` 的 `tracked_companies`
3. Level 3：WebSearch -> 所有 `enabled: true` 的 `search_queries`

这三层是叠加关系。全部执行，再统一去重。

## Workflow

1. **读取配置**：`portals.yml`
2. **读取历史**：`data/scan-history.tsv` -> 已看过的 URL
3. **读取去重源**：`data/applications.md` + `data/pipeline.md`

4. **Level 1 -- Playwright scan**（按 3-5 家一组并行）  
   对每个 `enabled: true` 且有 `careers_url` 的公司：
   a. `browser_navigate` 到 `careers_url`  
   b. `browser_snapshot` 读取所有职位  
   c. 如果页面有部门筛选 / tab，切到相关区域  
   d. 提取每个职位的 `{title, url, company}`  
   e. 如果页面有分页，继续翻页  
   f. 汇总到候选列表  
   g. 如果 `careers_url` 失效（404 / redirect），改用 `scan_query` fallback，并在总结里提示应更新 URL

5. **Level 2 -- Greenhouse API**（可并行）  
   对每个 `enabled: true` 且有 `api:` 的公司：
   a. 用 WebFetch 读取 API URL  
   b. 从 JSON 提取每个职位的 `{title, url, company}`  
   c. 加入候选列表（与 Level 1 去重）

6. **Level 3 -- WebSearch queries**（尽量并行）  
   对每个 `enabled: true` 的 query：
   a. 执行 WebSearch  
   b. 从结果中提取 `{title, url, company}`  
      - **title**：通常取搜索结果标题里 `@` 或 `|` 前面的部分  
      - **url**：结果 URL  
      - **company**：通常取 `@` 后面的部分，或从域名 / path 推断  
   c. 加入候选列表（与前两层去重）

6. **按标题过滤**，使用 `title_filter`：  
   - 至少匹配 1 个 `positive`  
   - 不匹配任何 `negative`  
   - `seniority_boost` 只加优先级，不是硬条件

7. **对 3 个来源去重：**
   - `scan-history.tsv` -> 完全相同的 URL
   - `applications.md` -> 同公司 + 同类岗位已评估
   - `pipeline.md` -> URL 已在待处理或已处理区

7.5. **在写入 pipeline 前，先做 WebSearch 结果活性校验（只针对 Level 3）**

WebSearch 结果可能过期。为避免把已下线岗位加入 pipeline，对 Level 3 的新 URL 必须再用 Playwright 逐个检查。

对每个来自 Level 3 的新 URL（顺序执行，**不要并行 Playwright**）：

a. `browser_navigate` 到 URL  
b. `browser_snapshot` 读取页面  
c. 判断状态：  
   - **活跃**：能看到职位标题、岗位内容，以及 Apply / Submit / 申请类按钮  
   - **失效**：出现以下任一信号  
     - 最终 URL 带 `?error=true`  
     - 页面出现 “job no longer available” / “no longer open” / “position has been filled” / “this job has expired” / “page not found”  
     - 只有 navbar + footer，没有实质 JD 内容（正文 < ~300 字）  
d. 如果失效 -> 在 `scan-history.tsv` 记为 `skipped_expired` 并丢弃  
e. 如果活跃 -> 继续下一步

**不要因为单个 URL 失败就中断整个 scan。** 如果 `browser_navigate` 超时、403 或其他错误，也记成 `skipped_expired` 然后继续。

8. **对每个通过校验且过滤通过的新职位：**
   a. 加到 `pipeline.md` 的“待处理”区：`- [ ] {url} | {company} | {title}`  
   b. 在 `scan-history.tsv` 记一行：`{url}\t{date}\t{query_name}\t{title}\t{company}\tadded`

9. **被标题过滤掉的职位**：记 `skipped_title`
10. **重复职位**：记 `skipped_dup`
11. **失效职位（Level 3）**：记 `skipped_expired`

## 从 WebSearch 结果提取 title 和 company

常见搜索结果格式：

- `"Job Title @ Company"`
- `"Job Title | Company"`
- `"Job Title -- Company"`

现有平台的提取示例：

- **Ashby**：`"Senior AI PM (Remote) @ EverAI"` -> title: `Senior AI PM`，company: `EverAI`
- **Greenhouse**：`"AI Engineer at Anthropic"` -> title: `AI Engineer`，company: `Anthropic`
- **Lever**：`"Product Manager - AI @ Temporal"` -> title: `Product Manager - AI`，company: `Temporal`

本地市场示例（仅作为搜索标题示例，不代表内建专用抓取器）：

- **Boss直聘**：`"大模型产品经理 - 某公司"` -> 尽量把 company 和 title 分开
- **猎聘**：`"AI 解决方案架构师_某公司"` -> 先去掉站点装饰词，再提取
- **脉脉**：如果搜索结果标题过短，就优先从域名和 snippet 推断

通用 regex：

`(.+?)(?:\s*[@|—–-]\s*|\s+at\s+)(.+?)$`

## 私有 URL

如果遇到公开不可访问的 URL：

1. 把 JD 保存到 `jds/{company}-{role-slug}.md`
2. 往 `pipeline.md` 里写：`- [ ] local:jds/{company}-{role-slug}.md | {company} | {title}`

## Scan History

`data/scan-history.tsv` 记录所有见过的 URL：

```
url	first_seen	portal	title	company	status
https://...	2026-02-10	Ashby — AI PM	PM AI	Acme	added
https://...	2026-02-10	Greenhouse — SA	Junior Dev	BigCo	skipped_title
https://...	2026-02-10	Ashby — AI PM	SA AI	OldCo	skipped_dup
https://...	2026-02-10	WebSearch — AI PM	PM AI	ClosedCo	skipped_expired
```

## 输出总结

```
Portal Scan -- {YYYY-MM-DD}
━━━━━━━━━━━━━━━━━━━━━━━━━━
执行的 queries: N
发现职位: N total
标题过滤后保留: N
重复职位: N
失效职位: N（Level 3）
新增到 pipeline.md: N

  + {company} | {title} | {query_name}
  ...

-> 运行 /career-ops pipeline 继续评估这些新职位。
```

## `careers_url` 的维护

`tracked_companies` 里的每家公司都应该保存 `careers_url`，避免每次重找。

**常见平台 URL 规则：**

- **Ashby**：`https://jobs.ashbyhq.com/{slug}`
- **Greenhouse**：`https://job-boards.greenhouse.io/{slug}` 或 `https://job-boards.eu.greenhouse.io/{slug}`
- **Lever**：`https://jobs.lever.co/{slug}`
- **Custom**：公司自己的 career 页面，例如 `https://openai.com/careers`

**如果公司还没有 `careers_url`：**

1. 先按已知 ATS 模式尝试
2. 不行就快速做 WebSearch：`"{company}" careers jobs`
3. 用 Playwright 验证页面可用
4. **把找到的 URL 写回 `portals.yml`**，供之后复用

**如果 `careers_url` 返回 404 或 redirect：**

1. 在输出总结里记一条
2. 用 `scan_query` 作为 fallback
3. 标记为待人工更新

## `portals.yml` 维护规则

- 新增公司时，**始终保存 `careers_url`**
- 发现值得长期跟踪的公司，就加到 `tracked_companies`
- 发现噪音很高的 query，就把 `enabled` 设为 `false`
- 随目标岗位变化，持续微调 `title_filter`
- 对 Boss直聘 / 脉脉 / 猎聘 这类平台，只在文档里作为市场语境和搜索示例引用，不表示 repo 已有专用 scraper
