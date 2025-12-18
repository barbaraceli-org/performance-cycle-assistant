# Performance Cycle Report Assistant

Generates performance-cycle reports for Technical Writers and Managers using Jira and GitHub data with competency frameworks. Reports are fully formatted and saved automatically.

## 🚀 Quick start

- Open Cursor Chat (`Ctrl+L` or `Cmd+L`) and write your request. For example:

  ```
  Generate my performance cycle report for 2025-01-01 to 2025-06-30.
  I'm a Level 2 Technical Writer.
  ```

- The assistant fetches Jira activity and GitHub contributions (if configured), calculates metrics, writes accomplishments and unfinished work by quarter/work area, compares to level expectations, and saves two Markdown files in `reports/`.

> **Important:** Always state whether you are an individual contributor or a manager. IC (Technical Writer) tops out at **L3**; the manager track starts at **L3**. Use "technical writer", "tech writer", "ic", or "individual collaborator" for IC roles, and "technical writing manager" or "manager" for manager roles.
>
> **Note:** Jira and GitHub data are fetched automatically. Mention additional activities like mentoring, presentations, process improvements, or (for managers) team outcomes and leadership decisions that aren't tracked in systems.

## What you get
- **Work Summary** (`work-summary-[date-range].md`): Jira and GitHub metrics; accomplishments by quarter/work area; unfinished tasks with blocker analysis.
- **Performance Analysis** (`performance-analysis-[date-range].md`): strengths and areas to develop across competency areas; alignment summary.

## Data Sources
- **Jira** (required): Task tracking, issue management, project work
- **GitHub** (optional): Pull requests, code reviews, documentation commits, repository contributions
  - Automatically includes: PRs authored/reviewed, documentation commits, README updates, API docs
  - Captures work not tracked in Jira: in-repo documentation, community contributions, code reviews

## Supported levels
- **Technical Writers:** L1 (Technical Writer I), L2 (Technical Writer II), L3 (Senior Technical Writer)
- **Technical Writing Managers:** L3–L6 (manager track starts at L3)

> **Competency frameworks differ by role:** IC requests use the Technical Writer framework; manager requests use the Technical Writing Manager framework. Always specify whether you’re an IC or a manager so the right competencies are applied.

## Prerequisites
- Cursor IDE (latest)
- Jira Cloud access with appropriate permissions
- Atlassian account (authentication handled automatically via `mcp.json`)
- GitHub account (optional, for repository contribution tracking)
  - Requires GitHub Personal Access Token with `repo`, `read:org`, `read:user` scopes
  - **Quick setup:** The project `mcp.json` already includes GitHub MCP configuration. Just set the `GITHUB_TOKEN` environment variable (see [Setup Guide](docs/SETUP.md))

## Documentation
- **[Quick Start](QUICK_START.md)** — one-page generate-and-go
- **[Usage Guide](docs/USAGE_GUIDE.md)** — detailed instructions and tips
- **[Metrics Guide](METRICS_GUIDE.md)** — how metrics are calculated and interpreted
- **[Setup](docs/SETUP.md)** — MCP configuration (Jira + GitHub)
- **[GitHub Integration](docs/GITHUB_INTEGRATION.md)** — complete GitHub MCP guide
- **[Examples](examples/example-request.md)** — sample requests
- **[Example report](examples/example-report-with-metrics.md)** — full sample output
- **[Changelog](CHANGELOG.md)** — release history

> **Framework selection:** Always state IC vs manager. IC requests use `context/technical-writer-career-path.json`; manager requests use `context/technical-writing-manager-career-path.json` (includes Management expectations). The agent picks the framework based on your stated role.

## Customization
- Replace competency frameworks: `context/technical-writer-career-path.json` (writers) or `context/technical-writing-manager-career-path.json` (managers).
- Adjust report format and metrics in `.cursorrules` (sections 4.1 and 4.2).
- Tweak JQL filters in `.cursorrules` section 1 if your Jira workflow differs.

## Troubleshooting
- **No Jira issues returned:** ensure you are logged into Atlassian; try "Show me my recent Jira issues"; verify date format `YYYY-MM-DD`; restart Cursor if MCP connection stalls.
- **No GitHub data:** GitHub MCP is optional; verify `GITHUB_TOKEN` is set (`echo $env:GITHUB_TOKEN` on Windows, `echo $GITHUB_TOKEN` on Mac/Linux); ensure you've fully restarted Cursor after setting the variable; see [Setup Guide](docs/SETUP.md) for configuration.
- **Report feels incomplete:** add non-Jira/GitHub activities or specific projects in your request.
- **Wrong level/role:** restate your level explicitly (e.g., "I'm a Level 3 Technical Writing Manager").

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
├── CHANGELOG.md                               # Release history
│
├── context/
│   ├── technical-writer-career-path.json      # Expectations (writers L1–L3)
│   └── technical-writing-manager-career-path.json  # Expectations (managers L3–L6)
│
├── docs/
│   ├── SETUP.md                               # MCP configuration (Jira + GitHub)
│   ├── USAGE_GUIDE.md                         # Detailed usage guide
│   └── GITHUB_INTEGRATION.md                  # GitHub MCP integration guide
│
├── examples/
│   ├── example-request.md                     # Sample requests
│   └── example-report-with-metrics.md         # Complete example report
│
└── reports/                                   # Generated reports (auto-created)
```

