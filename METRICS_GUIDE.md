# Metrics Guide

This guide explains the quantitative metrics automatically included in your work summary reports.

## Overview

The Performance Cycle Report Assistant automatically calculates and includes quantitative metrics at three levels:

1. **Overall metrics** - Summary of all work during the review period
2. **Per-quarter metrics** - Breakdown by calendar quarter
3. **Per-work-area metrics** - Detailed metrics for each project/initiative

## Metric Definitions

### Overall Metrics

These appear at the top of your work summary report:

| Metric | Definition | Why It Matters |
|--------|------------|----------------|
| **Total issues worked on** | All Jira issues with activity during the review period | Shows overall workload volume |
| **Issues completed** | Issues with status Done/Resolved/Closed during the period | Demonstrates productivity and delivery |
| **Issues in progress** | Issues actively being worked on at period end | Shows current workload and pipeline |
| **Issues blocked/unfinished** | Issues that couldn't be completed | Highlights obstacles and dependencies |
| **Completion rate** | (Completed ÷ Total) × 100 | Measures efficiency and delivery success |
| **Average resolution time** | Mean time from creation to completion (days) | Indicates velocity and complexity |
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
- **Avg resolution** - Mean time to complete issues (days)

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

### Calculation Rules

- **Percentages**: Rounded to nearest whole number (e.g., 83%)
- **Days**: Rounded to 1 decimal place (e.g., 8.5 days)
- **Missing data**: Shows "N/A" or omits metric if calculation not possible
- **Zero values**: Shown explicitly as "0" rather than omitted
- **Rounding**: Conservative (round down for rates, round up for time)

### Status Mapping

**Completed statuses**: Done, Resolved, Closed, Completed

**In Progress statuses**: In Progress, In Review, In Development, In Testing

**Blocked statuses**: Blocked, On Hold, Waiting, Impediment

**Unfinished statuses**: Backlog, To Do, Open (at period end)

## Using Metrics in Performance Reviews

### What Metrics Show

✅ **Good indicators:**
- Volume and consistency of work
- Efficiency and velocity
- Breadth of contribution
- Ability to complete work
- Focus on priorities

❌ **What metrics DON'T show:**
- Quality of work (requires qualitative review)
- Complexity or difficulty
- Strategic impact
- Collaboration and communication
- Learning and growth

### Interpreting Your Metrics

**High completion rate (>80%)**: Demonstrates strong execution and realistic scope management

**Lower completion rate (<70%)**: May indicate scope creep, dependencies, or overly ambitious planning (not necessarily negative)

**Fast average resolution (<5 days)**: Shows efficiency, may indicate simpler tasks or strong expertise

**Slow average resolution (>15 days)**: May indicate complex work, dependencies, or need for process improvement

**Many blocked issues**: Highlights need for stakeholder engagement or dependency management

**High work area count**: Demonstrates versatility and broad contribution

**Low work area count**: May indicate deep focus and specialization (not necessarily negative)


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
