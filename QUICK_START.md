# ⚡ Quick Start

> **Note:** The Atlassian Rovo MCP is automatically configured via `mcp.json`. Just open Cursor and start!
>
> **More detail?** See the **[Usage Guide](docs/USAGE_GUIDE.md)**.
>
> **Release history?** See the **[CHANGELOG.md](CHANGELOG.md)**.
>
> **Frameworks:** IC requests use the Technical Writer competency framework (`context/technical-writer-career-path.json`); manager requests use the Technical Writing Manager framework (`context/technical-writing-manager-career-path.json`, includes Management expectations). Your stated role selects the correct one.

## Generate Your Report

Open Cursor Chat (`Ctrl+L` or `Cmd+L`) and specify the date range, role, and level. See the examples below:

> Specify whether you are an individual contributor or a manager. The IC (Technical Writer) track progresses up to **L3**, while the manager track starts at **L3**. Use "technical writer", "tech writer", "ic", or "individual collaborator" for IC, and "technical writing manager" or "manager" for managers.

**Technical writer**

```
Generate my performance cycle report for 2025-01-01 to 2025-06-30.
I'm a Level 2 IC.
```

```
H1 2025
Tech writer
L3
```

> **ICs:** include activities beyond Jira that demonstrate your impact across competencies: mentoring/onboarding support, chapter/community participation, process improvements you drove, cross-team collaboration, documentation strategy work, content audits/reorganizations, user research, speaking/presentations, writing guidelines/standards, tools/automation you built, and links to key artifacts (style guides, templates, research findings, metrics dashboards).


**Technical writing manager**

```
Generate my performance cycle report for 2025-01-01 to 2025-06-30.
I'm a Level 3 Technical Writing Manager.
```

```
Q2 2025, manager, L3
```

> **Managers:** include Management evidence that isn't in Jira (team outcomes, health signals, escalations, process leadership, stakeholder comms, coaching, strategy/roadmap decisions, incident leadership, and links to plans/retros/dashboards) so the Management competency is fully covered.

## 📋 Best Practice: Generate Reports by Period

**Recommendation:** Generate reports for **shorter periods** rather than an entire year.

**Why?**
- More manageable data volume
- Clearer quarterly patterns and trends
- Easier to review and validate
- Better alignment with typical performance review cycles

**Suggested periods:**
- **By quarter:** Q1, Q2, Q3, Q4
- **By semester:** H1 (Jan-Jun), H2 (Jul-Dec)
- **By month:** For detailed monthly tracking

**Example:**
Instead of:
```
Generate my report for 2025
```

Do this:
```
Generate my report for Q1 2025
```

> **Tip:** You can still review a full year by generating and reviewing quarterly reports together.

That's it!

## Levels

**Technical Writers**
- **L1** - Technical Writer I
- **L2** - Technical Writer II
- **L3** - Senior Technical Writer

**Technical Writing Managers**
- **L3** - Technical Writing Manager I (first manager level)
- **L4** - Technical Writing Manager II
- **L5** - Senior Manager, Technical Writing
- **L6** - Head of Education & Documentation

## What You Get

Two reports saved to `reports/`:

1. **Work Summary** (`work-summary-[date-range].md`)
   - Quantitative metrics (completion rate, avg resolution time, etc.)
   - Accomplishments by quarter and area
   - Unfinished tasks with blocker analysis

2. **Performance Analysis** (`performance-analysis-[date-range].md`)
   - 6 competency areas with strengths and development areas
   - Summary of alignment

## Need Help?

- **[Metrics Guide](METRICS_GUIDE.md)** - Understanding quantitative metrics
- **[Examples](examples/example-request.md)** - More sample requests
- **[Usage Guide](docs/USAGE_GUIDE.md)** - Detailed instructions
- **[README](README.md)** - Full documentation
