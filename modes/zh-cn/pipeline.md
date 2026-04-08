# 模式：pipeline -- URL Inbox（Second Brain）

处理积累在 `data/pipeline.md` 里的职位 URL。用户平时看到合适岗位就丢进 inbox，之后再统一执行 `/career-ops pipeline` 批量处理。

## Workflow

1. **读取** `data/pipeline.md` -> 找到 “待处理” 区块里的 `- [ ]`
2. **对每个待处理 URL**：
   a. 计算下一个连续 `REPORT_NUM`（读取 `reports/`，取最大编号 + 1）
   b. 用 Playwright（`browser_navigate` + `browser_snapshot`）-> WebFetch -> WebSearch 提取 JD
   c. 如果 URL 不可访问 -> 标记为 `- [!]` 并附注释，然后继续
   d. **执行完整 auto-pipeline**：A-F 评估 -> report `.md` -> PDF（如果 score >= 3.0）-> tracker
   e. **从“待处理”移到“已处理”**：`- [x] #NNN | URL | 公司 | 岗位 | Score/5 | PDF ✅/❌`
3. **如果待处理 URL >= 3 条**，用后台 agent 并行跑，提高速度
4. **结束时**输出汇总表：

```
| # | 公司 | 岗位 | Score | PDF | 建议动作 |
```

## `pipeline.md` 格式

```markdown
## 待处理
- [ ] https://jobs.example.com/posting/123
- [ ] https://boards.greenhouse.io/company/jobs/456 | Company Inc | Senior PM
- [!] https://private.url/job -- 需要登录

## 已处理
- [x] #143 | https://jobs.example.com/posting/789 | Acme Corp | AI PM | 4.2/5 | PDF ✅
- [x] #144 | https://boards.greenhouse.io/xyz/jobs/012 | BigCo | SA | 2.1/5 | PDF ❌
```

> 注意：读取时要兼容多语言标题。常见区块标题可能是 EN 的 `Pending` / `Processed`，ES 的 `Pendientes` / `Procesadas`，DE 的 `Offen` / `Verarbeitet`，FR 的 `En attente` / `Traitees`，ZH-CN 的 `待处理` / `已处理`。写回时尽量保持文件原本风格。

## 从 URL 智能提取 JD

1. **Playwright（首选）**：`browser_navigate` + `browser_snapshot`，适合各种 SPA
2. **WebFetch（fallback）**：适合静态页面或 Playwright 不可用时
3. **WebSearch（最后手段）**：去二级平台或缓存来源里找 JD

**特殊情况：**

- **LinkedIn**：可能要求登录 -> 标记 `[!]`，让用户直接贴文本
- **Boss直聘 / 脉脉 / 猎聘**：可能存在登录墙、强动态加载或风控。如果拿不到完整 JD，就标 `[!]` 或让用户粘贴职位正文，不要假装已经有平台专用抓取器
- **PDF**：如果 URL 指向 PDF，直接用 Read tool 读
- **`local:` 前缀**：读取本地文件。例如：`local:jds/linkedin-pm-ai.md` -> 读取 `jds/linkedin-pm-ai.md`

## 自动编号

1. 列出 `reports/` 下的全部文件
2. 提取文件名前缀编号（例如 `142-medispend...` -> 142）
3. 新编号 = 当前最大值 + 1

## 来源同步检查

处理任何 URL 之前都先检查 sync：

```bash
node cv-sync-check.mjs
```

如果发现不同步，先提醒用户，再继续。
