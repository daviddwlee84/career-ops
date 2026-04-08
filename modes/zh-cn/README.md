# career-ops -- 简体中文模式（`modes/zh-cn/`）

这个目录提供面向中国大陆求职市场的简体中文版本 career-ops 模式。它不是把原文做简繁转换，而是按中国大陆常见招聘语境、合同词汇和平台习惯重写。

## 什么时候用这些模式？

如果满足以下任一条件，就优先使用 `modes/zh-cn/`：

- 你主要投递的是**简体中文职位**，例如 Boss直聘、脉脉、猎聘、公司招聘页上的中文 JD
- 你的**简历语言**是简体中文，或你会在中文 / 英文 JD 之间切换
- 你需要**自然的中文技术表达**，而不是翻译腔
- 你需要处理中国大陆求职语境里的常见概念：试用期、五险一金、13薪 / 年终奖、双休 / 大小周、到岗时间、统招、base 城市、驻场等

如果你多数投递的是英文岗位，即使公司在中国大陆，也优先使用根目录下的默认英文模式 `modes/`。英文模式可以处理中文市场公司，但不会像这里一样默认带入中国大陆语境。

## 怎么启用？

### 方式 1：按会话指定

在 session 开始时直接告诉 Claude：

> "请使用 `modes/zh-cn/` 下的简体中文模式。"

或者更明确一点：

> "请用简体中文模式评估和申请岗位，优先读 `modes/zh-cn/_shared.md` 和 `modes/zh-cn/offer.md`。"

Claude 随后就会读取这个目录，而不是默认的 `modes/`。

### 方式 2：写进 `config/profile.yml`

```yaml
language:
  primary: zh-CN
  modes_dir: modes/zh-cn
```

第一次会话时提醒 Claude 注意这个字段，例如：

> "看一下 `profile.yml`，我已经把 `language.modes_dir` 设成 `modes/zh-cn`。"

> 说明：`language.modes_dir` 目前仍是文档约定和 agent 使用规则，不是硬编码开关。

## 这个目录包含什么？

本轮目录共 6 个文件：

| 文件 | 来源 | 用途 |
|------|------|------|
| `README.md` | 新增 | 说明何时使用简体中文模式、如何启用、哪些内容已本地化 |
| `_shared.md` | `modes/_shared.md` | 共享上下文、评分规则、工具规则、中国大陆市场语境 |
| `offer.md` | `modes/oferta.md` | 单个岗位的完整 A-F 评估 |
| `apply.md` | `modes/apply.md` | 实时申请表填写助手 |
| `pipeline.md` | `modes/pipeline.md` | URL inbox / 待处理职位流水线 |
| `scan.md` | `modes/scan.md` | 平台扫描与新职位发现 |

其他 mode 例如 `batch`、`pdf`、`tracker`、`auto-pipeline`、`deep`、`contacto`、`project`、`training` 这一轮仍沿用根目录下的 EN/ES 原版。

## 哪些内容仍保留英文？

以下内容刻意不做硬翻译：

- `cv.md`、`pipeline`、`tracker`、`report`、`score`、`archetype`、`proof point`
- 工具名：`Playwright`、`WebSearch`、`WebFetch`、`Read`、`Write`、`Edit`、`Bash`
- tracker 里的状态值：`Evaluated`、`Applied`、`Interview`、`Offer`、`Rejected`
- 代码片段、命令、路径、字段名

中文模式使用自然的技术中文。正文用中文，行业里约定俗成的技术词保留英文，不强行把 `pipeline`、`deploy`、`embedding` 这类词逐字翻成很重的书面语。

## 参考词汇

| English | 简体中文（本代码库中建议用法） |
|---------|------------------------------|
| Job posting | 职位 / 职位描述 / JD |
| Application | 申请 / 投递 |
| Cover letter | 求职信 / 附信 |
| Resume / CV | 简历 |
| Salary | 薪资 |
| Compensation | 薪酬 / 总包 |
| Skills | 技能 / 能力 |
| Interview | 面试 |
| Hiring manager | 用人经理 |
| Recruiter | 招聘方 / 招聘经理 / Recruiter |
| AI | AI / 人工智能 |
| Requirements | 要求 / 任职要求 |
| Career history | 职业经历 |
| Notice period | 到岗时间 / 离职周期 |
| Probation | 试用期 |
| 13th month salary | 13薪 |
| Annual bonus | 年终奖 |
| Social insurance | 五险一金 |
| Work authorization | 工作身份 / 工签资格 |
| Hybrid work | 混合办公 |
| On-site | 线下到岗 / 坐班 |
| Remote | 远程 |

## 贡献说明

如果你要继续扩展简体中文模式：

1. 先开 Issue，对齐范围和方向
2. 保留 A-F 评估结构、表格、工具指令和命令格式
3. 做意译，不做逐字直译
4. 让中文读起来像真实招聘 / 求职场景里的技术表达
5. 提交前用一条真实中文 JD 做通读检查，确认没有混入繁体或英文翻译腔
