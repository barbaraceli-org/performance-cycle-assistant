# 📎 Example: Additional Context (Local Only)

This is an example of the personal `context/additional-context.local.md` file that any Technical Writer can create to feed extra evidence into their own performance reports.

> **This file is git-ignored.** Copy it to `context/additional-context.local.md` (or create your own from scratch) — it will never be committed, just like the generated `reports/` folder (including year subfolders such as `reports/2025/`). See `.gitignore`: `context/*.local.md` and `context/additional-context*.md`.

Use it to track activities/evidence that aren't (or can't be) automatically retrieved via the Jira/GitHub queries defined in `.cursorrules` — e.g., issues opened on repos outside the indexed scope, mentoring/community work, strategy docs, presentations, certifications, etc.

When you request a report, mention that you have local additional context (or paste relevant entries directly into the chat), and it will be folded into the relevant work areas/competencies alongside the automatically retrieved Jira/GitHub data.

---

## How to add an entry

Copy this template for each item:

```markdown
### [Source] — [Title]

- **URL:** 
- **Role:** [Author / Reviewer / Contributor / Presenter / etc.]
- **Date(s):** 
- **Summary:** [1-3 sentences on what it was and why it mattered]
- **Resolution/Outcome:** [if applicable]
- **Suggested competency linkage:** [optional — which competency/dimension this best supports]
```

---

## Sample entries

### GitHub Issue — acme-org/docs-platform#42: Search results show stale excerpts after content updates

- **URL:** https://github.com/acme-org/docs-platform/issues/42
- **Role:** Author
- **Date(s):** Opened 2025-02-10, closed 2025-03-01 (resolved via PR #58)
- **Summary:** Reported that search result excerpts were not refreshing after content edits, and outlined the expected re-indexing behavior with links to the relevant caching layer.
- **Resolution/Outcome:** Fixed by adding a cache invalidation hook on content publish; verified with manual QA across three locales.
- **Suggested competency linkage:** Technical depth / problem identification; cross-team collaboration (engineering).

### Presentation — "Writing for Search: SEO Fundamentals for Docs"

- **URL:** N/A (internal team all-hands)
- **Role:** Presenter
- **Date(s):** 2025-04-15
- **Summary:** Delivered a 30-minute session to the documentation team on structuring content for search discoverability, covering heading hierarchy, metadata, and internal linking.
- **Resolution/Outcome:** Adopted as a reference deck for onboarding new writers.
- **Suggested competency linkage:** Mentoring / knowledge sharing; strategy and standards.

### Mentoring — Onboarding support for new Technical Writer hire

- **URL:** N/A
- **Role:** Mentor
- **Date(s):** 2025-01-06 to 2025-02-28
- **Summary:** Paired weekly with a new L1 hire to review their first PRs, walk through the docs style guide, and answer platform-specific questions.
- **Resolution/Outcome:** Mentee independently shipped their first three documentation PRs by end of period.
- **Suggested competency linkage:** Mentoring; team collaboration.

---

## 🔗 Next Steps

- **Copy this file:** `cp examples/additional-context.example.md context/additional-context.local.md`
- **See report request examples:** [example-request.md](example-request.md)
- **See a complete report example:** [example-report-with-metrics.md](example-report-with-metrics.md)
- **Quick start:** [../README.md](../README.md)
