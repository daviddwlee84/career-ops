# Mode: offer — Full A-F Evaluation

When the candidate pastes an offer (text or URL), ALWAYS deliver all 6 blocks:

## Step 0 — Archetype Detection

Classify the offer into one of the 6 archetypes (see `_shared.md`). If it is hybrid, name the 2 closest. This determines:
- Which proof points to prioritize in Block B
- How to rewrite the summary in Block E
- Which STAR stories to prepare in Block F

## Block A — Role Summary

Table with:
- Detected archetype
- Domain (platform/agentic/LLMOps/ML/enterprise)
- Function (build/consult/manage/deploy)
- Seniority
- Remote (full/hybrid/onsite)
- Team size (if mentioned)
- TL;DR in one sentence

## Block B — CV Match

Read `cv.md`. Build a table mapping each JD requirement to exact lines in the CV.

**Tailored to the archetype:**
- If FDE → prioritize proof points for fast delivery and client-facing work
- If SA → prioritize system design and integrations
- If PM → prioritize product discovery and metrics
- If LLMOps → prioritize evals, observability, pipelines
- If Agentic → prioritize multi-agent, HITL, orchestration
- If Transformation → prioritize change management, adoption, scaling

A **gaps** section with a mitigation strategy for each. For each gap:
1. Is it a hard blocker or a nice-to-have?
2. Can the candidate show adjacent experience?
3. Is there a portfolio project that covers this gap?
4. Concrete mitigation plan (line for a cover letter, quick project, etc.)

## Block C — Level & Strategy

1. **Level implied** in the JD vs **the candidate’s natural level for that archetype**
2. **Plan to “sell senior without lying”**: archetype-specific phrases, concrete wins to lead with, how to frame founder experience as an asset
3. **Plan if they try to downlevel you**: accept if total comp is fair, negotiate a 6‑month performance check-in, clear promotion criteria

## Block D — Comp & Demand

Use WebSearch for:
- Current compensation for the role (Glassdoor, Levels.fyi, Blind)
- The company’s pay reputation (cash + equity where relevant)
- Demand trend for the role (hiring volume, market heat)

Table with figures and cited sources. If data is missing, say so—do not invent.

## Block E — Personalization Plan

| # | Section | Current state | Proposed change | Why |
|---|---------|---------------|-----------------|-----|
| 1 | Summary | ... | ... | ... |
| ... | ... | ... | ... | ... |

Top 5 CV changes + Top 5 LinkedIn changes to maximize match.

## Block F — Interview Plan

6–10 STAR+R stories mapped to JD requirements (STAR + **Reflection**):

| # | JD requirement | STAR+R story | S | T | A | R | Reflection |
|---|----------------|----------------|---|---|---|---|------------|

The **Reflection** column captures what was learned or what you would do differently. This signals seniority — junior candidates describe what happened; senior candidates extract lessons.

**Story Bank:** If `interview-prep/story-bank.md` exists, check whether any of these stories are already there. If not, append new ones. Over time this builds a reusable bank of 5–10 master stories you can adapt to any interview question.

**Selected and framed by archetype:**
- FDE → emphasize delivery speed and client-facing impact
- SA → emphasize architecture decisions
- PM → emphasize discovery and trade-offs
- LLMOps → emphasize metrics, evals, production hardening
- Agentic → emphasize orchestration, error handling, HITL
- Transformation → emphasize adoption and organizational change

Also include:
- 1 recommended case study (which project to present and how)
- Red-flag questions and how to answer them (e.g. “Why did you sell your company?”, “Do you have people management experience?”)

---

## Post-evaluation

**ALWAYS** after generating blocks A–F:

### 1. Save report .md

Save the full evaluation to `reports/{###}-{company-slug}-{YYYY-MM-DD}.md`.

- `{###}` = next sequential number (3 digits, zero-padded)
- `{company-slug}` = company name in lowercase, no spaces (use hyphens)
- `{YYYY-MM-DD}` = today’s date

**Report format:**

```markdown
# Evaluation: {Company} — {Role}

**Date:** {YYYY-MM-DD}
**Archetype:** {detected}
**Score:** {X/5}
**PDF:** {path or pending}

---

## A) Role Summary
(full content of Block A)

## B) CV Match
(full content of Block B)

## C) Level & Strategy
(full content of Block C)

## D) Comp & Demand
(full content of Block D)

## E) Personalization Plan
(full content of Block E)

## F) Interview Plan
(full content of Block F)

## G) Draft Application Answers
(only if score >= 4.5 — draft answers for the application form)

---

## Extracted keywords
(list of 15–20 keywords from the JD for ATS optimization)
```

### 2. Register in tracker

**ALWAYS** register in `data/applications.md`:
- Next sequential number
- Today’s date
- Company
- Role
- Score: average match (1–5)
- Status: `Evaluated`
- PDF: ❌ (or ✅ if auto-pipeline generated a PDF)
- Report: relative link to the .md report (e.g. `[001](reports/001-company-2026-01-01.md)`)

**Tracker format:**

```markdown
| # | Date | Company | Role | Score | Status | PDF | Report |
```
