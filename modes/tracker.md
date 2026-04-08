# Mode: tracker — Application Tracker

Read and display `data/applications.md`.

**Tracker format:**
```markdown
| # | Date | Company | Role | Score | Status | PDF | Report |
```

Possible statuses: `Evaluated` → `Applied` → `Responded` → `Interview` → `Offer` / `Rejected` / `Discarded` / `SKIP`

- `Applied` — the candidate submitted an application
- `Responded` — the company or recruiter reached out and the candidate replied (inbound)
- `Interview` — active interview process (includes phone screens, loops, take-homes)

If the user asks to update a status, edit the corresponding row.

Also show statistics:
- Total applications
- Count by status
- Average score
- % with PDF generated
- % with report generated
