# Mode: auto-pipeline — Full automatic pipeline

When the user pastes a JD (text or URL) without an explicit sub-command, run the entire pipeline in sequence:

## Step 0 — Extract the JD

If the input is a **URL** (not pasted JD text), use this strategy to extract content:

**Priority order:**

1. **Playwright (preferred):** Most job boards (Lever, Ashby, Greenhouse, Workday) are SPAs. Use `browser_navigate` + `browser_snapshot` to render and read the JD.
2. **WebFetch (fallback):** For static pages (ZipRecruiter, WeLoveProduct, company career pages).
3. **WebSearch (last resort):** Search role title + company on secondary boards that index the JD as static HTML.

**If nothing works:** Ask the candidate to paste the JD manually or share a screenshot.

**If the input is JD text** (not a URL): use it directly; no fetch needed.

## Step 1 — A–F evaluation

Run exactly like `offer` mode (read `modes/offer.md` for all blocks A–F).

## Step 2 — Save report `.md`

Save the full evaluation to `reports/{###}-{company-slug}-{YYYY-MM-DD}.md` (see format in `modes/offer.md`).

## Step 3 — Generate PDF

Run the full `pdf` pipeline (read `modes/pdf.md`).

## Step 4 — Draft application answers (only if score >= 4.5)

If the final score is >= 4.5, generate draft answers for the application form:

1. **Extract form questions**: Use Playwright to open the form and snapshot. If you cannot extract them, use the generic questions below.
2. **Generate answers** following the tone (see below).
3. **Save in the report** as section `## G) Draft Application Answers`.

### Generic questions (use if you cannot extract from the form)

- Why are you interested in this role?
- Why do you want to work at [Company]?
- Tell us about a relevant project or achievement
- What makes you a good fit for this position?
- How did you hear about this role?

### Tone for form answers

**Position: "I'm choosing you."** The candidate has options and is choosing this company for concrete reasons.

**Tone rules:**
- **Confident without arrogance**: "I've spent the past year building production AI agent systems — your role is where I want to apply that experience next"
- **Selective without snobbery**: "I've been intentional about finding a team where I can contribute meaningfully from day one"
- **Specific and concrete**: Always reference something REAL from the JD or the company, and something REAL from the candidate's experience
- **Direct, no fluff**: 2–4 sentences per answer. No "I'm passionate about..." or "I would love the opportunity to..."
- **The hook is proof, not claims**: Instead of "I'm great at X", say "I built X that does Y"

**Per-question framework:**
- **Why this role?** → "Your [specific thing] maps directly to [specific thing I built]."
- **Why this company?** → Name something concrete about the company. "I've been using [product] for [time/purpose]."
- **Relevant experience?** → One quantified proof point. "Built [X] that [metric]. Sold the company in 2025."
- **Good fit?** → "I sit at the intersection of [A] and [B], which is exactly where this role lives."
- **How did you hear?** → Be honest: "Found through [portal/scan], evaluated against my criteria, and it scored highest."

**Language**: Always match the JD language (English default). Apply `/tech-translate`.

## Step 5 — Update tracker

Record in `data/applications.md` with all columns including Report and PDF marked ✅.

**If any step fails**, continue with the rest and mark the failed step as pending in the tracker.
