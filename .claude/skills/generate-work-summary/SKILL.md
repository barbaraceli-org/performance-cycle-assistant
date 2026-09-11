---
name: generate-work-summary
description: Generate the Work Summary report for a performance cycle — accomplishments, unfinished work, and blocker analysis for a Technical Writer or Technical Writing Manager, built from Jira/GitHub/Slack/Drive activity over a date range. Use when the user asks for a work summary, accomplishments report, "what did I get done" report, or a performance-cycle report and names a date range, role, and level.
---

# Generate Work Summary

Extract blocker reasons from issue descriptions, comments, and labels — don't rely on status labels alone.

## Steps

1. Read `../_shared/data-collection.md` in full and follow it exactly: validate the Atlassian MCP connection first (mandatory — stop and report if it fails), then retrieve and process Jira/GitHub/Slack/Drive data as described there.
2. Read `../_shared/writing-standards.md` for tone, evidence-linking, and output-location rules.
3. Build the report using the structure and metrics below.
4. Save to `reports/[YYYY]/work-summary-[date-range].md` (year folder per `writing-standards.md`).

## Structure

```markdown
# Work summary

**Period:** [[date range]]

## Overview Metrics

### Jira Activity
- **Total issues worked on:** X (issues that were "In Progress" at any point during the period, including carryover work from before the period)
- **Carryover issues:** Y (issues already "In Progress" at period start)
- **New issues started:** Z (issues that moved to "In Progress" during the period)
- **Scope creep:** W (issues created and assigned after period start)
- **Issues completed:** Y
- **Issues in progress:** Z (actively being worked on at period end; may include blocked issues)
- **Issues blocked:** W (blocked status at period end; may overlap with in progress)
- **Issues unfinished:** V (backlog/other non-completed issues at period end, excluding in progress and blocked)
- **Completion rate:** N% (completed / worked on; X issues still in progress)
- **Average resolution time:** M days (resolutiondate - in_progress_date)
- **Work areas covered:** K
- **Issue type breakdown:** [counts and percentages for Epic, New, Update, Review, Task]
- **Priority distribution:** [counts and percentages]

### GitHub Activity
- **Pull requests authored:** X total (Y merged, Z open, W closed)
- **Pull requests reviewed:** X across N repositories
- **Review-to-author ratio:** X.X:1 (reviews / authored PRs)
- **Documentation commits:** X commits in Y PRs, Z files modified (or X standalone commits, Y files modified if counting direct commits)
- **Repositories contributed to:** N
- **Average PR merge time:** X.X days
- **Lines changed:** +X,XXX / -X,XXX (or N/A if data unavailable)
- **PR status breakdown:** [merged, open, closed with percentages]
- **Repository distribution:** [counts and percentages]

### Slack Activity
*(Omit this subsection entirely if the Slack plugin is not connected; otherwise state "Slack: not connected" only if connected but no matching activity in range)*
- **Messages/threads authored:** X across N channels
- **Threads helped/answered:** Y (mentoring/force-multiplier signal)
- **Threads started (asked for help):** Z
- **Jira/PR mentions found:** W (linked back to work areas)
- **Channel distribution:** [counts and percentages for top channels]

*(No "Google Drive Activity" Overview Metrics subsection: Drive is an additional-context source, not an aggregated metric — user-provided Drive docs appear as supporting evidence in the relevant work area/competency instead.)*

## Accomplishments

### Quarter 1
**Jira:** X issues completed | Y in progress | Z% completion rate
**GitHub:** X PRs merged | Y open | Z reviews | N repositories

#### Work Area 1
**Jira metrics:** X issues completed | Y in progress | Avg resolution: Z days

**Accomplishments:**
- [Concrete bullet with Jira key/PR number/Slack link/Drive doc when applicable]
- [Combine PR+Jira: "Completed X (EDU-123, PR #456)"]
- [Standalone: "Fixed Y (PR #789)" or "Completed Z (EDU-999)"]
- [Slack-sourced: "Helped unblock N teammates on API doc formatting questions (#dev-docs thread)"]
- [Drive-sourced, only when the user provided the doc: "Authored the Q2 documentation style guide (Google Doc)"]

### Quarter 2-4
[Same structure, only include quarters in date range]

## What couldn't be finished

**Unfinished work metrics:**
- **Jira:** X unfinished total (In progress: Y | Blocked: Z | Backlog: W) | Avg age: N days
- **GitHub:** X open PRs | Draft: Y | Avg age: N days
- **Primary blockers:** [Top 3-5 categories with counts]

#### Work Area 1
**Metrics:** X unfinished Jira issues | Y open PRs | Avg age: Z days

**Unfinished tasks:**
- [Concrete bullet with current status/next step]

### Blocker Analysis

**Blocker categories (semantically grouped from descriptions, comments, and labels):**
- **[Semantic Category 1]:** X issues | Avg resolution: Y days
  - *Root causes:* [Specific themes from issue content]
  - *Mitigation:* [Strategies]
- **[Semantic Category 2]:** X issues | Avg resolution: Y days
  - *Root causes:* [Specific themes from issue content]
  - *Mitigation:* [Strategies]
- **[Semantic Category 3-5]:** [Same structure]

**Recurring impediment patterns:**
- [Most common blocker theme with frequency and impact]
- [Second most common blocker theme]
- [Third most common blocker theme]
```

## Metrics Calculation

**Jira:**
- **Worked on:** Issues that were "In Progress" at any point during [[date range]], including:
  - Issues that moved to "In Progress" during the period (new starts)
  - Issues that were already "In Progress" at period start (carryover work)
  - This reflects all work that was "on your plate" during the period, regardless of when it was originally started
- **Carryover issues:** Issues with status = "In Progress" before period start date. Flag high carryover (>30% of worked on) to contextualize completion rate—indicates complexity, persistence, or inherited workload
- **New issues started:** Issues that transitioned to "In Progress" during [[date range]] (created_date OR first "In Progress" transition within period)
- **Scope creep:** Issues created AND assigned to user after period start date. High scope creep (>40% of new starts) indicates reactive work or poor planning
- **Completed:** Normalized status = "Completed" AND resolutiondate within [[date range]]
- **In progress:** Normalized status = "In Progress" at period end (includes issues that may also be blocked)
- **Blocked:** Normalized status = "Blocked" at period end (may overlap with in progress if issue is both active and blocked)
- **Unfinished:** Normalized status = "Backlog" or other non-completed statuses at period end, excluding in progress and blocked
- **Completion rate:** (completed / worked on) × 100, rounded to whole number. Include context: "X issues still in progress" and "Y carryover issues" to provide clarity when rate appears low due to active work or inherited complexity
- **Resolution time:** avg(resolutiondate - in_progress_date) in days, 1 decimal place

**GitHub:**
- **PRs authored:** All PRs during [[date range]] (total count)
  - **Merged:** state = "merged" AND merged_at within [[date range]]
  - **Open:** state = "open" at period end
  - **Closed:** state = "closed" AND NOT merged (closed without merging)
- **PRs merged:** state = "merged" AND merged_at within [[date range]]
- **PRs reviewed:** Count of PRs where user submitted review comments (exclude self-authored PRs)
- **Review-to-author ratio:** (PRs reviewed / PRs authored), 1 decimal place. Ratio >1.5 indicates "Force Multiplier" behavior (unblocking others), a key trait for L2/L3 Technical Writers. Ratio <0.5 may indicate siloed work or limited team collaboration
- **Merge time:** avg(merged_at - created_at) in days, 1 decimal place (calculated only for merged PRs)
- **Documentation commits:** Count commits in PRs (or standalone commits if applicable) that modify documentation files (*.md, **/docs/**, README*, CONTRIBUTING*)
- **Lines changed:** Sum of additions/deletions in documentation files. If data unavailable, show "N/A" instead of placeholder text
- **Impact vs. Effort flags:** Identify outliers:
  - High-priority Jira issues (<50 lines changed): Potential invisible complexity or blocked work
  - Low-priority Jira issues (>1000 lines changed): Potential over-engineering or misaligned priorities

**Slack (only if plugin connected):**
- **Messages/threads authored:** Count of distinct messages/threads with `from:@me` (or public-only equivalent) within [[date range]]
- **Threads helped/answered:** Count of threads where the user replied to another person's question/request (exclude threads the user started); ratio of helped/answered ÷ threads started >1.5 supports "Force Multiplier" / mentoring evidence, mirroring the GitHub review-to-author ratio interpretation
- **Jira/PR mentions found:** Count of distinct messages containing a Jira key or PR number pattern, deduped by issue/PR
- **Channel distribution:** Group by channel, count and percentage of total

**Google Drive:** Not part of automatic metric calculation. Each Drive doc the user references is included as a single supporting-evidence bullet in the relevant work area/competency — no counts, ratios, or Overview Metrics entries are computed for it.

**Per-quarter:** Same calculations scoped to each quarter
**Per-work-area:** Jira metrics only (completed, in progress, avg resolution time)

**Rounding:** Percentages to whole number, days to 1 decimal, conservative rounding (down for rates, up for time)
