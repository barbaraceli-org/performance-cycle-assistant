# Performance Cycle Report Assistant

Generates performance-cycle reports for Technical Writers and Managers using Jira and GitHub data with competency frameworks. Reports are fully formatted and saved automatically.

> **Note:** Atlassian Rovo MCP is automatically configured via project `mcp.json`. For GitHub MCP, configure it in your global `~/.cursor/mcp.json` file (see [Setup Guide](docs/SETUP.md)).

## 🚀 Quick Start

Open Cursor Chat (`Ctrl+L` or `Cmd+L`) and specify the date range, role, and level:

> Specify whether you are an individual contributor or a manager. The IC (Technical Writer) track progresses up to **L3**, while the manager track starts at **L3**. Use "technical writer", "tech writer", "ic", or "individual collaborator" for IC, and "technical writing manager" or "manager" for managers.

**Technical writer examples:**
```
Generate my performance cycle report for 2025-01-01 to 2025-06-30.
I'm a Level 2 IC.
```

```
H1 2025
Tech writer
L3
```

**Technical writing manager examples:**
```
Generate my performance cycle report for 2025-01-01 to 2025-06-30.
I'm a Level 3 Technical Writing Manager.
```

```
Q2 2025, manager, L3
```

> **Note:** Jira and GitHub data are fetched automatically. Mention additional activities not tracked in systems (mentoring, presentations, process improvements, team outcomes, etc.).

### 📋 Best Practice: Generate Reports by Period

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

## What you get

Two reports saved to `reports/`:

1. **Work Summary** (`work-summary-[date-range].md`)
   - Jira metrics (completion rate, carryover analysis, scope creep, avg resolution time, etc.)
   - GitHub metrics (PRs, commits, reviews, review-to-author ratio - if configured)
   - Accomplishments by quarter and area
   - Unfinished tasks with semantic blocker analysis and root cause identification

2. **Performance Analysis** (`performance-analysis-[date-range].md`)
   - Competency areas with strengths and development areas
   - Summary of alignment

## Supported levels

**Technical Writers**
- **L1** - Technical Writer I
- **L2** - Technical Writer II
- **L3** - Senior Technical Writer

**Technical Writing Managers**
- **L3** - Technical Writing Manager I (first manager level)
- **L4** - Technical Writing Manager II
- **L5** - Senior Manager, Technical Writing
- **L6** - Head of Education & Documentation

## Prerequisites
- Cursor IDE (latest)
- Jira Cloud access with appropriate permissions
- Atlassian account (authentication handled automatically via `mcp.json`)
- GitHub account (optional, for repository contribution tracking) - see [Setup Guide](docs/SETUP.md)

> **Note:** The assistant automatically validates the Jira connection before generating reports. If connection fails, you'll be directed to the [Setup Guide](docs/SETUP.md) for configuration help.

## Data Sources

- **Jira** (required): Automatically fetched via Atlassian MCP
  - Connection is validated automatically before report generation
  - If connection fails, see [Setup Guide](docs/SETUP.md) for configuration help
  - Issue types (Epic, New, Update, Review, Task) are detected automatically - see [Metrics Guide](METRICS_GUIDE.md#issue-type-breakdown)
- **GitHub** (optional): Automatically included if GitHub MCP is configured (see [Setup Guide](docs/SETUP.md))

## Documentation
- **[Setup Guide](docs/SETUP.md)** — MCP configuration (Jira + GitHub), usage instructions, troubleshooting
- **[Metrics Guide](METRICS_GUIDE.md)** — all metrics explained: basic + advanced (carryover, review ratios, impact vs. effort, semantic blockers)
- **[Examples](examples/example-request.md)** — sample requests
- **[Example report](examples/example-report-with-metrics.md)** — full sample output
- **[Changelog](CHANGELOG.md)** — release history


## Customization
- Replace competency frameworks: `context/technical-writer-career-path.json` (writers) or `context/technical-writing-manager-career-path.json` (managers).
- Adjust report format and metrics in `.cursorrules` (sections 4.1 and 4.2).
- Tweak JQL filters in `.cursorrules` section 1 if your Jira workflow differs.

## Jira Configuration

Reports automatically detect and display your Jira issue types (Epic, New, Update, Review, Task, etc.). See the [Metrics Guide](METRICS_GUIDE.md#issue-type-breakdown) for details on how issue types are tracked and displayed in reports.

## Troubleshooting
See [Setup Guide](docs/SETUP.md#troubleshooting) for detailed troubleshooting steps.

## Project structure
```
performance-cycle/
├── .cursorrules                               # Assistant configuration
├── .gitignore                                 # Git ignore rules
├── mcp.json                                   # Atlassian MCP configuration (project-level)
│
├── README.md                                  # Overview and quick start (this file)
├── METRICS_GUIDE.md                           # All metrics: basic + advanced
├── CHANGELOG.md                               # Release history
│
├── context/
│   ├── technical-writer-career-path.json      # Expectations (writers L1–L3)
│   └── technical-writing-manager-career-path.json  # Expectations (managers L3–L6)
│
├── docs/
│   └── SETUP.md                               # Setup, usage, and troubleshooting
│
├── examples/
│   ├── example-request.md                     # Sample requests
│   └── example-report-with-metrics.md         # Complete example report
│
└── reports/                                   # Generated reports (auto-created)
```

