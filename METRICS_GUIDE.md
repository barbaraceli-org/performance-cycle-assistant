# Metrics Guide

This guide explains the quantitative metrics automatically included in your work summary reports.

## Overview

The Performance Cycle Report Assistant automatically calculates and includes quantitative metrics from Jira and GitHub (if configured) at three levels:

1. **Overall metrics** - Summary of all work during the review period
2. **Per-quarter metrics** - Breakdown by calendar quarter
3. **Per-work-area metrics** - Detailed metrics for each project/initiative

## Data Sources

- **Jira** (required): Task tracking, issue management, project work
- **GitHub** (optional): Pull requests, code reviews, documentation commits
  - Automatically included if GitHub MCP server is configured
  - See [Setup Guide](docs/SETUP.md) for GitHub configuration

## Metric Definitions

### Jira Metrics

These metrics track your Jira activity:

| Metric | Definition | Why It Matters |
|--------|------------|----------------|
| **Total issues started** | Issues that were "In Progress" at any point during the review period, including issues that moved to "In Progress" during the period and issues already "In Progress" at period start (carryover work) | Shows actual work volume - reflects all work that was "on your plate" during the period, regardless of when originally started |
| **Issues completed** | Issues with status Done/Resolved/Closed during the period | Demonstrates productivity and delivery |
| **Issues in progress** | Issues actively being worked on at period end | Shows current workload and pipeline |
| **Issues blocked/unfinished** | Issues that couldn't be completed | Highlights obstacles and dependencies |
| **Completion rate** | (Completed ÷ Started) × 100 | Measures efficiency and delivery success - counts all issues that were actively being worked on during the period (including carryover from before the period) |
| **Average resolution time** | Mean time from "in progress" to completion (days) | Indicates actual work velocity and complexity |
| **Work areas covered** | Number of distinct projects/initiatives | Shows breadth of contribution |

### Issue Type Breakdown

Shows distribution of work by type:

- **Documentation** - New docs, updates, improvements
- **Bug fixes** - Corrections to existing content
- **Tasks** - General work items
- **Stories** - User-focused features
- **Other types** - Any additional issue types in your project

Each type shows count and percentage (e.g., "Documentation: 54 (62%)")

### Priority Distribution

Shows how work was prioritized:

- **High** - Critical or urgent items
- **Medium** - Standard priority work
- **Low** - Nice-to-have improvements

Helps demonstrate focus on high-impact work.

### GitHub Metrics (Optional)

These metrics track your GitHub contributions when GitHub MCP is configured:

| Metric | Definition | Why It Matters |
|--------|------------|----------------|
| **Pull requests authored** | PRs you created (total, merged, open) | Shows documentation contributions in code repos |
| **Pull requests reviewed** | PRs you reviewed for others | Demonstrates collaboration and technical review skills |
| **Documentation commits** | Commits to .md, docs/, README files | Tracks direct documentation updates |
| **Repositories contributed to** | Distinct repos where you contributed | Shows breadth of impact across projects |
| **Average PR merge time** | Mean time from PR creation to merge (days) | Indicates efficiency and collaboration speed |
| **Lines changed** | Lines added/deleted in documentation files | Shows volume of documentation work |

**Documentation file types tracked:**
- Markdown files (*.md)
- README files
- API documentation
- Contributing guides
- Documentation site content

**Per-repository breakdown:**
- Contributions by repository
- PR counts per repo
- Documentation files modified per repo

### Per-Quarter Metrics

Each quarter section includes:

- **Issues completed** - Finished during that quarter
- **In progress** - Active at quarter end
- **Completion rate** - Success rate for that quarter

Format: `**Q1 Metrics:** 38 issues completed | 5 in progress | 88% completion rate`

### Per-Work-Area Metrics

Each work area (project/initiative) includes:

- **Completed** - Issues finished in this area
- **In progress** - Issues currently active
- **Avg resolution** - Mean time from "in progress" to completion (days)

Format: `**Metrics:** 15 completed | 2 in progress | Avg resolution: 6 days`

*Note: Work areas with fewer than 3 issues may omit metrics to avoid statistical noise.*

### Unfinished Work Metrics

The "What couldn't be finished" section includes:

| Metric | Definition | Why It Matters |
|--------|------------|----------------|
| **Total unfinished issues** | All non-completed issues | Shows scope of remaining work |
| **In progress** | Count and % of active issues | Indicates work that's moving forward |
| **Blocked** | Count and % of blocked issues | Highlights obstacles needing attention |
| **Average age** | Mean time since creation (days) | Shows how long items have been pending |
| **Primary blockers** | Top 3-5 categories of blockers | Identifies patterns in obstacles |

Each unfinished work area shows:
- Count of unfinished issues
- Average age in days

Format: `**Metrics:** 4 unfinished issues | Avg age: 62 days`

## How Metrics Are Calculated

### Date Ranges and Inclusion

Issues are included if they had **any activity** during the review period, regardless of when they were created or completed. This reflects "when the task was on your plate."

### Handling Issues Started Before the Period

**"Total issues started" includes carryover work:**

Issues that were already "In Progress" at the start of the review period (started before the period) are counted in "Total issues started" if they:
- Were completed during the period, OR
- Remained "In Progress" during the period (even if not completed)

This ensures the metric reflects all work that was actively being worked on during the period, not just newly started work.

**Example:**
- Period: January 2025
- 12 issues moved to "In Progress" during January (new starts)
- 8 issues were already "In Progress" at January 1st and were completed during January (carryover)
- **Total issues started:** 20 (12 + 8)
- **Issues completed:** 20
- **Completion rate:** 100% (20/20)

This approach provides a fairer view of your productivity by including all work that was "on your plate" during the period, regardless of when it was originally started.

### Calculation Rules

- **Percentages**: Rounded to nearest whole number (e.g., 83%)
- **Days**: Rounded to 1 decimal place (e.g., 8.5 days)
- **Missing data**: Shows "N/A" or omits metric if calculation not possible
- **Zero values**: Shown explicitly as "0" rather than omitted
- **Rounding**: Conservative (round down for rates, round up for time)

### GitHub-Specific Calculation Rules

- **PR merge time**: Calculated only for merged PRs (created_at to merged_at)
- **Documentation commits**: Filtered by file patterns (*.md, docs/**, README*, CONTRIBUTING*, etc.)
- **Lines changed**: Sum of additions and deletions in documentation files only
- **Repository counting**: Each distinct repository where you had activity counts once
- **Review counting**: Includes all review types (approve, request changes, comment)
- **Date filtering**: GitHub activities included if they occurred during the review period

### Average Resolution Time Calculation

**Important:** Average resolution time measures **actual work time**, not total issue lifetime.

**Formula:** `avg(resolutiondate - in_progress_date)` for completed issues

**How "in progress" date is determined:**
1. **Primary method**: Extract from Jira changelog the first time the issue status changed to "In Progress" (or "In Review", "In Development")
2. **Fallback method**: If changelog is unavailable, use the `updated` date when the issue status is "In Progress"

**Why this matters:**
- More accurately reflects your actual work velocity
- Excludes time when issues were waiting in backlog or not yet assigned
- Aligns with the principle that metrics should reflect "when the task was on your plate"
- Provides fairer comparison across different work types and dependencies

**Example:**
- Issue created: 2025-01-01
- Moved to "In Progress": 2025-01-15 (14 days in backlog)
- Completed: 2025-01-25
- **Resolution time**: 10 days (not 24 days)

### Status Mapping

**Completed statuses**: Done, Resolved, Closed, Completed

**In Progress statuses**: In Progress, In Review, In Development, In Testing

**Blocked statuses**: Blocked, On Hold, Waiting, Impediment

**Unfinished statuses**: Backlog, To Do, Open (at period end)

## Using Metrics in Performance Reviews

### What Metrics Show

✅ **Good indicators:**
- Volume and consistency of work (Jira + GitHub)
- Efficiency and velocity
- Breadth of contribution across systems
- Ability to complete work
- Focus on priorities
- Collaboration through code reviews (GitHub)
- Technical engagement with repositories (GitHub)

❌ **What metrics DON'T show:**
- Quality of work (requires qualitative review)
- Complexity or difficulty
- Strategic impact
- Communication effectiveness
- Learning and growth
- Impact of mentoring or leadership

### Interpreting Your Metrics

**High completion rate (>80%)**: Demonstrates strong execution and realistic scope management

**Lower completion rate (<70%)**: May indicate scope creep, dependencies, or overly ambitious planning (not necessarily negative)

**Fast average resolution (<5 days)**: Shows efficiency, may indicate simpler tasks or strong expertise. Note: This measures time from "in progress" to completion, not total issue lifetime.

**Slow average resolution (>15 days)**: May indicate complex work, dependencies, or need for process improvement. Remember: This reflects actual work time, so longer times may indicate genuinely complex tasks.

**Many blocked issues**: Highlights need for stakeholder engagement or dependency management

**High work area count**: Demonstrates versatility and broad contribution

**Low work area count**: May indicate deep focus and specialization (not necessarily negative)

**High PR count (GitHub)**: Shows active engagement with code repositories and developer workflows

**High review count (GitHub)**: Demonstrates collaboration, technical expertise, and team support

**Fast PR merge time (GitHub)**: Indicates efficient collaboration and clear documentation

**Many repositories (GitHub)**: Shows broad impact across multiple projects or teams


## Example Interpretation

```
**Q1 Metrics:** 38 issues completed | 5 in progress | 88% completion rate
```

**What this tells you:**
- Strong delivery (88% completion)
- Healthy pipeline (5 in progress)
- Consistent productivity (38 completed in 3 months ≈ 3 per week)

```
**Metrics:** 15 completed | 2 in progress | Avg resolution: 6 days
```

**What this tells you:**
- Efficient execution (6-day average)
- Active work area (2 in progress)
- Significant contribution (15 issues)

```
**Unfinished work metrics:**
- **Total unfinished issues:** 15
- **In progress:** 11 (73%)
- **Blocked:** 4 (27%)
- **Average age:** 45 days
```

**What this tells you:**
- Most unfinished work is actively progressing (73%)
- Small number of blockers (4 issues)
- Some items have been pending for a while (45 days average)
- May need to address blockers or re-prioritize old items

## Best Practices

### Do:
- ✅ Use metrics to support your accomplishment narratives
- ✅ Explain context when metrics seem unusual
- ✅ Highlight trends across quarters
- ✅ Reference metrics when discussing efficiency or scope
- ✅ Use blocker analysis to identify process improvements

### Don't:
- ❌ Rely solely on metrics without qualitative context
- ❌ Compare your metrics directly to others (different work types)
- ❌ Treat high volume as automatically better than low volume
- ❌ Ignore low completion rates without investigation
- ❌ Use metrics to justify poor quality work

## Questions?

If you have questions about:
- **How metrics are calculated**: See "How Metrics Are Calculated" above
- **What metrics mean**: See "Metric Definitions" above
- **How to interpret your results**: See "Using Metrics in Performance Reviews" above
- **Customizing metrics**: Modify `.cursorrules` section 4.2

---

*Last updated: December 2025*
