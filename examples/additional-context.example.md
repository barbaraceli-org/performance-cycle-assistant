# 📎 Example: Additional Context (Local Only)

This is an example of the personal `context/additional-context.local.md` file that any Technical Writer can create to feed extra evidence into their own performance reports.

> **This file is git-ignored.** Copy it to `context/additional-context.local.md` (or create your own from scratch) — it will never be committed, just like the generated `reports/` folder (including year subfolders such as `reports/2025/`). See `.gitignore`: `context/*.local.md` and `context/additional-context*.md`.

Use it to track activities/evidence that aren't (or can't be) automatically retrieved via the Jira/GitHub queries defined in `.claude/skills/_shared/data-collection.md` — e.g., issues opened on repos outside the indexed scope, mentoring/community work, strategy docs, presentations, certifications, etc.

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
- **Resolution/Outcome:** [name the concrete artifact or result — see note below]
- **Suggested competency linkage:** [competency keys from the career-path JSON, e.g. `ecosystem_collaboration` (why it applies)]
```

**Two things worth knowing when writing entries:**

- **These entries count as evidence.** A manually provided item counts toward the ≥3 threshold a competency needs, on equal footing with Jira issues and GitHub PRs — but only if it names a concrete artifact or outcome. That's what **Resolution/Outcome** is for; an entry without one doesn't count.
- **Use real competency keys** from `context/technical-writer-career-path.json` (or `context/technical-writing-manager-career-path.json` for managers). Invented names like "team collaboration" can't be matched to a competency section. See the [competency vocabulary mapping](../METRICS_GUIDE.md#competency-vocabulary-mapping) if you're unsure which key a friendly name corresponds to.

---

## Sample entries

### GitHub Issue — acme-org/docs-platform#42: Search results show stale excerpts after content updates

- **URL:** https://github.com/acme-org/docs-platform/issues/42
- **Role:** Author
- **Date(s):** Opened 2025-02-10, closed 2025-03-01 (resolved via PR #58)
- **Summary:** Reported that search result excerpts were not refreshing after content edits, and outlined the expected re-indexing behavior with links to the relevant caching layer.
- **Resolution/Outcome:** Fixed by adding a cache invalidation hook on content publish; verified with manual QA across three locales.
- **Suggested competency linkage:** `problem_framing` (outlined expected behavior precisely enough for engineering to scope a fix); `ecosystem_collaboration` (cross-team work with engineering).

### Presentation — "Writing for Search: SEO Fundamentals for Docs"

- **URL:** N/A (internal team all-hands)
- **Role:** Presenter
- **Date(s):** 2025-04-15
- **Summary:** Delivered a 30-minute session to the documentation team on structuring content for search discoverability, covering heading hierarchy, metadata, and internal linking.
- **Resolution/Outcome:** Adopted as a reference deck for onboarding new writers.
- **Suggested competency linkage:** `strategic_influence` (shared knowledge across the team); `content_strategy` (search discoverability as a content decision); `editorial_governance` (deck adopted as an onboarding standard).

### Mentoring — Onboarding support for new Technical Writer hire

- **URL:** N/A
- **Role:** Mentor
- **Date(s):** 2025-01-06 to 2025-02-28
- **Summary:** Paired weekly with a new L1 hire to review their first PRs, walk through the docs style guide, and answer platform-specific questions.
- **Resolution/Outcome:** Mentee independently shipped their first three documentation PRs by end of period.
- **Suggested competency linkage:** `ecosystem_collaboration` (onboarded and mentored a new team member); `editorial_governance` (walked through the style guide in PR review).

---

## 🔗 Next Steps

- **Copy this file:** `cp examples/additional-context.example.md context/additional-context.local.md`
- **See report request examples:** [example-request.md](example-request.md)
- **See a complete report example:** [example-report-with-metrics.md](example-report-with-metrics.md)
- **Quick start:** [../README.md](../README.md)
