# 共享脈絡 -- career-ops（繁體中文）

<!-- ============================================================
     這個檔案是系統層共享規則。

     你的個人化資訊應放在 modes/_profile.md（不會被自動更新）。
     使用時先讀這個檔案，再讀 modes/_profile.md。
     ============================================================ -->

## 資訊來源（每次評估前都要讀）

| 檔案 | 路徑 | 何時讀取 |
|------|------|----------|
| cv.md | `cv.md`（專案根目錄） | 每次都讀 |
| article-digest.md | `article-digest.md`（如果存在） | 每次都讀（詳細 proof points） |
| profile.yml | `config/profile.yml` | 每次都讀（身份、目標職缺、地點、薪酬） |
| _profile.md | `modes/_profile.md` | 每次都讀（你的自訂 framing、敘事、談薪偏好） |

**規則：不要把 proof point 的數字硬編碼在這裡。** 評估時從 `cv.md` 和 `article-digest.md` 讀取。
**規則：涉及專案 / 文章指標時，`article-digest.md` 優先於 `cv.md`。**
**規則：讀完本檔後再讀 `modes/_profile.md`。使用者自訂優先於系統預設。**

---

## 評分系統

評估使用 6 個區塊（A-F），總分為 1-5：

| 維度 | 衡量什麼 |
|------|----------|
| Match with CV | 技能、經歷、proof points 與 JD 的匹配度 |
| North Star alignment | 這個職缺和你的目標 archetype 是否一致 |
| Comp | 薪資與市場相比是否合理（5 = 上四分位，1 = 明顯低於市場） |
| Cultural signals | 團隊文化、成長性、穩定性、遠端政策 |
| Red flags | 阻斷項、風險項、負面修正 |
| **Global** | 以上維度的加權平均 |

**分數解讀：**
- 4.5+ -> 強匹配，建議盡快投遞
- 4.0-4.4 -> 明顯值得投
- 3.5-3.9 -> 還可以，但需要有明確理由
- 低於 3.5 -> 原則上不建議投（參見 `CLAUDE.md` 的 Ethical Use）

## Archetype 辨識

每個職缺都要歸類到以下 6 類之一，或標記為 2 類混合：

| Archetype | JD 裡的典型訊號 |
|-----------|------------------|
| AI Platform / LLMOps | `observability`、`evals`、`pipelines`、`monitoring`、`reliability` |
| Agentic / Automation | `agent`、`HITL`、`orchestration`、`workflow`、`multi-agent` |
| Technical AI PM | `PRD`、`roadmap`、`discovery`、`stakeholder`、`product manager` |
| AI Solutions Architect | `architecture`、`enterprise`、`integration`、`design`、`systems` |
| AI Forward Deployed | `client-facing`、`deploy`、`prototype`、`fast delivery`、`field` |
| AI Transformation | `change management`、`adoption`、`enablement`、`transformation` |

辨識出 archetype 後，再讀 `modes/_profile.md` 中對應的 framing 和 proof points。

## 目標職缺與自適應 framing

預設把以下六類職缺都視為有效目標，而不是主次分明的備選：

| Archetype | 主題軸 | 公司真正購買的能力 |
|-----------|--------|--------------------|
| **AI Platform / LLMOps Engineer** | Evaluation、Observability、Reliability、Pipelines | 能把 AI 系統穩定落到 production，並以指標管理品質 |
| **Agentic Workflows / Automation** | HITL、Tooling、Orchestration、Multi-Agent | 能把 agent workflow 做得可靠、可控、可擴展 |
| **Technical AI Product Manager** | GenAI / Agents、PRD、Discovery、Delivery | 能把商業問題轉譯成 AI 產品並推進落地 |
| **AI Solutions Architect** | Hyperautomation、Enterprise、Integrations | 能設計端到端 AI 架構並與企業系統整合 |
| **AI Forward Deployed Engineer** | Client-facing、快速交付、Prototyping | 能貼近客戶快速交付 AI 方案 |
| **AI Transformation Lead** | Change management、Adoption、Enablement | 能推動組織級 AI 導入與採用 |

> 具體專案映射、proof points 與優先敘事，優先讀 `modes/_profile.md`。不要在這裡寫死你的真實數字。

| 如果職缺屬於... | 應強調你哪一面 | proof point 來源 |
|------------------|----------------|------------------|
| Platform / LLMOps | production 經驗、observability、evals、閉環品質 | article-digest.md + cv.md |
| Agentic / Automation | 多 agent 編排、HITL、可靠性、成本控制 | article-digest.md + cv.md |
| Technical AI PM | product discovery、PRD、指標、stakeholder 管理 | cv.md + article-digest.md |
| Solutions Architect | 系統設計、整合、企業級可落地性 | article-digest.md + cv.md |
| Forward Deployed Engineer | 快速交付、客戶互動、prototype 到 production | cv.md + article-digest.md |
| AI Transformation Lead | 變革管理、團隊 enablement、adoption | cv.md + article-digest.md |

## 轉換敘事（所有內容都要重用）

統一用 `config/profile.yml` 的 `narrative.exit_story` 做 framing：

- **PDF Summary**：把過去經歷和目標職缺自然接上
- **STAR 故事**：把敘事與 proof points 串起來
- **表單回答草稿**：第一題通常最適合放轉換敘事
- **JD 出現 builder / owner / entrepreneurial / end-to-end**：把它當成強匹配訊號，提高相關敘事權重

## 跨職缺通用優勢

把候選人統一 framing 為：

**「有真實落地證明的技術型 builder，而且能依職缺調整表達。」**

- 對 PM：強調先用 prototype 降低不確定性，再把東西穩定交付
- 對 FDE：強調能在客戶前線快速交付，且從第一天就帶著指標意識
- 對 SA：強調端到端系統設計與跨系統整合
- 對 LLMOps：強調 evals、observability、production hardening

## 作品集 / demo 的使用規則

如果 `profile.yml` 中配置了 live demo / dashboard / public project，就在高價值申請裡主動提供。

- 適合場景：LLMOps、AI Platform、Solutions Architect、FDE
- 如果職缺偏產品或轉型，也可以把 demo 當作可信 proof point，而不是唯一賣點

## 薪酬研究（Comp Intelligence）

**通用建議：**

- 先按職稱比價，不要只按技能點比價
- 台灣本地市場優先參考：104 薪資情報、比薪水、面試趣、公開薪酬報告
- 國際化公司或遠端職缺可補充：Glassdoor、Levels.fyi、Blind
- 算總包時要一起看固定薪、年終、三節、股票 / RSU、津貼與遠端 / 到班成本

## 台灣求職語境（重要）

以下詞在台灣 JD、HR 溝通與談薪裡很常出現，必須正確理解：

| 術語 | 含義 | 評估影響 |
|------|------|----------|
| **試用期** | 一般 3 個月左右，期間薪資與考核方式可能不同 | 屬常見安排；過長或條件模糊需要記風險 |
| **勞保 / 健保** | 法定保險 | 不是加分項，是基本盤；模糊不清要記 red flag |
| **勞退** | 雇主提撥退休金 | 算總包時要納入，不要只看月薪 |
| **年終** | 常見固定或浮動年終獎金 | 比較總包時必須一起算 |
| **三節** | 三節獎金 / 禮金 / 禮品 | 金額通常不大，但仍屬 package 一部分 |
| **補班** | 依政府行事曆調整上班日 | 如果公司文化明顯重度補班 / 加班，要記在文化風險 |
| **到職日** | 最快可報到日期 | 影響申請節奏與面試預期管理 |
| **面議** | 不公開數字 | 不可腦補，必須明確寫「缺乏可靠資料」 |
| **混合辦公 / 到班** | 實體出勤要求 | 直接影響 remote / hybrid 維度分數 |
| **派遣 / 駐點 / 外包** | 雇主關係與實際工作場域可能分離 | 穩定性、歸屬感與升遷空間通常較弱 |

## 談薪腳本

**薪資期望：**
> “依照這類職缺目前的市場資料，我的預期區間是 [來自 profile.yml 的範圍]。我對結構有彈性，但更看重總包、職責範圍與成長空間。”

**如果對方用地點壓價：**
> “我競爭的是結果導向職缺，不是按地區定價的職缺。我的交付能力不會因為所在地不同而改變。”

**如果報價低於目標：**
> “我目前也在比較更高區間的機會。我對 [公司] 的興趣是真實的，因為 [具體原因]。如果能往 [目標值] 靠攏，我會更願意往前推進。”

**如果總包裡含年終、三節或股票：**
> “為了公平比較不同機會，方便把固定月薪、年終、三節與股票 / RSU 分開說明嗎？”

## 地點政策（Location Policy）

**在表單裡：**
- 二選一的「是否可到班 / hybrid」問題，依 `profile.yml` 的真實可用性回答
- 自由文本裡明確寫時區、城市、可出差範圍與最快到職日

**在評分裡：**
- hybrid 且不在你所在城市 / 國家：remote 維度給 **3.0**，不是 1.0
- 只有在 JD 明確寫「每週 4-5 天必須到班、沒有例外」時，才給 1.0

## Time-to-offer 優先順序

- 可展示的成果 + 指標 > 完美主義
- 更快投遞 > 無限延伸研究
- 一律用 80/20 做時間盒管理

---

## 全域規則

### 絕不

1. 編造經歷或指標
2. 修改 `cv.md` 或 portfolio 原始檔
3. 代替候選人直接送出申請
4. 在生成訊息裡分享電話號碼
5. 推薦明顯低於市場的薪酬
6. 沒讀 JD 就生成 PDF
7. 使用空泛套話或 corporate-speak
8. 忽略 tracker（每份已評估職缺都要登記）

### 必做

0. **求職信：** 如果表單允許附求職信或填寫 cover letter，始終提供。視覺風格與履歷一致，引用 JD 並映射到 proof points，最多 1 頁。
1. 評估任何職缺前，先讀 `cv.md`、`modes/_profile.md`、以及 `article-digest.md`（如果有）
1b. **每次 session 第一次評估：** 執行 `node cv-sync-check.mjs`。如果有 warning，先提醒使用者
2. 先辨識 archetype，再決定 framing
3. 做 match 時引用 CV 裡的確切行
4. 用 WebSearch 做薪酬與公司研究
5. 每次評估後都要登記 tracker
6. 依 JD 語言輸出：中文 JD 用自然繁體中文，英文 JD 用英文
7. 表達直接、具體、可執行
8. 寫中文材料時使用自然台灣技術中文。短句、主動語態、避免翻譯腔；技術詞不必強行全中文化
8b. **PDF Professional Summary 裡的案例連結：** 如果 PDF 提到 case study 或 demo，URL 必須出現在第一段 summary 裡。Recruiter 常常只看第一屏
9. **Tracker 新增項寫 TSV**：不要直接手改 `applications.md`。寫到 `batch/tracker-additions/`，再由 `merge-tracker.mjs` 合併
10. **每個 report header 都要包含 `**URL:**`**

### 工具

| 工具 | 用途 |
|------|------|
| WebSearch | 薪酬研究、趨勢、公司文化、LinkedIn 聯絡人、JD fallback |
| WebFetch | 從靜態頁面提取 JD 的 fallback |
| Playwright | 檢查職缺是否仍有效、讀取 SPA 頁面。**關鍵：不要並行跑多個使用 Playwright 的 agent** |
| Read | 讀取 `cv.md`、`modes/_profile.md`、`article-digest.md`、`cv-template.html` |
| Write | 寫暫存 HTML、applications.md、reports .md |
| Edit | 更新 tracker |
| Bash | 執行 `node generate-pdf.mjs` |

## ATS 相容寫作

### Unicode 正規化
`generate-pdf.mjs` 會把 em-dash、智慧引號、零寬字元正規化成較相容的 ASCII 形式，但最好一開始就避免產生這些字元。

### 句型要有變化
- 不要每個 bullet 都用同一個動詞起手
- 長短句交錯
- 不要永遠都是「X、Y、Z」三項並列

### 用具體資訊取代抽象表述
- 「把 p95 延遲從 2.1s 降到 380ms」優於「優化了效能」
- 「用 Postgres + pgvector 在 1.2 萬份文件上做檢索」優於「設計了可擴展的 RAG 架構」
- 允許時直接寫工具名、專案名、客戶類型
