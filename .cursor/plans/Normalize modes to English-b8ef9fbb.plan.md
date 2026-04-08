<!-- b8ef9fbb-53bd-43af-9b99-980b0824b90f -->
---
todos:
  - id: "es-mirror"
    content: "Create modes/es/ -- copy all current Spanish root mode files with English filenames, fix internal cross-refs, add README.md"
    status: pending
  - id: "translate-root"
    content: "Translate all root mode files from Spanish to English with North America market framing; rename oferta->offer, ofertas->compare, contacto->contact; delete old Spanish-named files"
    status: pending
  - id: "translate-batch-prompt"
    content: "Translate batch/batch-prompt.md from Spanish to English"
    status: pending
  - id: "rename-de-fr"
    content: "Rename modes/de/angebot.md->offer.md, bewerben.md->apply.md; modes/fr/offre.md->offer.md, postuler.md->apply.md; fix internal refs"
    status: pending
  - id: "update-localized-readmes"
    content: "Update READMEs in de/, fr/, zh-cn/, zh-tw/ to reference new English filenames and English-root reality"
    status: pending
  - id: "update-skill-router"
    content: "Update .claude/skills/career-ops/SKILL.md router table, discovery menu, and context loading to use offer/compare/contact"
    status: pending
  - id: "update-claude-md"
    content: "Update CLAUDE.md command tables, skill modes mapping, language-mode descriptions, add modes/es/ option"
    status: pending
  - id: "update-opencode-cmds"
    content: "Update .opencode/commands/career-ops-evaluate.md, career-ops-compare.md, career-ops-contact.md to reference new mode names"
    status: pending
  - id: "update-readme"
    content: "Update README.md command examples in EN, zh-TW, and ES sections; update project structure snippets"
    status: pending
  - id: "update-system-files"
    content: "Update DATA_CONTRACT.md, update-system.mjs, test-all.mjs, interview-prep/story-bank.md"
    status: pending
  - id: "update-localized-apply"
    content: "Update /career-ops contacto references in modes/de/bewerben.md (now apply.md), fr/postuler.md (now apply.md), zh-cn/apply.md, zh-tw/apply.md"
    status: pending
  - id: "verify"
    content: "Verify no old Spanish filenames/mode-ids remain outside modes/es/ and .specstory/; run node test-all.mjs"
    status: pending
isProject: false
---
# Normalize Modes to English Root and Move Spanish to `modes/es`

## Current State

The root `modes/` set is Spanish-heavy: `oferta.md`, `ofertas.md`, `contacto.md` have Spanish filenames, and nearly all root mode files (`apply.md`, `pipeline.md`, `auto-pipeline.md`, `batch.md`, `scan.md`, `pdf.md`, `deep.md`, `tracker.md`, `training.md`, `project.md`) have Spanish content. Only `_shared.md` is already English.

German (`de/`) and French (`fr/`) folders use localized filenames (`angebot.md`, `bewerben.md`, `offre.md`, `postuler.md`). Chinese folders (`zh-cn/`, `zh-tw/`) already use English filenames. A `modes/es/` directory exists but is empty.

## Work Streams

### Stream 1: Create `modes/es/` -- Spanish mirror (preserve originals)

Before modifying root files, copy the current Spanish content into `modes/es/` with standardized English filenames:

- `modes/oferta.md` --> `modes/es/offer.md`
- `modes/ofertas.md` --> `modes/es/compare.md`
- `modes/contacto.md` --> `modes/es/contact.md`
- `modes/apply.md` --> `modes/es/apply.md`
- `modes/pipeline.md` --> `modes/es/pipeline.md`
- `modes/auto-pipeline.md` --> `modes/es/auto-pipeline.md`
- `modes/batch.md` --> `modes/es/batch.md`
- `modes/scan.md` --> `modes/es/scan.md`
- `modes/pdf.md` --> `modes/es/pdf.md`
- `modes/deep.md` --> `modes/es/deep.md`
- `modes/tracker.md` --> `modes/es/tracker.md`
- `modes/training.md` --> `modes/es/training.md`
- `modes/project.md` --> `modes/es/project.md`
- `modes/_shared.md` --> `modes/es/_shared.md` (Spanish version of the shared context)

Internal cross-references in `modes/es/` files should use the new English filenames (e.g., `modes/es/auto-pipeline.md` should reference `modes/es/offer.md` not `modes/oferta.md`).

Create `modes/es/README.md` following the pattern of `modes/de/README.md` -- explaining when to use, how to activate (`language.modes_dir: modes/es`), vocabulary reference.

Also copy and translate `batch/batch-prompt.md` content to `modes/es/` context -- the batch-prompt.md in root stays as English (see Stream 2).

### Stream 2: Translate root modes to English (North America framing)

Translate every Spanish mode file in `modes/` to English. This is the core content work.

**Files that need renaming + translation:**
- `modes/oferta.md` --> `modes/offer.md` (translate + North America comp sources)
- `modes/ofertas.md` --> `modes/compare.md` (translate)
- `modes/contacto.md` --> `modes/contact.md` (translate)

**Files that keep their name but need translation:**
- `modes/apply.md` -- currently Spanish, translate to English
- `modes/pipeline.md` -- currently Spanish, translate to English
- `modes/auto-pipeline.md` -- currently Spanish, translate to English. Update internal references: `modes/oferta.md` --> `modes/offer.md`
- `modes/batch.md` -- currently Spanish, translate to English
- `modes/scan.md` -- currently Spanish, translate to English
- `modes/pdf.md` -- currently Spanish, translate to English
- `modes/deep.md` -- currently Spanish, translate to English
- `modes/tracker.md` -- currently Spanish, translate to English
- `modes/training.md` -- currently Spanish, translate to English
- `modes/project.md` -- currently Spanish, translate to English
- `batch/batch-prompt.md` -- currently Spanish, translate to English

**North America framing rules for translated content:**
- Compensation: Glassdoor, Levels.fyi, Blind, LinkedIn Salary, Indeed, Built In
- Employment terms: base salary, total comp, equity/RSU, PTO, health insurance, 401(k)/RRSP, visa sponsorship, work authorization, hybrid/onsite, start date, notice period
- Keep scoring logic and operational structure identical -- only language and market framing change

**Files that stay as-is:**
- `modes/_shared.md` -- already English
- `modes/_profile.template.md` -- user template, stays in root

**Delete after creating `modes/es/` and translating:**
- `modes/oferta.md`
- `modes/ofertas.md`
- `modes/contacto.md`

### Stream 3: Rename localized mode files

Rename files without changing their content language, only fix internal cross-references:

- `modes/de/angebot.md` --> `modes/de/offer.md`
- `modes/de/bewerben.md` --> `modes/de/apply.md`
- `modes/fr/offre.md` --> `modes/fr/offer.md`
- `modes/fr/postuler.md` --> `modes/fr/apply.md`

Update internal references within these files:
- `modes/de/bewerben.md` line 107: `/career-ops contacto` --> `/career-ops contact`
- `modes/fr/postuler.md` line 107: `/career-ops contacto` --> `/career-ops contact`
- `modes/zh-cn/apply.md` line 114: `/career-ops contacto` --> `/career-ops contact`
- `modes/zh-tw/apply.md` line 114: `/career-ops contacto` --> `/career-ops contact`

### Stream 4: Update localized READMEs

Each localized README references old filenames and the "EN/ES" root reality. Update:

**`modes/de/README.md`:**
- Line 28: `modes/de/angebot.md` --> `modes/de/offer.md`
- Line 53: `angebot.md | modes/oferta.md (ES)` --> `offer.md | modes/offer.md (EN)`
- Line 54: `bewerben.md` --> `apply.md`
- Line 57: Remove `contacto`, `ofertas` from EN/ES references, explain root is now English

**`modes/fr/README.md`:**
- Line 45: `offre.md | modes/oferta.md (ES)` --> `offer.md | modes/offer.md (EN)`
- Line 46: `postuler.md` --> `apply.md`
- Line 49: Remove `contacto`, `ofertas` from EN/ES references, explain root is now English

**`modes/zh-cn/README.md`:**
- Line 26: `modes/zh-cn/offer.md` (already correct filename)
- Line 52: `modes/oferta.md` --> `modes/offer.md`
- Line 57: Remove `contacto` from untranslated list, update to say root is now English

**`modes/zh-tw/README.md`:**
- Line 26: `modes/zh-tw/offer.md` (already correct filename)
- Line 52: `modes/oferta.md` --> `modes/offer.md`
- Line 57: Remove `contacto` from untranslated list, update to say root is now English

### Stream 5: Update all reference files

**`.claude/skills/career-ops/SKILL.md`:**
- Router table (lines 18-29): `oferta` --> `offer`, `ofertas` --> `compare`, `contacto` --> `contact`
- Discovery menu (lines 47-57): Update command names and descriptions to English
- Context loading section (line 72): `oferta`, `ofertas`, `contacto` --> `offer`, `compare`, `contact`

**`CLAUDE.md`:**
- "Skill Modes" table: `oferta` --> `offer`, `ofertas` --> `compare`, `contacto` --> `contact`
- Command table under "OpenCode Commands": update descriptions referencing old Spanish names
- Language modes section: add `modes/es/` as Spanish option; update `de` and `fr` sections to reference renamed files (`angebot.md` --> `offer.md`, etc.)
- "Personalization" section line about "Translate the modes to English" can be changed
- Data contract references in main file text

**`.opencode/commands/career-ops-evaluate.md`:**
- Line 5: `oferta mode` --> `offer mode`

**`.opencode/commands/career-ops-compare.md`:**
- Line 5: `ofertas mode` --> `compare mode`

**`.opencode/commands/career-ops-contact.md`:**
- Line 5: `contacto mode` --> `contact mode`

**`README.md`:**
- English section (line 100): `/career-ops contacto` --> `/career-ops contact`
- English project structure (line 169): `oferta.md` --> `offer.md`
- Traditional Chinese section (line 317): `/career-ops contacto` --> `/career-ops contact`
- Traditional Chinese project structure (line 385-386): `oferta.md` --> `offer.md`
- Spanish section (line 529): `/career-ops contacto` --> `/career-ops contact` (or keep as-is since this is the Spanish README section -- but command surface is English-only per requirements)

**`DATA_CONTRACT.md`:**
- System layer table (lines 33-43): rename `oferta.md`, `ofertas.md`, `contacto.md` to `offer.md`, `compare.md`, `contact.md`; add `modes/es/`

**`update-system.mjs`:**
- SYSTEM_PATHS array (lines 34-36): `modes/oferta.md` --> `modes/offer.md`, `modes/ofertas.md` --> `modes/compare.md`, `modes/contacto.md` --> `modes/contact.md`; add `modes/es/`

**`test-all.mjs`:**
- systemFiles array (line 104): `modes/oferta.md` --> `modes/offer.md`
- expectedModes array (lines 185-188): `oferta.md` --> `offer.md`, `ofertas.md` --> `compare.md`, `contacto.md` --> `contact.md`

**`interview-prep/story-bank.md`:**
- Line 7: `/career-ops oferta` --> `/career-ops offer`

### Stream 6: Verification

After all changes, verify:
- Root `modes/` contains no Spanish filenames (`oferta.md`, `ofertas.md`, `contacto.md`)
- Root `modes/` has complete file set: `_shared.md`, `_profile.template.md`, `offer.md`, `compare.md`, `contact.md`, `apply.md`, `pipeline.md`, `auto-pipeline.md`, `batch.md`, `scan.md`, `pdf.md`, `deep.md`, `tracker.md`, `training.md`, `project.md`
- `modes/es/` has full Spanish mirror with English filenames
- `modes/de/` has `offer.md` and `apply.md` (not `angebot.md` / `bewerben.md`)
- `modes/fr/` has `offer.md` and `apply.md` (not `offre.md` / `postuler.md`)
- No references to `oferta`, `ofertas`, or `contacto` as mode IDs or filenames outside `modes/es/` content, `.specstory/`, and `.git/`
- Run `node test-all.mjs`

## Files Changed (count estimate)

- ~15 new files in `modes/es/` (copies of current Spanish content + README)
- ~15 root mode files translated to English (in-place content replacement)
- 3 root mode files renamed (oferta/ofertas/contacto --> offer/compare/contact)
- 4 localized files renamed (de: 2, fr: 2)
- 4 localized READMEs updated
- 4 localized apply.md files updated (internal command references)
- 8 reference files updated (CLAUDE.md, SKILL.md, 3 opencode commands, README.md, DATA_CONTRACT.md, update-system.mjs, test-all.mjs, story-bank.md, batch-prompt.md)

Total: ~50 file operations across the repo.
