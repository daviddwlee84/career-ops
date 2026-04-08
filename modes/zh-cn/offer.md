# 模式：offer -- A-F 完整评估

当候选人贴入一个职位（文本或 URL）时，始终输出 6 个区块：

## 第 0 步 -- Archetype 识别

把岗位归类到 6 个 archetype 之一（见 `_shared.md`）。如果是混合型，就写出最接近的 2 类。这会决定：

- 在 B 区块优先使用哪些 proof points
- 在 E 区块如何重写 summary
- 在 F 区块准备哪些 STAR 故事

## A 区块 -- 岗位摘要

输出表格，包含：

- 检测到的 archetype
- Domain（platform / agentic / LLMOps / ML / enterprise）
- Function（build / consult / manage / deploy）
- Seniority
- Remote（full / hybrid / onsite）
- Team size（如果 JD 提到）
- 1 句 TL;DR

## B 区块 -- 与 CV 的匹配度

读取 `cv.md`。把 JD 里的每个要求映射到 CV 的确切行号。

**按 archetype 做优先级调整：**

- 如果是 FDE → 优先强调快速交付和 client-facing 证明
- 如果是 SA → 优先强调系统设计和 integrations
- 如果是 PM → 优先强调 product discovery 和 metrics
- 如果是 LLMOps → 优先强调 evals、observability、pipelines
- 如果是 Agentic → 优先强调 multi-agent、HITL、orchestration
- 如果是 Transformation → 优先强调 change management、adoption、scaling

再补一个 **gaps** 小节，对每个 gap 给出缓解策略。每个 gap 都要回答：

1. 它是 hard blocker 还是 nice-to-have？
2. 候选人能否用相邻经验证明？
3. 是否有 portfolio project 能覆盖？
4. 明确的缓解动作是什么（例如 cover letter 里如何说、补一个小项目、面试时如何 framing）？

## C 区块 -- 级别与策略

1. **JD 推断级别** 对比 **候选人在该 archetype 下的自然级别**
2. **“不撒谎地卖 senior” 方案**：给出具体说法、应突出哪些成果、如何把 founder / owner 经历说成优势
3. **“如果被 downlevel” 方案**：什么条件下可以接受、如何谈 6 个月 review、如何要求明确晋升标准

## D 区块 -- 薪酬与需求趋势

用 WebSearch 研究：

- 该岗位的当前薪资（优先：脉脉、Boss直聘、猎聘、看准网；国际岗位可补 Glassdoor、Levels.fyi、Blind）
- 公司在薪酬方面的口碑
- 该岗位的市场需求趋势

输出带引用来源的表格。没有数据就明确写“暂无可靠数据”，不要猜。

## E 区块 -- 个性化改写计划

| # | 部分 | 当前状态 | 建议修改 | 原因 |
|---|------|----------|----------|------|
| 1 | Summary | ... | ... | ... |
| ... | ... | ... | ... | ... |

最后给出：

- Top 5 简历修改点
- Top 5 LinkedIn 资料修改点

## F 区块 -- 面试准备计划

准备 6-10 个 STAR+R 故事，映射到 JD 要求（STAR + **Reflection**）：

| # | JD 要求 | STAR+R 故事 | S | T | A | R | Reflection |
|---|---------|-------------|---|---|---|---|------------|

`Reflection` 要写出复盘：学到了什么、下次会怎么做。高级候选人不是只讲发生了什么，而是能提炼方法论。

**Story Bank：** 如果存在 `interview-prep/story-bank.md`，先检查这些故事是否已经在里面。没有的话追加进去，逐步形成可复用的 5-10 个母故事库。

**按 archetype 调整故事 framing：**

- FDE → 强调交付速度、client-facing、现场推进
- SA → 强调架构取舍与系统边界
- PM → 强调 discovery、trade-offs、指标判断
- LLMOps → 强调指标、evals、production hardening
- Agentic → 强调 orchestration、error handling、HITL
- Transformation → 强调 adoption、组织推动、change management

同时补充：

- 1 个推荐展示的 case study（展示哪个项目、怎么讲）
- red-flag 问题及答法（例如“为什么卖掉公司？”“你带过团队吗？”“为什么想从创业转成 IC / PM / SA？”）

---

## 评估完成后

**输出完 A-F 后始终执行：**

### 1. 保存 report `.md`

把完整评估保存到 `reports/{###}-{company-slug}-{YYYY-MM-DD}.md`

- `{###}` = 下一个连续编号（3 位数，左侧补零）
- `{company-slug}` = 公司名小写、空格改连字符
- `{YYYY-MM-DD}` = 当前日期

**report 格式：**

```markdown
# 评估：{公司} -- {岗位}

**日期：** {YYYY-MM-DD}
**Archetype：** {detectado}
**Score：** {X/5}
**URL：** {职位链接}
**PDF：** {路径或待生成}

---

## A) 岗位摘要
（完整 A 区块内容）

## B) 与 CV 的匹配度
（完整 B 区块内容）

## C) 级别与策略
（完整 C 区块内容）

## D) 薪酬与需求趋势
（完整 D 区块内容）

## E) 个性化改写计划
（完整 E 区块内容）

## F) 面试准备计划
（完整 F 区块内容）

## G) Draft Application Answers
（只有 score >= 4.5 时才写，作为申请表问题草稿）

---

## 提取出的 Keywords
（列出 15-20 个 JD 关键词，用于 ATS 优化）
```

### 2. 登记 tracker

**始终**在 `data/applications.md` 中登记：

- 下一个连续编号
- 当前日期
- 公司名
- 岗位名
- Score：match 的 1-5 平均分
- 状态：`Evaluated`
- PDF：❌（如果 auto-pipeline 同时生成了 PDF，则写 ✅）
- Report：report `.md` 的相对链接，例如 `[001](reports/001-company-2026-01-01.md)`

**tracker 格式：**

```markdown
| # | 日期 | 公司 | 岗位 | Score | 状态 | PDF | Report |
```
