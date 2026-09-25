---
name: generate-brag-doc
description: Generate a brag document (daily, weekly, biweekly, monthly, quarter, or semester) for a Technical Writer from Jira/GitHub/Slack/Drive activity, with each accomplishment linked to career-path competencies and a paste-ready Hibob goals update. Use when the user asks for a brag doc, brag document, weekly brag, "@brag", or gives a cadence plus a period (e.g., "weekly last week", "monthly February 2026"). Produces only the brag doc — it never triggers the Work Summary or Performance Analysis.
---

# Generate Brag Doc

One brag doc per request. Structure and length depend on the cadence. The brag doc is also the source for updating goal progress in Hibob (Bob), so every run includes a Hibob goals update.

## Inputs

- **Cadence and period (required):** Resolve relative terms ("yesterday", "last week", "this month") to concrete `YYYY-MM-DD` dates using the current date.
  - **Daily:** a single day (e.g., "yesterday", "2026-02-09").
  - **Weekly:** a calendar week, Monday to Sunday (e.g., "last week", "2026-W06").
  - **Biweekly:** a two-week period (e.g., "last 2 weeks", "2026-02-09 to 2026-02-22").
  - **Monthly:** a calendar month (e.g., "February 2026", "2026-02").
  - **Quarter:** Q1–Q4 (e.g., "Q1 2026").
  - **Semester:** H1 (Jan–Jun) or H2 (Jul–Dec) (e.g., "H1 2026").
- **Role (optional):** defaults to Technical Writer. Use the Technical Writing Manager framework only when the user says they're a manager.
- **Level (optional):** used only to read the matching competency descriptions. Brag docs link work to competencies; they never rate them, so a missing level doesn't block the run.
- **Extra context (optional):** activities the user lists in the request (presentations, mentoring, process improvements). Treat each as evidence with the same weight as a kept `additional-context.local.md` entry.

## Steps

1. Read `../_shared/data-collection.md` in full and follow it exactly for the requested period: validate the Atlassian MCP and GitHub connections first (mandatory — stop and report if either fails), check `context/additional-context.local.md` filtered to the period, and retrieve Jira, GitHub, Slack, and user-referenced Drive data.
2. **Record each source's status** for the "Sources checked" line (see below): Jira and GitHub are always `connected` (the run would have stopped in step 1 otherwise). Slack is `connected` if the plugin/connection succeeded and was queried, or `not connected` if it was skipped. Google Drive is `referenced` if the user named a document this run — directly or via a kept `additional-context.local.md` entry — or `not referenced` if no document was named.
3. **Brag-only Jira filter:** after retrieval, drop Epics and issues whose summary contains the word `LOC` or the phrase `LOC REVIEW` (case-insensitive, whole word — "location" is not excluded). Dropped Epics still name the project headings (see "Grouping by project").
4. **Read `context/hibob-goals.local.md`** (always, not only when the user mentions it). Keep the `## Cycle: YYYY` block matching the year of the period's start date. If the file or the block doesn't exist, omit the Hibob goals update section and say so in the completion message.
5. **Load the career-path framework** for the role:
   - Technical Writer → `context/technical-writer-career-path.json`. Competency keys are listed in `dimensions[].competencies`; use `dimensions[].label` as the dimension name.
   - Technical Writing Manager → `context/technical-writing-manager-career-path.json`. Keys are dimension-level (`levels[level].competencies`).
   - Read the behavior each key describes from `levels[userLevel].competencies[key]`. If no level was given, read the key's descriptions across all levels to understand the behavior it names.
6. Read `../_shared/writing-standards.md` for tone, evidence-linking, regeneration, and output rules. **Brag exception:** bullets use active, outcome-focused verbs ("Shipped…", "Unblocked…", "Reduced…") while staying evidence-based and free of flattery.
7. Group the work, link competencies, match it to Hibob KRs, and build the report using the structure for the cadence below — starting with the `**Sources checked:**` line from step 2.
8. Save to `reports/[YYYY]/brag/[filename]` (year of the period's start date; create the folder if needed). The regeneration policy in `writing-standards.md` applies.

## Grouping by project

Highlights are grouped by project, for every cadence. A project is the Jira Epic the work rolls up to.

- **Resolve the Epic:** request the `parent` field with every Jira issue; if the response omits it, retry with the full view. If the parent's issue type is Epic, that Epic is the project. Otherwise fetch the parent with its own `parent` field and repeat until you reach an Epic (a task can sit under a story that sits under the Epic). An issue with no Epic above it has no project. Resolve each parent key once and reuse the result across issues.
- **Project heading:** the Epic's summary exactly as written in Jira, followed by its key: `### [Store Framework] 26H2 (EDU-18912)`.
- **Grouping order:** parent epic → components → labels → repo. Group by Epic whenever there is one; work without an Epic goes under `### Other`, clustered there by component, then label, then repo.
- **Reviews always go under `### Other`**, even when the review ticket sits under an Epic: peer-review tickets and PRs reviewed for others.
- **GitHub, Slack, and Drive:** a PR belongs to the project of the Jira key in its title, branch, or description. Fold Slack threads and Drive docs into the bullet they support. PRs with no Jira key and activity with no Jira/GitHub match go under `### Other`.
- Order project headings by bullet count, most first, with `### Other` always last. Omit headings with no bullets.

## Competency linking

- End every accomplishment bullet with its competency tags: `— *Competencies:* \`content_quality\`, \`ecosystem_collaboration\``.
- Tag 1 to 3 keys per bullet. Only tag a key when the work shows the behavior that competency describes in the career-path JSON (e.g., an editorial review of another team's docs → `editorial_governance`; a cross-team sync or unblocking another team → `ecosystem_collaboration`; a taxonomy or navigation restructure → `content_strategy`).
- Use only keys that exist in the loaded JSON. Never invent keys or use dimension labels as keys (Technical Writer track).
- Don't rate or score competencies — ratings belong to `generate-performance-analysis`.

## Hibob goals update

Match the period's evidence (Jira, GitHub, Slack, Drive, additional context, extra context from the request) against every KR in the kept cycle block:

- A KR matches when the evidence directly advances what the KR describes. Jira keys named in a KR (e.g., `EDU-17897`) are direct matches. Otherwise match by theme against the KR text; skip weak or speculative matches.
- **Linked competencies:** infer them from the matched evidence, using the rules in "Competency linking" above. `hibob-goals.local.md` doesn't store competencies.
- **Progress note for Bob:** 1–3 sentences the user can paste into Bob as-is. Write it **in the KR's own language** (a Portuguese KR gets a Portuguese note; an English KR gets an English note). State what moved, cite Jira keys/PR numbers, and name the next step when one is clear.
- Goals marked `Status: Draft` still get their matched KRs listed, under the marker `Not yet in Bob: enter the KRs before updating progress`.
- A goal with no matching KR gets the single line `No KR activity this period.`

Section structure (identical for every cadence):

```markdown
## Hibob goals update

### Goal N — [Goal title, as written in the goals file]
**Status:** [Validated in Bob | Not yet in Bob: enter the KRs before updating progress]

- **KR N:** [KR text, as written in the goals file]
  - **Evidence:** [Jira keys, PR numbers, Slack threads, Drive docs]
  - **Linked competencies:** `key`, `key`
  - **Progress note for Bob:** [1–3 sentences in the KR's language]

### Goal N — [Goal title]
**Status:** Validated in Bob

No KR activity this period.
```

## Competency coverage

Every cadence ends with a short list of each competency key tagged in the report (accomplishment bullets and KR entries), with its dimension (Technical Writer track) and the number of bullets it was tagged on, sorted by count. List only tagged keys.

```markdown
## Competency coverage
- `ecosystem_collaboration` (Responsibility & Scope): 4 bullets
- `content_quality` (Writing): 3 bullets
```

## Sources checked line

Every report opens with one line, right after the title and before the first section, stating which sources fed the report:

```markdown
**Sources checked:** Jira, GitHub, Slack ([connected|not connected]), Google Drive ([referenced|not referenced])
```

- Jira and GitHub always read `Jira, GitHub` with no status — a failed connection stops the run before any report is written.
- Slack and Google Drive always show their status from step 2, so the reader knows at a glance whether a source was skipped rather than simply empty of results.
- Keep the line as-is; don't add explanations or extra sources.

## Filenames

- Daily: `brag-daily-YYYY-MM-DD.md`
- Weekly: `brag-weekly-YYYY-Www.md` (ISO week)
- Biweekly: `brag-biweekly-YYYY-MM-DD-to-YYYY-MM-DD.md`
- Monthly: `brag-monthly-YYYY-MM.md`
- Quarter: `brag-quarter-YYYY-Qn.md`
- Semester: `brag-semester-YYYY-Hn.md` (H1 or H2)

## Structure by cadence

Each structure below is followed by `## Hibob goals update` and `## Competency coverage`, in that order.

**Daily:**

```markdown
# Brag doc — YYYY-MM-DD

**Sources checked:** Jira, GitHub, Slack ([connected|not connected]), Google Drive ([referenced|not referenced])

## Highlights
### [Epic summary] (EPIC-KEY)
- [3–5 bullets in total: what you did + impact; Jira/PR refs in parentheses; competency tags]
### Other
- [Reviews and work without an Epic]
## Blockers / notes (optional)
- [If any]
```

**Weekly:**

```markdown
# Brag doc — Week of YYYY-MM-DD to YYYY-MM-DD

**Sources checked:** Jira, GitHub, Slack ([connected|not connected]), Google Drive ([referenced|not referenced])

## Highlights
### [Epic summary] (EPIC-KEY)
- [5–10 bullets in total, with evidence and competency tags]
### [Epic summary] (EPIC-KEY)
- [Same]
### Other
- [Reviews and work without an Epic]
## Metrics (optional)
- Issues completed: X | PRs merged: Y | Reviews: Z
```

**Biweekly:**

```markdown
# Brag doc — Week of YYYY-MM-DD to YYYY-MM-DD

**Sources checked:** Jira, GitHub, Slack ([connected|not connected]), Google Drive ([referenced|not referenced])

## Highlights
### [Epic summary] (EPIC-KEY)
- [Bullets with evidence and competency tags]
### [Epic summary] (EPIC-KEY)
- [Bullets with evidence and competency tags]
### Other
- [Reviews and work without an Epic]
### In Progress
- [Current work items, each starting with its project heading text, e.g., **[Store Framework] 26H2 (EDU-18912):**, or **Other:**]
## Metrics
- Issues completed: X | Issues in progress: Y | PRs merged: Z | PRs under review: W
```

**Monthly:**

```markdown
# Brag doc — Month YYYY-MM

**Sources checked:** Jira, GitHub, Slack ([connected|not connected]), Google Drive ([referenced|not referenced])

## Overview
- Short summary sentence and key metrics (issues completed, PRs, projects).

## By project
### [Epic summary] (EPIC-KEY)
- [Bullets with evidence and competency tags]
### [Epic summary] (EPIC-KEY)
- [Bullets with evidence and competency tags]
### Other
- [Reviews and Jira/GitHub work without an Epic]
## Other wins
- [Presentations, mentoring, process — from user context, additional context, or Slack]
```

**Quarter / Semester:**

```markdown
# Brag doc — Qn YYYY (or H1/H2 YYYY)

**Sources checked:** Jira, GitHub, Slack ([connected|not connected]), Google Drive ([referenced|not referenced])

## Overview
- Metrics: issues completed, in progress, PRs merged, reviews, projects.

## Accomplishments by project
### [Epic summary] (EPIC-KEY)
- [Evidence-based bullets with Jira/PR refs and competency tags]
### [Epic summary] (EPIC-KEY)
- [Same]
### Other
- [Reviews and work without an Epic]
## Highlights and impact
- [2–4 bullets on biggest impact or focus]
```

## Metrics

- **Issues completed:** Jira issues with normalized status "Completed" and `resolutiondate` in the period (after the brag-only filter).
- **Issues in progress:** normalized status "In Progress" at period end.
- **PRs merged / PRs under review:** authored PRs merged in the period / still open at period end.
- **Reviews:** PRs reviewed in the period, excluding self-authored PRs.
- If Slack is connected, you may append `Slack threads helped: N` to the metrics line; omit it otherwise.

## Writing rules

- One sentence per bullet; 3–10 bullets per section depending on cadence.
- Always include Jira keys and PR numbers where applicable, linked to their URLs.
- Valid Markdown; no placeholders or empty sections (optional sections are omitted when empty).
- Output only the report; save it and tell the user the full path and whether it was created or overwritten.
