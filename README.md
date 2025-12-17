# Performance Cycle Report Assistant

Generates performance-cycle reports for Technical Writers and Managers using Jira data and the LX competency frameworks. Reports are fully formatted and saved automatically.

## 🚀 Quick start
- Open Cursor Chat (`Ctrl+L` or `Cmd+L`) and paste:
  ```
  Generate my performance cycle report for 2025-01-01 to 2025-06-30.
  I'm a Level 2 Technical Writer.
  ```
- The assistant fetches Jira activity, calculates metrics, writes accomplishments and unfinished work by quarter/work area, compares to level expectations, and saves two Markdown files in `reports/`.

> **Important:** Always state whether you are an individual contributor or a manager. IC (Technical Writer) tops out at **L3**; the manager track also starts at **L3**. You can say “technical writer”, “tech writer”, “ic”, or “individual collaborator” for IC roles, and “technical writing manager” or “manager” for manager roles.

## What you get
- **Work Summary** (`work-summary-[date-range].md`): overall, per-quarter, and per-work-area metrics; accomplishments; unfinished tasks with blocker analysis.
- **Performance Analysis** (`performance-analysis-[date-range].md`): strengths and areas to develop across six competencies; alignment summary.

## Supported levels
- **Technical Writers:** L1 (Technical Writer I), L2 (Technical Writer II), L3 (Senior Technical Writer)
- **Technical Writing Managers:** L3–L6 (manager track starts at L3)

> **Competency frameworks differ by role:** IC requests use the Technical Writer framework; manager requests use the Technical Writing Manager framework. Always specify whether you’re an IC or a manager so the right competencies are applied.

## Prerequisites
- Cursor IDE (latest)
- Jira Cloud access with appropriate permissions
- Atlassian account (authentication handled automatically via `mcp.json`)

## Documentation
- **[Quick Start](QUICK_START.md)** — one-page generate-and-go
- **[Usage Guide](docs/USAGE_GUIDE.md)** — detailed instructions and tips
- **[Metrics Guide](METRICS_GUIDE.md)** — how metrics are calculated and interpreted
- **[Setup](docs/SETUP.md)** — MCP configuration and troubleshooting (rarely needed)
- **[Examples](examples/example-request.md)** — sample requests
- **[Example report](examples/example-report-with-metrics.md)** — full sample output
- **[Changelog](CHANGELOG.md)** — release history

## Customization
- Replace competency frameworks: `context/Technical writer career path.csv` (writers) or `context/Tech Writer Career Path - Technical Writing Manager.csv` (managers).
- Adjust report format and metrics in `.cursorrules` (sections 4.1 and 4.2).
- Tweak JQL filters in `.cursorrules` section 1 if your Jira workflow differs.

## Troubleshooting
- **No Jira issues returned:** ensure you are logged into Atlassian; try “Show me my recent Jira issues”; verify date format `YYYY-MM-DD`; restart Cursor if MCP connection stalls.
- **Report feels incomplete:** add non-Jira activities or specific projects in your request.
- **Wrong level/role:** restate your level explicitly (e.g., “I’m a Level 3 Technical Writing Manager”).

## Project structure
```
performance-cycle/
├── .cursorrules                               # Assistant configuration
├── .gitignore                                 # Git ignore rules
├── mcp.json                                   # Atlassian MCP auto-configuration
│
├── README.md                                  # Overview (this file)
├── QUICK_START.md                             # One-page quick reference
├── METRICS_GUIDE.md                           # Quantitative metrics explained
│
├── context/
│   ├── Technical writer career path.csv                       # LX expectations (writers L1–L3)
│   └── Tech Writer Career Path - Technical Writing Manager.csv # Manager expectations (L3–L6)
├── CHANGELOG.md                               # Release history
│
├── docs/
│   ├── SETUP.md                               # MCP configuration
│   └── USAGE_GUIDE.md                         # Detailed usage guide
│
├── examples/
│   ├── example-request.md                     # Sample requests
│   └── example-report-with-metrics.md         # Complete example report
│
└── reports/                                   # Generated reports (auto-created)
```

