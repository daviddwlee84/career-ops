# career-ops -- 繁體中文模式（`modes/zh-tw/`）

這個目錄提供面向台灣求職市場的繁體中文版本 career-ops 模式。它不是把原文做簡繁轉換，而是依照台灣常見招聘語境、勞動條件與平台習慣重寫。

## 什麼時候用這些模式？

如果符合以下任一條件，就優先使用 `modes/zh-tw/`：

- 你主要投遞的是**繁體中文職缺**，例如 104、Cake、Yourator、公司職涯頁上的中文 JD
- 你的**履歷語言**是繁體中文，或你會在中英文 JD 之間切換
- 你需要**自然的中文技術表達**，而不是翻譯腔
- 你需要處理台灣求職語境裡常見的概念：試用期、勞保 / 健保、勞退、年終、三節、補班、到職日、面議、混合辦公等

如果你多數投遞的是英文職缺，即使公司在台灣，也優先使用根目錄下的預設英文模式 `modes/`。英文模式可以處理台灣市場公司，但不會像這裡一樣預設帶入台灣語境。

## 怎麼啟用？

### 方式 1：每個 session 指定

在 session 一開始直接告訴 Claude：

> "請使用 `modes/zh-tw/` 下的繁體中文模式。"

或更明確一點：

> "請用繁體中文模式評估和申請職缺，優先讀 `modes/zh-tw/_shared.md` 和 `modes/zh-tw/offer.md`。"

Claude 之後就會讀取這個目錄，而不是預設的 `modes/`。

### 方式 2：寫進 `config/profile.yml`

```yaml
language:
  primary: zh-TW
  modes_dir: modes/zh-tw
```

第一次會話時提醒 Claude 注意這個欄位，例如：

> "看一下 `profile.yml`，我已經把 `language.modes_dir` 設成 `modes/zh-tw`。"

> 說明：`language.modes_dir` 目前仍是文件慣例與 agent 使用規則，不是硬編碼切換器。

## 這個目錄包含什麼？

本輪目錄共 6 個檔案：

| 檔案 | 來源 | 用途 |
|------|------|------|
| `README.md` | 新增 | 說明何時使用繁體中文模式、如何啟用、哪些內容已在地化 |
| `_shared.md` | `modes/_shared.md` | 共享脈絡、評分規則、工具規則、台灣求職語境 |
| `offer.md` | `modes/oferta.md` | 單一職缺的完整 A-F 評估 |
| `apply.md` | `modes/apply.md` | 即時申請表填寫助手 |
| `pipeline.md` | `modes/pipeline.md` | URL inbox / 待處理職缺流程 |
| `scan.md` | `modes/scan.md` | 平台掃描與新職缺發現 |

其他 mode 例如 `batch`、`pdf`、`tracker`、`auto-pipeline`、`deep`、`contacto`、`project`、`training`，這一輪仍沿用根目錄下的 EN/ES 原版。

## 哪些內容仍保留英文？

以下內容刻意不硬翻：

- `cv.md`、`pipeline`、`tracker`、`report`、`score`、`archetype`、`proof point`
- 工具名稱：`Playwright`、`WebSearch`、`WebFetch`、`Read`、`Write`、`Edit`、`Bash`
- tracker 裡的狀態值：`Evaluated`、`Applied`、`Interview`、`Offer`、`Rejected`
- 程式碼片段、命令、路徑、欄位名稱

繁體中文模式使用自然的台灣技術職場中文。正文用中文，業界約定俗成的技術詞可保留英文，不需要把 `pipeline`、`deploy`、`embedding` 這類詞硬翻成很重的書面語。

## 參考詞彙

| English | 繁體中文（本程式庫中建議用法） |
|---------|------------------------------|
| Job posting | 職缺 / 職位描述 / JD |
| Application | 投遞 / 申請 |
| Cover letter | 求職信 / 附信 |
| Resume / CV | 履歷 |
| Salary | 薪資 |
| Compensation | 薪酬 / 總包 / package |
| Skills | 技能 / 能力 |
| Interview | 面試 |
| Hiring manager | 用人主管 / Hiring manager |
| Recruiter | 招募方 / Recruiter |
| AI | AI / 人工智慧 |
| Requirements | 條件 / 任職要求 |
| Career history | 職涯經歷 |
| Notice period | 到職日 / 離職交接期 |
| Probation | 試用期 |
| Annual bonus | 年終 |
| Benefits | 福利 |
| Work authorization | 工作資格 / 身分資格 |
| Hybrid work | 混合辦公 |
| On-site | 到班 / 實體到班 |
| Remote | 遠端 |

## 貢獻說明

如果你要繼續擴充繁體中文模式：

1. 先開 Issue，對齊範圍與方向
2. 保留 A-F 評估結構、表格、工具指令與命令格式
3. 做意譯，不做逐字翻譯
4. 讓文字像台灣真實招募 / 求職情境裡會出現的技術中文
5. 送出前用一條真實繁中 JD 通讀，確認沒有混入簡體或翻譯腔
