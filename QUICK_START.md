# ⚡ Quick Start

> **Note:** Both Atlassian Rovo MCP and GitHub MCP are automatically configured via `mcp.json`. For GitHub, just set the `GITHUB_TOKEN` environment variable (see [Setup Guide](docs/SETUP.md)).
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

> **Note:** Jira and GitHub data are fetched automatically. Mention additional activities not tracked in systems (mentoring, presentations, process improvements, team outcomes, etc.).

**Technical writing manager**

```
Generate my performance cycle report for 2025-01-01 to 2025-06-30.
I'm a Level 3 Technical Writing Manager.
```

```
Q2 2025, manager, L3
```

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
   - Jira metrics (completion rate, avg resolution time, etc.)
   - GitHub metrics (PRs, commits, reviews - if configured)
   - Accomplishments by quarter and area
   - Unfinished tasks with blocker analysis

2. **Performance Analysis** (`performance-analysis-[date-range].md`)
   - Competency areas with strengths and development areas
   - Summary of alignment

## Data Sources

- **Jira** (required): Automatically fetched via Atlassian MCP
- **GitHub** (optional): Automatically included if `GITHUB_TOKEN` environment variable is set
  - The project `mcp.json` already includes GitHub MCP configuration
  - Just set `GITHUB_TOKEN` environment variable and restart Cursor (see [Setup Guide](docs/SETUP.md))
  - Captures: PRs authored/reviewed, documentation commits, repository contributions
  - **Verify token:** `echo $env:GITHUB_TOKEN` (Windows) or `echo $GITHUB_TOKEN` (Mac/Linux)

## Need Help?

- **[Metrics Guide](METRICS_GUIDE.md)** - Understanding quantitative metrics
- **[Examples](examples/example-request.md)** - More sample requests
- **[Usage Guide](docs/USAGE_GUIDE.md)** - Detailed instructions
- **[README](README.md)** - Full documentation
