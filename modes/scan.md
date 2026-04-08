# Mode: scan — Portal Scanner (Offer Discovery)

Scans configured job portals, filters by title relevance, and adds new postings to the pipeline for later evaluation.

## Recommended execution

Run as a subagent so it does not consume the main context:

```
Agent(
    subagent_type="general-purpose",
    prompt="[contents of this file + specific data]",
    run_in_background=True
)
```

## Configuration

Read `portals.yml`, which contains:
- `search_queries`: List of WebSearch queries with `site:` filters per portal (broad discovery)
- `tracked_companies`: Specific employers with `careers_url` for direct navigation
- `title_filter`: positive/negative/seniority_boost keywords for title filtering

## Discovery strategy (3 tiers)

### Tier 1 — Direct Playwright (PRIMARY)

**For each company in `tracked_companies`:** Navigate to its `careers_url` with Playwright (`browser_navigate` + `browser_snapshot`), read ALL visible job listings, and extract title + URL for each. This is the most reliable method because:
- It sees the page in real time (not cached Google results)
- It works with SPAs (Ashby, Lever, Workday)
- It detects new postings instantly
- It does not depend on Google indexing

**Each company MUST have `careers_url` in portals.yml.** If it does not, find it once, save it, and use it in future scans.

### Tier 2 — Greenhouse API (SUPPLEMENTARY)

For companies on Greenhouse, the JSON API (`boards-api.greenhouse.io/v1/boards/{slug}/jobs`) returns clean structured data. Use as a fast supplement to Tier 1 — faster than Playwright but Greenhouse-only.

### Tier 3 — WebSearch queries (BROAD DISCOVERY)

`search_queries` with `site:` filters cover portals horizontally (all Ashby, all Greenhouse, etc.). Useful for discovering NEW employers not yet in `tracked_companies`, but results can be stale.

**Execution priority:**
1. Tier 1: Playwright → all `tracked_companies` with `careers_url`
2. Tier 2: API → all `tracked_companies` with `api:`
3. Tier 3: WebSearch → all `search_queries` with `enabled: true`

Tiers are additive — run all of them, merge results, and deduplicate.

## Workflow

1. **Read configuration**: `portals.yml`
2. **Read history**: `data/scan-history.tsv` → URLs already seen
3. **Read dedup sources**: `data/applications.md` + `data/pipeline.md`

4. **Tier 1 — Playwright scan** (parallel in batches of 3–5):
   For each company in `tracked_companies` with `enabled: true` and `careers_url` defined:
   a. `browser_navigate` to `careers_url`
   b. `browser_snapshot` to read all job listings
   c. If the page has filters/departments, navigate relevant sections
   d. For each listing extract: `{title, url, company}`
   e. If the page paginates results, navigate additional pages
   f. Accumulate in candidate list
   g. If `careers_url` fails (404, redirect), try `scan_query` as fallback and note for URL update

5. **Tier 2 — Greenhouse APIs** (parallel):
   For each company in `tracked_companies` with `api:` defined and `enabled: true`:
   a. WebFetch the API URL → JSON job list
   b. For each job extract: `{title, url, company}`
   c. Accumulate in candidate list (dedup with Tier 1)

6. **Tier 3 — WebSearch queries** (parallel if possible):
   For each query in `search_queries` with `enabled: true`:
   a. Run WebSearch with the defined `query`
   b. From each result extract: `{title, url, company}`
      - **title**: from the result title (before " @ " or " | ")
      - **url**: result URL
      - **company**: after " @ " in the title, or extract from domain/path
   c. Accumulate in candidate list (dedup with Tier 1+2)

6. **Filter by title** using `title_filter` from `portals.yml`:
   - At least 1 keyword from `positive` must appear in the title (case-insensitive)
   - 0 keywords from `negative` may appear
   - `seniority_boost` keywords add priority but are not required

7. **Deduplicate** against 3 sources:
   - `scan-history.tsv` → exact URL already seen
   - `applications.md` → normalized company + role already evaluated
   - `pipeline.md` → exact URL already in Pending or Processed

7.5. **Verify liveness of WebSearch results (Tier 3)** — BEFORE adding to pipeline:

   WebSearch results can be outdated (Google may cache results for weeks or months). To avoid evaluating expired postings, verify with Playwright each new URL that comes from Tier 3. Tiers 1 and 2 are inherently real-time and do not need this check.

   For each new Tier 3 URL (sequential — NEVER Playwright in parallel):
   a. `browser_navigate` to the URL
   b. `browser_snapshot` to read content
   c. Classify:
      - **Active**: job title visible + role description + Apply/Submit button
      - **Expired** (any of these signals):
        - Final URL contains `?error=true` (Greenhouse redirects this way when closed)
        - Page contains: "job no longer available" / "no longer open" / "position has been filled" / "this job has expired" / "page not found"
        - Only navbar and footer visible, no JD content (content < ~300 chars)
   d. If expired: log in `scan-history.tsv` with status `skipped_expired` and discard
   e. If active: continue to step 8

   **Do not abort the whole scan if one URL fails.** If `browser_navigate` errors (timeout, 403, etc.), mark as `skipped_expired` and continue with the next.

8. **For each new verified posting that passes filters**:
   a. Add to `pipeline.md` under "Pending": `- [ ] {url} | {company} | {title}`
   b. Log in `scan-history.tsv`: `{url}\t{date}\t{query_name}\t{title}\t{company}\tadded`

9. **Title-filtered postings**: log in `scan-history.tsv` with status `skipped_title`
10. **Duplicate postings**: log with status `skipped_dup`
11. **Expired postings (Tier 3)**: log with status `skipped_expired`

## Extracting title and company from WebSearch results

WebSearch results often look like: `"Job Title @ Company"` or `"Job Title | Company"` or `"Job Title — Company"`.

Extraction patterns by portal:
- **Ashby**: `"Senior AI PM (Remote) @ EverAI"` → title: `Senior AI PM`, company: `EverAI`
- **Greenhouse**: `"AI Engineer at Anthropic"` → title: `AI Engineer`, company: `Anthropic`
- **Lever**: `"Product Manager - AI @ Temporal"` → title: `Product Manager - AI`, company: `Temporal`

Generic regex: `(.+?)(?:\s*[@|—–-]\s*|\s+at\s+)(.+?)$`

## Private URLs

If you find a URL that is not publicly accessible:
1. Save the JD to `jds/{company}-{role-slug}.md`
2. Add to pipeline.md as: `- [ ] local:jds/{company}-{role-slug}.md | {company} | {title}`

## Scan history

`data/scan-history.tsv` tracks ALL URLs seen:

```
url	first_seen	portal	title	company	status
https://...	2026-02-10	Ashby — AI PM	PM AI	Acme	added
https://...	2026-02-10	Greenhouse — SA	Junior Dev	BigCo	skipped_title
https://...	2026-02-10	Ashby — AI PM	SA AI	OldCo	skipped_dup
https://...	2026-02-10	WebSearch — AI PM	PM AI	ClosedCo	skipped_expired
```

## Output summary

```
Portal Scan — {YYYY-MM-DD}
━━━━━━━━━━━━━━━━━━━━━━━━━━
Queries run: N
Postings found: N total
Title-filtered: N relevant
Duplicates: N (already evaluated or in pipeline)
Expired discarded: N (dead links, Tier 3)
New additions to pipeline.md: N

  + {company} | {title} | {query_name}
  ...

→ Run /career-ops pipeline to evaluate the new postings.
```

## Managing careers_url

Each company in `tracked_companies` should have `careers_url` — the direct URL to its job listings page. That avoids looking it up every time.

**Known patterns by platform:**
- **Ashby:** `https://jobs.ashbyhq.com/{slug}`
- **Greenhouse:** `https://job-boards.greenhouse.io/{slug}` or `https://job-boards.eu.greenhouse.io/{slug}`
- **Lever:** `https://jobs.lever.co/{slug}`
- **Custom:** The company’s own URL (e.g. `https://openai.com/careers`)

**If `careers_url` is missing** for a company:
1. Try the known pattern for its platform
2. If that fails, quick WebSearch: `"{company}" careers jobs`
3. Navigate with Playwright to confirm it works
4. **Save the URL you find in portals.yml** for future scans

**If `careers_url` returns 404 or redirects:**
1. Note it in the output summary
2. Try `scan_query` as fallback
3. Flag for manual update

## Maintaining portals.yml

- **Always save `careers_url`** when adding a new company
- Add new queries as you discover portals or interesting roles
- Disable noisy queries with `enabled: false`
- Adjust filter keywords as target roles evolve
- Add companies to `tracked_companies` when you want to follow them closely
- Verify `careers_url` periodically — companies change ATS platforms
