# 模式：offer -- A-F 完整評估

當候選人貼入一個職缺（文字或 URL）時，始終輸出 6 個區塊：

## 第 0 步 -- Archetype 辨識

把職缺歸類到 6 個 archetype 之一（見 `_shared.md`）。如果是混合型，就寫出最接近的 2 類。這會決定：

- 在 B 區塊優先使用哪些 proof points
- 在 E 區塊如何重寫 summary
- 在 F 區塊準備哪些 STAR 故事

## A 區塊 -- 職缺摘要

輸出表格，包含：

- 偵測到的 archetype
- Domain（platform / agentic / LLMOps / ML / enterprise）
- Function（build / consult / manage / deploy）
- Seniority
- Remote（full / hybrid / onsite）
- Team size（如果 JD 有提）
- 1 句 TL;DR

## B 區塊 -- 與 CV 的匹配度

讀取 `cv.md`。把 JD 裡的每個要求映射到 CV 的確切行號。

**依 archetype 調整優先順序：**

- 如果是 FDE -> 優先強調快速交付和 client-facing 證明
- 如果是 SA -> 優先強調系統設計和 integrations
- 如果是 PM -> 優先強調 product discovery 和 metrics
- 如果是 LLMOps -> 優先強調 evals、observability、pipelines
- 如果是 Agentic -> 優先強調 multi-agent、HITL、orchestration
- 如果是 Transformation -> 優先強調 change management、adoption、scaling

再補一個 **gaps** 小節，對每個 gap 都給緩解策略。每個 gap 都要回答：

1. 它是 hard blocker 還是 nice-to-have？
2. 候選人能否用相鄰經驗證明？
3. 是否有 portfolio project 能覆蓋？
4. 明確的緩解動作是什麼（例如 cover letter 怎麼說、補一個小專案、面試時如何 framing）？

## C 區塊 -- 級別與策略

1. **JD 推斷級別** 對比 **候選人在該 archetype 下的自然級別**
2. **「不說謊地賣 senior」方案**：給具體說法、該突出哪些成果、如何把 founder / owner 經歷說成優勢
3. **「如果被 downlevel」方案**：什麼情況下可接受、如何談 6 個月 review、如何要求明確晉升標準

## D 區塊 -- 薪酬與需求趨勢

用 WebSearch 研究：

- 該職缺的當前薪資（優先：104 薪資情報、比薪水、面試趣；國際職缺可補 Glassdoor、Levels.fyi、Blind）
- 公司在薪酬上的口碑
- 這類職缺的市場需求趨勢

輸出附來源引用的表格。沒有資料就明確寫「暫無可靠資料」，不要猜。

## E 區塊 -- 客製化改寫計畫

| # | 部分 | 目前狀態 | 建議修改 | 原因 |
|---|------|----------|----------|------|
| 1 | Summary | ... | ... | ... |
| ... | ... | ... | ... | ... |

最後給出：

- Top 5 履歷修改點
- Top 5 LinkedIn 個人檔案修改點

## F 區塊 -- 面試準備計畫

準備 6-10 個 STAR+R 故事，映射到 JD 要求（STAR + **Reflection**）：

| # | JD 要求 | STAR+R 故事 | S | T | A | R | Reflection |
|---|---------|-------------|---|---|---|---|------------|

`Reflection` 要寫出復盤：學到了什麼、下次會怎麼做。資深候選人不是只講發生了什麼，而是能提煉方法論。

**Story Bank：** 如果存在 `interview-prep/story-bank.md`，先檢查這些故事是否已在其中。沒有就追加進去，逐步形成可重用的 5-10 個母故事庫。

**依 archetype 調整故事 framing：**

- FDE -> 強調交付速度、client-facing、現場推進
- SA -> 強調架構取捨與系統邊界
- PM -> 強調 discovery、trade-offs、指標判斷
- LLMOps -> 強調指標、evals、production hardening
- Agentic -> 強調 orchestration、error handling、HITL
- Transformation -> 強調 adoption、組織推動、change management

同時補充：

- 1 個推薦展示的 case study（展示哪個專案、怎麼講）
- red-flag 問題與答法（例如「為什麼賣掉公司？」、「你帶過團隊嗎？」、「為什麼想從創業轉成 IC / PM / SA？」）

---

## 評估完成後

**輸出完 A-F 後始終執行：**

### 1. 儲存 report `.md`

把完整評估存到 `reports/{###}-{company-slug}-{YYYY-MM-DD}.md`

- `{###}` = 下一個連續編號（3 位數，左側補零）
- `{company-slug}` = 公司名小寫、空格改連字號
- `{YYYY-MM-DD}` = 當前日期

**report 格式：**

```markdown
# 評估：{公司} -- {職缺}

**日期：** {YYYY-MM-DD}
**Archetype：** {detectado}
**Score：** {X/5}
**URL：** {職缺連結}
**PDF：** {路徑或待生成}

---

## A) 職缺摘要
（完整 A 區塊內容）

## B) 與 CV 的匹配度
（完整 B 區塊內容）

## C) 級別與策略
（完整 C 區塊內容）

## D) 薪酬與需求趨勢
（完整 D 區塊內容）

## E) 客製化改寫計畫
（完整 E 區塊內容）

## F) 面試準備計畫
（完整 F 區塊內容）

## G) Draft Application Answers
（只有 score >= 4.5 時才寫，作為申請表問題草稿）

---

## 擷取出的 Keywords
（列出 15-20 個 JD 關鍵字，用於 ATS 最佳化）
```

### 2. 登記 tracker

**始終**在 `data/applications.md` 中登記：

- 下一個連續編號
- 當前日期
- 公司名
- 職缺名
- Score：match 的 1-5 平均分
- 狀態：`Evaluated`
- PDF：❌（如果 auto-pipeline 同時生成了 PDF，則寫 ✅）
- Report：report `.md` 的相對連結，例如 `[001](reports/001-company-2026-01-01.md)`

**tracker 格式：**

```markdown
| # | 日期 | 公司 | 職缺 | Score | 狀態 | PDF | Report |
```
