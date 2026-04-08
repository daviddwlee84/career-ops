# Mode: deep — Deep Research Prompt

Generate a structured prompt for Perplexity/Claude/ChatGPT with six pillars:

```
## Deep Research: [Company] — [Role]

Context: I'm evaluating a candidacy for [role] at [company]. I need actionable information for the interview.

### 1. AI strategy
- What products or features use AI/ML?
- What is their AI stack? (models, infra, tools)
- Do they have an engineering blog? What do they publish?
- What papers or talks have they given on AI?

### 2. Recent moves (last 6 months)
- Relevant hires in AI/ML/product?
- Acquisitions or partnerships?
- Product launches or pivots?
- Funding rounds or leadership changes?

### 3. Engineering culture
- How do they ship? (deploy cadence, CI/CD)
- Monorepo or multi-repo?
- What languages and frameworks do they use?
- Remote-first or office-first?
- Glassdoor/Blind (or similar) sentiment on eng culture?

### 4. Likely challenges
- What scaling problems are they facing?
- Reliability, cost, or latency challenges?
- Are they migrating something? (infra, models, platforms)
- What pain points do people mention in reviews?

### 5. Competitors and differentiation
- Who are their main competitors?
- What is their moat or differentiator?
- How do they position vs. the competition?

### 6. Candidate angle
Given my profile (read from cv.md and profile.yml for specific experience):
- What unique value do I bring to this team?
- Which of my projects are most relevant?
- What story should I lead with in the interview?
```

Personalize each section with the specific context of the offer being evaluated.
