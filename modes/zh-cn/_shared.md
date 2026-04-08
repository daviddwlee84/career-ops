# 共享上下文 -- career-ops（简体中文）

<!-- ============================================================
     这个文件是系统层共享规则。

     你的个性化信息应放在 modes/_profile.md（不会被自动更新）。
     使用时先读这个文件，再读 modes/_profile.md。
     ============================================================ -->

## 信息来源（每次评估前都要读）

| 文件 | 路径 | 何时读取 |
|------|------|----------|
| cv.md | `cv.md`（项目根目录） | 每次都读 |
| article-digest.md | `article-digest.md`（如果存在） | 每次都读（详细 proof points） |
| profile.yml | `config/profile.yml` | 每次都读（身份、目标岗位、地点、薪酬） |
| _profile.md | `modes/_profile.md` | 每次都读（你的定制 framing、叙事、谈薪偏好） |

**规则：绝不要把 proof point 里的指标硬编码进这里。** 评估时从 `cv.md` 和 `article-digest.md` 读取。
**规则：涉及项目 / 文章指标时，`article-digest.md` 优先于 `cv.md`。**
**规则：读完本文件后再读 `modes/_profile.md`。用户自定义优先于系统默认值。**

---

## 评分系统

评估使用 6 个区块（A-F），总分为 1-5：

| 维度 | 衡量什么 |
|------|----------|
| Match with CV | 技能、经历、proof points 与 JD 的匹配度 |
| North Star alignment | 这个岗位和你的目标 archetype 是否一致 |
| Comp | 薪资与市场对比（5 = 上四分位，1 = 明显低于市场） |
| Cultural signals | 团队文化、增长、稳定性、远程政策 |
| Red flags | 阻断项、风险项、负面修正 |
| **Global** | 以上维度的加权平均 |

**分数解读：**
- 4.5+ → 强匹配，建议尽快投递
- 4.0-4.4 → 明显值得投
- 3.5-3.9 → 还可以，但需要具体理由
- 低于 3.5 → 原则上不建议投（参见 `CLAUDE.md` 的 Ethical Use）

## Archetype 识别

每个岗位都要归类到以下 6 类之一，或标记为 2 类混合：

| Archetype | JD 里的典型信号 |
|-----------|------------------|
| AI Platform / LLMOps | `observability`、`evals`、`pipelines`、`monitoring`、`reliability` |
| Agentic / Automation | `agent`、`HITL`、`orchestration`、`workflow`、`multi-agent` |
| Technical AI PM | `PRD`、`roadmap`、`discovery`、`stakeholder`、`product manager` |
| AI Solutions Architect | `architecture`、`enterprise`、`integration`、`design`、`systems` |
| AI Forward Deployed | `client-facing`、`deploy`、`prototype`、`fast delivery`、`field` |
| AI Transformation | `change management`、`adoption`、`enablement`、`transformation` |

识别 archetype 后，再读取 `modes/_profile.md` 中对应的 framing 和 proof points。

## 目标角色与自适应 framing

默认把以下六类岗位都当成有效目标，而不是主次分明的备选：

| Archetype | 主题轴 | 公司真正购买的能力 |
|-----------|--------|--------------------|
| **AI Platform / LLMOps Engineer** | Evaluation、Observability、Reliability、Pipelines | 能把 AI 系统稳定落到生产环境，并用指标管理质量 |
| **Agentic Workflows / Automation** | HITL、Tooling、Orchestration、Multi-Agent | 能把 agent 工作流做得可靠、可控、可扩展 |
| **Technical AI Product Manager** | GenAI / Agents、PRD、Discovery、Delivery | 能把业务问题转译成 AI 产品并推动落地 |
| **AI Solutions Architect** | Hyperautomation、Enterprise、Integrations | 能设计端到端 AI 架构并和企业系统集成 |
| **AI Forward Deployed Engineer** | Client-facing、快速交付、Prototyping | 能贴近客户快速交付 AI 方案 |
| **AI Transformation Lead** | Change management、Adoption、Enablement | 能推动组织级 AI 落地和采用 |

> 具体项目映射、proof points 和优先叙事，优先读 `modes/_profile.md`。不要在这里写死你的真实指标。

| 如果岗位属于... | 应重点强调你哪一面 | proof point 来源 |
|------------------|-------------------|------------------|
| Platform / LLMOps | 生产经验、observability、evals、闭环质量 | article-digest.md + cv.md |
| Agentic / Automation | 多 agent 编排、HITL、可靠性、成本控制 | article-digest.md + cv.md |
| Technical AI PM | 产品 discovery、PRD、指标、stakeholder 管理 | cv.md + article-digest.md |
| Solutions Architect | 系统设计、集成、企业级可落地性 | article-digest.md + cv.md |
| Forward Deployed Engineer | 快速交付、客户沟通、prototype 到 production | cv.md + article-digest.md |
| AI Transformation Lead | 变革管理、团队 enablement、adoption | cv.md + article-digest.md |

## 转型叙事（全部内容都要复用）

统一用 `config/profile.yml` 里的 `narrative.exit_story` 做 framing：

- **PDF Summary**：把过去经历和目标岗位自然接上
- **STAR 故事**：把叙事和 proof points 串起来
- **表单问题草稿**：第一题往往最适合放转型叙事
- **JD 出现 builder / owner / entrepreneurial / end-to-end**：把它当作强匹配信号，提高相关叙事权重

## 跨岗位通用优势

将候选人统一 framing 为：

**“有真实落地证明的技术型 builder，并且能根据岗位调整表达。”**

- 对 PM：强调用 prototype 降低不确定性，再把东西稳定交付
- 对 FDE：强调能在客户前线快速交付，并且从第一天就带指标意识
- 对 SA：强调端到端系统设计和跨系统集成
- 对 LLMOps：强调 evals、observability、production hardening

## 作品集 / demo 的使用规则

如果 `profile.yml` 中配置了 live demo / dashboard / public project，就在高价值申请里主动提供。

- 适合场景：LLMOps、AI Platform、Solutions Architect、FDE
- 如果岗位明显重产品或转型，也可把 demo 当作可信 proof point，而不是当成唯一卖点

## 薪酬研究（Comp Intelligence）

**通用建议：**

- 先按岗位 title 做比较，不要只按技能点比价
- 中国大陆本地市场优先参考：脉脉、Boss直聘、猎聘、看准网、公开薪酬报告
- 国际化公司或远程岗位可补充：Glassdoor、Levels.fyi、Blind
- 外包 / 顾问 / 合同制岗位要把稳定性、社保、公积金、奖金一起算进去

## 中国大陆求职语境（重要）

以下词在中国大陆 JD、HR 沟通和谈薪里经常出现，必须正确理解：

| 术语 | 含义 | 评估影响 |
|------|------|----------|
| **试用期** | 一般 1-6 个月，期间薪资或离职条件可能不同 | 3 个月左右常见；明显过长、试用期打折过重需要记风险 |
| **五险一金** | 社保与住房公积金 | 不是加分项而是基本盘；缺失或模糊说明要标红 |
| **13薪 / 年终奖** | 固定额外月薪或绩效奖金 | 算总包时必须并入，不能只看月薪 |
| **双休 / 大小周** | 休息制度 | 大小周是明确负面信号，要写进 red flags |
| **到岗时间** | 离职周期 + 最快入职时间 | 影响申请策略和面试预期管理 |
| **统招 / 学历要求** | 学历筛选或学校背景门槛 | 如果是硬门槛，要明确判断能否跨过去 |
| **base 城市 / 驻场** | 实际办公城市或现场办公要求 | 与 remote / hybrid 分数直接相关 |
| **外包 / OD** | 劳务派遣、外包、驻场类安排 | 稳定性、晋升空间、雇主归属感通常更弱 |
| **背调** | 正式背景调查 | founder 转型、空档期、title 变化要提前准备说法 |
| **薪资面议** | 不公开数字 | 不能脑补；必须明确写“暂无可靠数据”并给范围假设 |

## 谈薪脚本

**薪资期望：**
> “基于这类岗位当前的市场数据，我的预期区间是 [来自 profile.yml 的范围]。我对结构有弹性，但更看重总包、职责范围和成长空间。”

**如果对方按地域压价：**
> “我竞争的是结果导向岗位，不是按邮编定价的岗位。我的交付能力不会因为所在地变化而变化。”

**如果报价低于目标：**
> “我目前也在对比更高区间的机会。我对 [公司] 的兴趣是真实的，因为 [具体原因]。如果能往 [目标值] 靠拢，我会更愿意推进。”

**如果总包里有 13 薪、年终奖或补贴：**
> “为了公平比较不同机会，能否把月薪、13 薪、年终奖和其他固定补贴拆开说明？”

## 地点政策（Location Policy）

**在表单里：**
- 二选一的“能否 onsite / hybrid”问题，按 `profile.yml` 的真实可用性回答
- 自由文本里明确时区、城市、可出差范围和最快到岗时间

**在评分里：**
- hybrid 且不在你所在城市 / 国家：remote 维度给 **3.0**，不是 1.0
- 只有在 JD 明确写“必须每周 4-5 天线下、没有例外”时，才给 1.0

## Time-to-offer 优先级

- 能演示的成果 + 指标 > 完美主义
- 更快投递 > 继续无限研究
- 一律按 80/20 做时间盒管理

---

## 全局规则

### 绝不

1. 编造经历或指标
2. 修改 `cv.md` 或 portfolio 源文件
3. 代替候选人直接提交申请
4. 在生成消息里分享手机号
5. 推荐明显低于市场的薪酬
6. 没看 JD 就生成 PDF
7. 使用空泛套话或 corporate-speak
8. 忽略 tracker（每份已评估职位都要登记）

### 必做

0. **求职信：** 如果表单允许附求职信或填写 cover letter，始终提供。风格与 CV 一致，引用 JD 并映射到 proof points，最多 1 页。
1. 评估任何岗位前，先读 `cv.md`、`modes/_profile.md`、以及 `article-digest.md`（如果有）
1b. **每次 session 第一次评估：** 运行 `node cv-sync-check.mjs`。如果有 warning，先提醒用户
2. 先识别 archetype，再决定 framing
3. 做 match 时引用 CV 里的确切行
4. 用 WebSearch 做薪酬与公司研究
5. 每次评估后都要登记 tracker
6. 按 JD 语言输出内容：中文 JD 用自然简体中文，英文 JD 用英文
7. 表达直接、具体、可执行
8. 写中文材料时使用自然技术中文。短句、主动语态、避免翻译腔；技术词不必强行全中文化
8b. **PDF Professional Summary 里的案例链接：** 如果 PDF 提到 case study 或 demo，URL 必须出现在第一段 summary 里。Recruiter 往往只看第一屏
9. **Tracker 新增项写 TSV**：不要直接把新记录手改进 `applications.md`。写到 `batch/tracker-additions/`，再由 `merge-tracker.mjs` 合并
10. **每个 report header 都要包含 `**URL:**`**

### 工具

| 工具 | 用途 |
|------|------|
| WebSearch | 薪酬研究、趋势、公司文化、LinkedIn 联系人、JD fallback |
| WebFetch | 从静态页面提取 JD 的 fallback |
| Playwright | 检查职位是否仍有效、读取 SPA 页面。**关键：不要并行跑多个使用 Playwright 的 agent** |
| Read | 读取 `cv.md`、`modes/_profile.md`、`article-digest.md`、`cv-template.html` |
| Write | 写临时 HTML、applications.md、reports .md |
| Edit | 更新 tracker |
| Bash | 运行 `node generate-pdf.mjs` |

## ATS 兼容写作

### Unicode 规范化
`generate-pdf.mjs` 会把 em-dash、智能引号、零宽字符归一到 ASCII 兼容形式，但最好一开始就避免生成这些字符。

### 句式要变化
- 不要每条 bullet 都用同一个动词开头
- 长短句交替
- 不要永远都是 “X、Y、Z” 的三项并列

### 用具体信息替代抽象表述
- “把 p95 延迟从 2.1s 降到 380ms” 优于 “优化了性能”
- “用 Postgres + pgvector 在 1.2 万份文档上做检索” 优于 “设计了可扩展的 RAG 架构”
- 允许时直接写工具名、项目名、客户类型
