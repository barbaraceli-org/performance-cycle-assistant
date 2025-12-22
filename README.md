# Performance Cycle Report Assistant

Generates performance-cycle reports for Technical Writers and Managers using Jira and GitHub data with competency frameworks. Reports are fully formatted and saved automatically.

## 🚀 Quick start

See **[QUICK_START.md](QUICK_START.md)** for immediate usage instructions.

## What you get
- **Work Summary** (`work-summary-[date-range].md`): Jira and GitHub metrics; accomplishments by quarter/work area; unfinished tasks with blocker analysis.
- **Performance Analysis** (`performance-analysis-[date-range].md`): strengths and areas to develop across competency areas; alignment summary.

## Supported levels
- **Technical Writers:** L1 (Technical Writer I), L2 (Technical Writer II), L3 (Senior Technical Writer)
- **Technical Writing Managers:** L3–L6 (manager track starts at L3)

> **Important:** Always state whether you are an individual contributor or a manager. IC (Technical Writer) tops out at **L3**; the manager track starts at **L3**. Use "technical writer", "tech writer", "ic", or "individual collaborator" for IC roles, and "technical writing manager" or "manager" for manager roles.

## Prerequisites
- Cursor IDE (latest)
- Jira Cloud access with appropriate permissions
- Atlassian account (authentication handled automatically via `mcp.json`)
- GitHub account (optional, for repository contribution tracking) - see [Setup Guide](docs/SETUP.md)

## Documentation
- **[Quick Start](QUICK_START.md)** — one-page generate-and-go
- **[Usage Guide](docs/USAGE_GUIDE.md)** — detailed instructions and tips
- **[Metrics Guide](METRICS_GUIDE.md)** — how metrics are calculated and interpreted
- **[Setup](docs/SETUP.md)** — MCP configuration (Jira + GitHub)
- **[GitHub Integration](docs/GITHUB_INTEGRATION.md)** — complete GitHub MCP guide
- **[Examples](examples/example-request.md)** — sample requests
- **[Example report](examples/example-report-with-metrics.md)** — full sample output


## Customization
- Replace competency frameworks: `context/technical-writer-career-path.json` (writers) or `context/technical-writing-manager-career-path.json` (managers).
- Adjust report format and metrics in `.cursorrules` (sections 4.1 and 4.2).
- Tweak JQL filters in `.cursorrules` section 1 if your Jira workflow differs.

## Troubleshooting
See [Usage Guide](docs/USAGE_GUIDE.md#troubleshooting) for detailed troubleshooting steps.

## Project structure
```
performance-cycle/
├── .cursorrules                               # Assistant configuration
├── .gitignore                                 # Git ignore rules
├── mcp.json                                   # Atlassian MCP configuration (project-level)
│
├── README.md                                  # Overview (this file)
├── QUICK_START.md                             # One-page quick reference
├── METRICS_GUIDE.md                           # Quantitative metrics explained
├── CHANGELOG.md                               # Release history
│
├── context/
│   ├── technical-writer-career-path.json      # Expectations (writers L1–L3)
│   └── technical-writing-manager-career-path.json  # Expectations (managers L3–L6)
│
├── docs/
│   ├── SETUP.md                               # MCP configuration (Jira + GitHub)
│   ├── USAGE_GUIDE.md                         # Detailed usage guide
│   └── GITHUB_INTEGRATION.md                  # GitHub integration guide
│
├── examples/
│   ├── example-request.md                     # Sample requests
│   └── example-report-with-metrics.md         # Complete example report
│
└── reports/                                   # Generated reports (auto-created)
```

