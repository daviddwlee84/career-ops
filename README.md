# Career-Ops

**[:gb: English](#what-is-this)** | **[:tw: 繁體中文](#zh-tw)** | **[:es: Español](#es-versión-en-español)**

> AI-powered job search pipeline built on Claude Code. Evaluate offers, generate tailored CVs, scan portals, and track everything -- powered by AI agents.

![Claude Code](https://img.shields.io/badge/Claude_Code-000?style=flat&logo=anthropic&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat&logo=node.js&logoColor=white)
![Go](https://img.shields.io/badge/Go-00ADD8?style=flat&logo=go&logoColor=white)
![Playwright](https://img.shields.io/badge/Playwright-2EAD33?style=flat&logo=playwright&logoColor=white)
![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)

---

<p align="center">
  <img src="docs/demo.gif" alt="Career-Ops Demo" width="800">
</p>

## What Is This

Career-Ops turns Claude Code into a full job search command center. Instead of manually tracking applications in a spreadsheet, you get an AI-powered pipeline that:

- **Evaluates offers** with a structured A-F scoring system (10 weighted dimensions)
- **Generates tailored PDFs** -- ATS-optimized CVs customized per job description
- **Scans portals** automatically (Greenhouse, Ashby, Lever, company pages)
- **Processes in batch** -- evaluate 10+ offers in parallel with sub-agents
- **Tracks everything** in a single source of truth with integrity checks

> **Important: This is NOT a spray-and-pray tool.** Career-ops is a filter -- it helps you find the few offers worth your time out of hundreds. The system strongly recommends against applying to anything scoring below 4.0/5. Your time is valuable, and so is the recruiter's. Always review before submitting.

Career-ops is agentic: Claude Code navigates career pages with Playwright, evaluates fit by reasoning about your CV vs the job description (not keyword matching), and adapts your resume per listing.

> **Heads up: the first evaluations won't be great.** The system doesn't know you yet. Feed it context -- your CV, your career story, your proof points, your preferences, what you're good at, what you want to avoid. The more you nurture it, the better it gets. Think of it as onboarding a new recruiter: the first week they need to learn about you, then they become invaluable.

Built by someone who used it to evaluate 740+ job offers, generate 100+ tailored CVs, and land a Head of Applied AI role. [Read the full case study](https://santifer.io/career-ops-system).

## Features

| Feature | Description |
|---------|-------------|
| **Auto-Pipeline** | Paste a URL, get a full evaluation + PDF + tracker entry |
| **6-Block Evaluation** | Role summary, CV match, level strategy, comp research, personalization, interview prep (STAR+R) |
| **Interview Story Bank** | Accumulates STAR+Reflection stories across evaluations -- 5-10 master stories that answer any behavioral question |
| **Negotiation Scripts** | Salary negotiation frameworks, geographic discount pushback, competing offer leverage |
| **ATS PDF Generation** | Keyword-injected CVs with Space Grotesk + DM Sans design |
| **Portal Scanner** | 45+ companies pre-configured (Anthropic, OpenAI, ElevenLabs, Retool, n8n...) + custom queries across Ashby, Greenhouse, Lever, Wellfound |
| **Batch Processing** | Parallel evaluation with `claude -p` workers |
| **Dashboard TUI** | Terminal UI to browse, filter, and sort your pipeline |
| **Human-in-the-Loop** | AI evaluates and recommends, you decide and act. The system never submits an application -- you always have the final call |
| **Pipeline Integrity** | Automated merge, dedup, status normalization, health checks |

## Quick Start

```bash
# 1. Clone and install
git clone https://github.com/santifer/career-ops.git
cd career-ops && npm install
npx playwright install chromium   # Required for PDF generation

# 2. Check setup
npm run doctor                     # Validates all prerequisites

# 3. Configure
cp config/profile.example.yml config/profile.yml  # Edit with your details
cp templates/portals.example.yml portals.yml       # Customize companies

# 4. Add your CV
# Create cv.md in the project root with your CV in markdown

# 5. Personalize with Claude
claude   # Open Claude Code in this directory

# Then ask Claude to adapt the system to you:
# "Change the archetypes to backend engineering roles"
# "Translate the modes to English"
# "Add these 5 companies to portals.yml"
# "Update my profile with this CV I'm pasting"

# 6. Start using
# Paste a job URL or run /career-ops
```

> **The system is designed to be customized by Claude itself.** Modes, archetypes, scoring weights, negotiation scripts -- just ask Claude to change them. It reads the same files it uses, so it knows exactly what to edit.

See [docs/SETUP.md](docs/SETUP.md) for the full setup guide.

## Usage

Career-ops is a single slash command with multiple modes:

```
/career-ops                → Show all available commands
/career-ops {paste a JD}   → Full auto-pipeline (evaluate + PDF + tracker)
/career-ops scan           → Scan portals for new offers
/career-ops pdf            → Generate ATS-optimized CV
/career-ops batch          → Batch evaluate multiple offers
/career-ops tracker        → View application status
/career-ops apply          → Fill application forms with AI
/career-ops pipeline       → Process pending URLs
/career-ops contacto       → LinkedIn outreach message
/career-ops deep           → Deep company research
/career-ops training       → Evaluate a course/cert
/career-ops project        → Evaluate a portfolio project
```

Or just paste a job URL or description directly -- career-ops auto-detects it and runs the full pipeline.

## How It Works

```
You paste a job URL or description
        │
        ▼
┌──────────────────┐
│  Archetype       │  Classifies: LLMOps / Agentic / PM / SA / FDE / Transformation
│  Detection       │
└────────┬─────────┘
         │
┌────────▼─────────┐
│  A-F Evaluation   │  Match, gaps, comp research, STAR stories
│  (reads cv.md)    │
└────────┬─────────┘
         │
    ┌────┼────┐
    ▼    ▼    ▼
 Report  PDF  Tracker
  .md   .pdf   .tsv
```

## Pre-configured Portals

The scanner comes with **45+ companies** ready to scan and **19 search queries** across major job boards. Copy `templates/portals.example.yml` to `portals.yml` and add your own:

**AI Labs:** Anthropic, OpenAI, Mistral, Cohere, LangChain, Pinecone
**Voice AI:** ElevenLabs, PolyAI, Parloa, Hume AI, Deepgram, Vapi, Bland AI
**AI Platforms:** Retool, Airtable, Vercel, Temporal, Glean, Arize AI
**Contact Center:** Ada, LivePerson, Sierra, Decagon, Talkdesk, Genesys
**Enterprise:** Salesforce, Twilio, Gong, Dialpad
**LLMOps:** Langfuse, Weights & Biases, Lindy, Cognigy, Speechmatics
**Automation:** n8n, Zapier, Make.com
**European:** Factorial, Attio, Tinybird, Clarity AI, Travelperk

**Job boards searched:** Ashby, Greenhouse, Lever, Wellfound, Workable, RemoteFront

## Dashboard TUI

The built-in terminal dashboard lets you browse your pipeline visually:

```bash
cd dashboard
go build -o career-dashboard .
./career-dashboard
```

Features: 6 filter tabs, 4 sort modes, grouped/flat view, lazy-loaded previews, inline status changes.

## Project Structure

```
career-ops/
├── CLAUDE.md                    # Agent instructions
├── cv.md                        # Your CV (create this)
├── article-digest.md            # Your proof points (optional)
├── config/
│   └── profile.example.yml      # Template for your profile
├── modes/                       # 14 skill modes
│   ├── _shared.md               # Shared context (customize this)
│   ├── oferta.md                # Single evaluation
│   ├── pdf.md                   # PDF generation
│   ├── scan.md                  # Portal scanner
│   ├── batch.md                 # Batch processing
│   └── ...
├── templates/
│   ├── cv-template.html         # ATS-optimized CV template
│   ├── portals.example.yml      # Scanner config template
│   └── states.yml               # Canonical statuses
├── batch/
│   ├── batch-prompt.md          # Self-contained worker prompt
│   └── batch-runner.sh          # Orchestrator script
├── dashboard/                   # Go TUI pipeline viewer
├── data/                        # Your tracking data (gitignored)
├── reports/                     # Evaluation reports (gitignored)
├── output/                      # Generated PDFs (gitignored)
├── fonts/                       # Space Grotesk + DM Sans
├── docs/                        # Setup, customization, architecture
└── examples/                    # Sample CV, report, proof points
```

## Tech Stack

![Claude Code](https://img.shields.io/badge/Claude_Code-000?style=flat&logo=anthropic&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat&logo=node.js&logoColor=white)
![Playwright](https://img.shields.io/badge/Playwright-2EAD33?style=flat&logo=playwright&logoColor=white)
![Go](https://img.shields.io/badge/Go-00ADD8?style=flat&logo=go&logoColor=white)
![Bubble Tea](https://img.shields.io/badge/Bubble_Tea-FF75B5?style=flat&logo=go&logoColor=white)

- **Agent**: Claude Code with custom skills and modes
- **PDF**: Playwright/Puppeteer + HTML template
- **Scanner**: Playwright + Greenhouse API + WebSearch
- **Dashboard**: Go + Bubble Tea + Lipgloss (Catppuccin Mocha theme)
- **Data**: Markdown tables + YAML config + TSV batch files

## Also Open Source

- **[cv-santiago](https://github.com/santifer/cv-santiago)** -- The portfolio website (santifer.io) with AI chatbot, LLMOps dashboard, and case studies. If you need a portfolio to showcase alongside your job search, fork it and make it yours.

## About the Author

I'm Santiago -- Head of Applied AI, former founder (built and sold a business that still runs with my name on it). I built career-ops to manage my own job search. It worked: I used it to land my current role.

My portfolio and other open source projects → [santifer.io](https://santifer.io)

☕ [Buy me a coffee](https://buymeacoffee.com/santifer) if career-ops helped your job search.

## Disclaimer

**career-ops is a local, open-source tool — NOT a hosted service.** By using this software, you acknowledge:

1. **You control your data.** Your CV, contact info, and personal data stay on your machine and are sent directly to the AI provider you choose (Anthropic, OpenAI, etc.). We do not collect, store, or have access to any of your data.
2. **You control the AI.** The default prompts instruct the AI not to auto-submit applications, but AI models can behave unpredictably. If you modify the prompts or use different models, you do so at your own risk. **Always review AI-generated content for accuracy before submitting.**
3. **You comply with third-party ToS.** You must use this tool in accordance with the Terms of Service of the career portals you interact with (Greenhouse, Lever, Workday, LinkedIn, etc.). Do not use this tool to spam employers or overwhelm ATS systems.
4. **No guarantees.** Evaluations are recommendations, not truth. AI models may hallucinate skills or experience. The authors are not liable for employment outcomes, rejected applications, account restrictions, or any other consequences.

See [LEGAL_DISCLAIMER.md](LEGAL_DISCLAIMER.md) for full details. This software is provided under the [MIT License](LICENSE) "as is", without warranty of any kind.

## License

MIT

---

<a id="zh-tw"></a>

# :tw: 繁體中文

## 這是什麼

Career-Ops 會把 Claude Code 變成完整的求職指揮中心。你不再需要手動用試算表追蹤申請，而是得到一條由 AI 驅動的 pipeline：

- **評估職缺**，使用結構化的 A-F 評分系統（10 個加權維度）
- **產生客製化 PDF**，依照每份職缺描述調整、並針對 ATS 最佳化的履歷
- **自動掃描求職平台**（Greenhouse、Ashby、Lever、公司職涯頁面）
- **批次處理**，用子代理並行評估 10+ 個職缺
- **集中追蹤一切**，以單一真實來源搭配完整性檢查來管理資料

> **重要：這不是一個海投工具。** Career-ops 是一個篩選器，幫你從數百個職缺中找出真正值得投入時間的少數機會。系統強烈建議不要投遞低於 4.0/5 的職缺。你的時間很寶貴，招募方的時間也是。送出前務必自己審核。

Career-ops 是 agentic 的：Claude Code 會透過 Playwright 瀏覽職涯頁面，根據你的履歷與職缺描述之間的匹配程度進行推理式評估，而不是只做關鍵字比對，並會依每個職缺調整你的履歷。

> **提醒：一開始的評估不會太準。** 系統還不夠了解你。請持續提供它背景資訊，例如你的履歷、職涯故事、成果證據、偏好、擅長的事，以及你想避免的方向。你餵給它的內容越多，它就會越準。把它想成是在幫一位新招募者做 onboarding：第一週他需要先認識你，之後就會變得非常有價值。

這套系統由一位親自用它評估超過 740 個職缺、產出 100+ 份客製履歷，並拿下 Head of Applied AI 職位的人所打造。[閱讀完整案例研究](https://santifer.io/career-ops-system)。

## 功能特色

| 功能 | 說明 |
|------|------|
| **自動化 Pipeline** | 貼上一個 URL，即可得到完整評估 + PDF + tracker 紀錄 |
| **6 區塊評估** | 職缺摘要、履歷匹配、職級策略、薪資研究、個人化、面試準備（STAR+R） |
| **面試故事庫** | 在每次評估中累積 STAR+Reflection 故事，沉澱成 5-10 個可回答各種行為題的核心故事 |
| **談薪腳本** | 提供薪資談判框架、地區折扣反駁、競爭 offer 槓桿策略 |
| **ATS PDF 生成** | 使用 Space Grotesk + DM Sans 設計的關鍵字優化履歷 |
| **Portal 掃描器** | 預設支援 45+ 間公司（Anthropic、OpenAI、ElevenLabs、Retool、n8n...），並可跨 Ashby、Greenhouse、Lever、Wellfound 自訂查詢 |
| **批次處理** | 透過 `claude -p` workers 並行評估 |
| **Dashboard TUI** | 在終端機中瀏覽、篩選與排序整條求職 pipeline |
| **Human-in-the-Loop** | AI 負責評估與提出建議，你來做決策與行動。系統不會自動送出申請，最後決定權永遠在你手上 |
| **Pipeline 完整性** | 自動合併、去重、狀態正規化與健康檢查 |

## 快速開始

```bash
# 1. 複製專案並安裝
git clone https://github.com/santifer/career-ops.git
cd career-ops && npm install
npx playwright install chromium   # PDF 生成功能必需

# 2. 檢查環境
npm run doctor                    # 驗證所有先決條件

# 3. 設定
cp config/profile.example.yml config/profile.yml  # 編輯成你的個人資料
cp templates/portals.example.yml portals.yml      # 自訂公司清單

# 4. 加入你的履歷
# 在專案根目錄建立 cv.md，內容為 Markdown 格式履歷

# 5. 用 Claude 個人化系統
claude   # 在此目錄開啟 Claude Code

# 然後請 Claude 根據你自己來調整系統：
# "把 archetypes 改成後端工程師職缺"
# "把 modes 翻成英文"
# "把這 5 間公司加到 portals.yml"
# "用我貼上的這份 CV 更新我的 profile"

# 6. 開始使用
# 貼上一個職缺 URL，或執行 /career-ops
```

> **這套系統本來就設計成可由 Claude 自己客製化。** Modes、archetypes、評分權重、談薪腳本，只要直接叫 Claude 修改即可。它讀的就是它自己在用的那些檔案，所以它知道該改哪裡。

完整設定指南請見 [docs/SETUP.md](docs/SETUP.md)。

## 使用方式

Career-ops 是一個單一 slash command，但支援多種模式：

```
/career-ops                → 顯示所有可用指令
/career-ops {貼上一段 JD}  → 完整自動 pipeline（評估 + PDF + tracker）
/career-ops scan           → 掃描平台上的新職缺
/career-ops pdf            → 產生 ATS 最佳化履歷
/career-ops batch          → 批次評估多個職缺
/career-ops tracker        → 查看申請狀態
/career-ops apply          → 用 AI 填寫申請表單
/career-ops pipeline       → 處理待處理 URL
/career-ops contacto       → LinkedIn 外聯訊息
/career-ops deep           → 深度公司研究
/career-ops training       → 評估課程或證照
/career-ops project        → 評估作品集專案
```

或者你也可以直接貼上一個職缺 URL 或描述，career-ops 會自動辨識並執行完整 pipeline。

## 運作方式

```
你貼上一個職缺 URL 或描述
        │
        ▼
┌──────────────────┐
│  Archetype       │  分類：LLMOps / Agentic / PM / SA / FDE / Transformation
│  Detection       │
└────────┬─────────┘
         │
┌────────▼─────────┐
│  A-F Evaluation  │  匹配度、缺口、薪資研究、STAR 故事
│  (reads cv.md)   │
└────────┬─────────┘
         │
    ┌────┼────┐
    ▼    ▼    ▼
  報告   PDF  Tracker
  .md   .pdf   .tsv
```

## 預設支援的求職平台

掃描器內建 **45+ 間公司** 與 **19 組搜尋查詢**，涵蓋主要求職平台。複製 `templates/portals.example.yml` 成 `portals.yml` 後，即可再加入你自己的清單：

**AI Labs:** Anthropic, OpenAI, Mistral, Cohere, LangChain, Pinecone
**Voice AI:** ElevenLabs, PolyAI, Parloa, Hume AI, Deepgram, Vapi, Bland AI
**AI Platforms:** Retool, Airtable, Vercel, Temporal, Glean, Arize AI
**Contact Center:** Ada, LivePerson, Sierra, Decagon, Talkdesk, Genesys
**Enterprise:** Salesforce, Twilio, Gong, Dialpad
**LLMOps:** Langfuse, Weights & Biases, Lindy, Cognigy, Speechmatics
**Automation:** n8n, Zapier, Make.com
**European:** Factorial, Attio, Tinybird, Clarity AI, Travelperk

**已搜尋的求職平台：** Ashby、Greenhouse、Lever、Wellfound、Workable、RemoteFront

## Dashboard TUI

內建的終端機 dashboard 讓你能以視覺化方式瀏覽整條 pipeline：

```bash
cd dashboard
go build -o career-dashboard .
./career-dashboard
```

功能包含：6 個篩選分頁、4 種排序模式、群組/平面檢視、lazy-loaded 預覽，以及行內狀態變更。

## 專案結構

```
career-ops/
├── CLAUDE.md                    # Agent 指令
├── cv.md                        # 你的履歷（需自行建立）
├── article-digest.md            # 你的成果證據（可選）
├── config/
│   └── profile.example.yml      # 個人資料範本
├── modes/                       # 14 個技能模式
│   ├── _shared.md               # 共用上下文（請客製化）
│   ├── oferta.md                # 單一職缺評估
│   ├── pdf.md                   # PDF 生成
│   ├── scan.md                  # 平台掃描器
│   ├── batch.md                 # 批次處理
│   └── ...
├── templates/
│   ├── cv-template.html         # ATS 最佳化履歷範本
│   ├── portals.example.yml      # 掃描器設定範本
│   └── states.yml               # 標準化狀態
├── batch/
│   ├── batch-prompt.md          # 自包含 worker prompt
│   └── batch-runner.sh          # 協調腳本
├── dashboard/                   # Go TUI pipeline 檢視器
├── data/                        # 你的追蹤資料（gitignored）
├── reports/                     # 評估報告（gitignored）
├── output/                      # 產生的 PDF（gitignored）
├── fonts/                       # Space Grotesk + DM Sans
├── docs/                        # 設定、客製化與架構文件
└── examples/                    # 範例履歷、報告與成果證據
```

## 技術棧

![Claude Code](https://img.shields.io/badge/Claude_Code-000?style=flat&logo=anthropic&logoColor=white)
![Node.js](https://img.shields.io/badge/Node.js-339933?style=flat&logo=node.js&logoColor=white)
![Playwright](https://img.shields.io/badge/Playwright-2EAD33?style=flat&logo=playwright&logoColor=white)
![Go](https://img.shields.io/badge/Go-00ADD8?style=flat&logo=go&logoColor=white)
![Bubble Tea](https://img.shields.io/badge/Bubble_Tea-FF75B5?style=flat&logo=go&logoColor=white)

- **Agent**：Claude Code，搭配自訂 skills 與 modes
- **PDF**：Playwright/Puppeteer + HTML template
- **Scanner**：Playwright + Greenhouse API + WebSearch
- **Dashboard**：Go + Bubble Tea + Lipgloss（Catppuccin Mocha 主題）
- **Data**：Markdown tables + YAML config + TSV batch files

## 其他開源專案

- **[cv-santiago](https://github.com/santifer/cv-santiago)** -- 這是作者的作品集網站（santifer.io），包含 AI chatbot、LLMOps dashboard 與案例研究。如果你也需要一個能搭配求職使用的作品集網站，可以 fork 後改成自己的版本。

## 作者介紹

我是 Santiago，現任 Head of Applied AI，也曾創業並出售過一間至今仍以我名字運作的公司。我打造 career-ops 來管理我自己的求職流程，而它確實有效：我靠它拿下了目前這份工作。

我的作品集與其他開源專案 → [santifer.io](https://santifer.io)

☕ 如果 career-ops 對你的求職有幫助，歡迎 [請我喝杯咖啡](https://buymeacoffee.com/santifer)。

## 免責聲明

**career-ops 是一個本地端的開源工具，不是託管式服務。** 使用本軟體即表示你理解並同意：

1. **你的資料由你掌控。** 你的履歷、聯絡資訊與個人資料都保留在你的電腦上，並直接傳送給你選擇的 AI 供應商（Anthropic、OpenAI 等）。我們不會收集、儲存，也無法存取你的資料。
2. **AI 由你掌控。** 預設 prompts 會指示 AI 不要自動送出申請，但 AI 模型仍可能出現不可預期的行為。如果你修改 prompts 或改用其他模型，相關風險由你自行承擔。**送出前務必審核 AI 生成內容的正確性。**
3. **你必須遵守第三方服務條款。** 你需要依照所互動求職平台的服務條款使用本工具（例如 Greenhouse、Lever、Workday、LinkedIn 等）。請勿使用本工具騷擾雇主，或對 ATS 系統造成壓力。
4. **不提供任何保證。** 這些評估是建議，不是真理。AI 模型可能會虛構技能或經歷。作者不對求職結果、被拒申請、帳號限制或任何其他後果負責。

詳情請參閱 [LEGAL_DISCLAIMER.md](LEGAL_DISCLAIMER.md)。本軟體依 [MIT License](LICENSE) 以 "as is" 方式提供，不附帶任何形式的保證。

## 授權

MIT

---

# :es: Version en Español

## Que es esto

Career-Ops convierte Claude Code en un centro de mando de busqueda de empleo. En vez de trackear aplicaciones en un spreadsheet, tienes un pipeline AI que:

- **Evalua ofertas** con scoring estructurado A-F (10 dimensiones ponderadas)
- **Genera PDFs personalizados** -- CVs ATS-optimizados por oferta
- **Escanea portales** automaticamente (Greenhouse, Ashby, Lever, webs de empresas)
- **Procesa en batch** -- evalua 10+ ofertas en paralelo con sub-agentes
- **Trackea todo** en una fuente de verdad unica con checks de integridad

> **Importante: Esto NO es para spamear empresas.** Career-ops es un filtro -- te ayuda a encontrar las pocas ofertas que merecen tu tiempo entre cientos. El sistema recomienda encarecidamente no aplicar a nada por debajo de 4.0/5. Tu tiempo es valioso, y el del recruiter tambien. Siempre revisa antes de enviar.

> **Aviso: las primeras evaluaciones no seran buenas.** El sistema no te conoce todavia. Dale contexto -- tu CV, tu historia profesional, tus proof points, tus preferencias, en que eres bueno, que quieres evitar. Cuanto mas lo nutras, mejor filtra. Piensa en ello como hacer onboarding a un recruiter nuevo: la primera semana necesita conocerte, luego se vuelve invaluable.

Construido por alguien que lo uso para evaluar 740+ ofertas, generar 100+ CVs personalizados, y conseguir un rol de Head of Applied AI. [Lee el case study completo](https://santifer.io/career-ops).

## Inicio rapido

```bash
# 1. Clonar
git clone https://github.com/santifer/career-ops.git
cd career-ops && npm install

# 2. Verificar setup
npm run doctor                     # Valida todos los prerequisitos

# 3. Configurar
cp config/profile.example.yml config/profile.yml  # Editar con tus datos
cp templates/portals.example.yml portals.yml       # Personalizar empresas

# 4. Añadir tu CV
# Crear cv.md en la raiz del proyecto con tu CV en markdown

# 5. Personalizar con Claude
claude   # Abrir Claude Code en este directorio

# Pidele a Claude que adapte el sistema a ti:
# "Cambia los arquetipos a roles de backend"
# "Traduce los modes a ingles"
# "Añade estas empresas a portals.yml"
# "Actualiza mi perfil con este CV que te pego"

# 6. Usar
# Pega una URL de oferta o ejecuta /career-ops
```

> **El sistema esta diseñado para que Claude lo personalice.** Modes, arquetipos, scoring, scripts de negociacion -- solo pidelo. Claude lee los mismos archivos que usa, asi que sabe exactamente que editar.

Guia completa en [docs/SETUP.md](docs/SETUP.md).

## Portales incluidos

El scanner viene con **45+ empresas** pre-configuradas y **19 queries** en los principales portales de empleo. Copia `templates/portals.example.yml` a `portals.yml` y añade las tuyas:

**AI Labs:** Anthropic, OpenAI, Mistral, Cohere, LangChain, Pinecone
**Voice AI:** ElevenLabs, PolyAI, Parloa, Hume AI, Deepgram, Vapi, Bland AI
**Plataformas AI:** Retool, Airtable, Vercel, Temporal, Glean, Arize AI
**Contact Center:** Ada, LivePerson, Sierra, Decagon, Talkdesk, Genesys
**Enterprise:** Salesforce, Twilio, Gong, Dialpad
**LLMOps:** Langfuse, Weights & Biases, Lindy, Cognigy, Speechmatics
**Automatizacion:** n8n, Zapier, Make.com
**Europa:** Factorial, Attio, Tinybird, Clarity AI, Travelperk

**Portales de empleo:** Ashby, Greenhouse, Lever, Wellfound, Workable, RemoteFront

## Uso

Career-ops es un unico slash command con multiples modos:

```
/career-ops                → Mostrar todos los comandos
/career-ops {pega un JD}   → Pipeline completo (evaluar + PDF + tracker)
/career-ops scan           → Escanear portales
/career-ops pdf            → Generar CV ATS-optimizado
/career-ops batch          → Evaluar ofertas en batch
/career-ops tracker        → Ver estado de aplicaciones
/career-ops apply          → Rellenar formularios con IA
/career-ops pipeline       → Procesar URLs pendientes
/career-ops contacto       → Mensaje LinkedIn outreach
/career-ops deep           → Research profundo de empresa
```

O simplemente pega una URL o descripcion de oferta -- career-ops la detecta y ejecuta el pipeline completo.

## Tambien Open Source

- **[cv-santiago](https://github.com/santifer/cv-santiago)** -- El portfolio (santifer.io) con chatbot IA, dashboard LLMOps y case studies. Si necesitas un portfolio para acompañar tu busqueda de empleo, echale un vistazo.

## Documentacion

- [SETUP.md](docs/SETUP.md) -- Guia de instalacion
- [CUSTOMIZATION.md](docs/CUSTOMIZATION.md) -- Como personalizar
- [ARCHITECTURE.md](docs/ARCHITECTURE.md) -- Como funciona el sistema

☕ [Invitame a un cafe](https://buymeacoffee.com/santifer) si career-ops te ayudo en tu busqueda.

## Aviso legal

**career-ops es una herramienta local y open source — NO un servicio alojado.** Al usar este software, aceptas que:

1. **Tu controlas tus datos.** Tu CV, datos de contacto e informacion personal se quedan en tu maquina y se envian directamente al proveedor de IA que elijas (Anthropic, OpenAI, etc.). No recopilamos, almacenamos ni tenemos acceso a tus datos.
2. **Tu controlas la IA.** Los prompts por defecto instruyen a la IA a no enviar aplicaciones automaticamente, pero los modelos pueden comportarse de forma impredecible. Si modificas los prompts o usas otros modelos, lo haces bajo tu responsabilidad. **Revisa siempre el contenido generado antes de enviarlo.**
3. **Tu cumples con los terminos de terceros.** Debes usar esta herramienta de acuerdo con los Terminos de Servicio de los portales de empleo (Greenhouse, Lever, Workday, LinkedIn, etc.). No uses esta herramienta para spamear empresas.
4. **Sin garantias.** Las evaluaciones son recomendaciones, no verdad absoluta. Los modelos pueden inventar habilidades o experiencia. Los autores no son responsables de resultados laborales, candidaturas rechazadas, restricciones de cuenta ni ninguna otra consecuencia.

Ver [LEGAL_DISCLAIMER.md](LEGAL_DISCLAIMER.md) para mas detalles. Este software se proporciona bajo la [Licencia MIT](LICENSE) "tal cual", sin garantia de ningun tipo.

## Let's Connect

[![Website](https://img.shields.io/badge/santifer.io-000?style=for-the-badge&logo=safari&logoColor=white)](https://santifer.io)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0A66C2?style=for-the-badge&logo=linkedin&logoColor=white)](https://linkedin.com/in/santifer)
[![Email](https://img.shields.io/badge/Email-EA4335?style=for-the-badge&logo=gmail&logoColor=white)](mailto:hola@santifer.io)
[![Buy Me a Coffee](https://img.shields.io/badge/Buy_Me_a_Coffee-FFDD00?style=for-the-badge&logo=buy-me-a-coffee&logoColor=black)](https://buymeacoffee.com/santifer)
