# Changelog

## Unreleased

## v2.3.0 — 2025-12-18
### Added
- **GitHub MCP integration** for tracking pull requests, code reviews, and documentation commits
  - Automatically fetches PRs authored and reviewed
  - Tracks documentation commits (*.md, docs/, README files)
  - Calculates GitHub metrics: PR merge time, lines changed, repositories contributed to
  - Merges GitHub activity with Jira data in work areas
- **Enhanced metrics** in work summary reports:
  - GitHub activity metrics (PRs, commits, reviews, repositories)
  - Per-repository contribution breakdown
  - Documentation file type analysis
- **Security enhancements**:
  - Updated `.gitignore` to protect GitHub tokens (*.token, secrets/, mcp.json.local)
  - Documented secure token management using environment variables
  - Added comprehensive GitHub MCP setup guide in docs/SETUP.md
- **Documentation updates**:
  - Added GitHub integration section to README, QUICK_START, and USAGE_GUIDE
  - Expanded METRICS_GUIDE with GitHub-specific metrics and calculations
  - Added GitHub examples to example-request.md
  - Updated all guides to reflect optional GitHub integration

### Changed
- `.cursorrules` now includes GitHub MCP data retrieval alongside Jira
- Work summary template includes separate sections for Jira and GitHub metrics
- Data processing now merges GitHub PRs with Jira issues when related

### Security
- **IMPORTANT**: Never commit GitHub tokens to Git. Use environment variables (`GITHUB_TOKEN`)
- GitHub MCP configuration should be in global `~/.cursor/mcp.json`, not project files

## v2.2.0 — 2025-12-18
- Added IC guidance prompting users to include non-Jira activities (mentoring, process improvements, cross-team collaboration, documentation strategy, user research, presentations, tools/automation).
- Added recommendation to generate reports by shorter periods (quarters, semesters, months) instead of full years.
- Changed average resolution time calculation from creation date to "in progress" date (extracted from Jira changelog).

## v2.1.0 — 2025-12-17
- Added guidance to specify role (Technical Writer vs Technical Writing Manager) and level in report requests.
- Documented manager track availability (manager track starts at L3) across Quick Start, README, Usage Guide, and Navigation.
- Updated `.cursorrules` to load expectations from both writer and manager career-path JSON files based on role.
- Simplified documentation (removed redundant Navigation doc, refreshed README/Quick Start) and aligned writer JSON filename with documented path.
- Clarified that competency frameworks differ for ICs and managers, and the correct one is chosen based on the stated role.

## v2.0.0 — Quantitative Metrics and Competency Analysis
- Added automatic quantitative metrics in work summary (completion rate, avg resolution time, issue type and priority distributions, per-quarter and per-work-area metrics, unfinished work metrics).
- Expanded performance analysis with structured strengths/areas-to-develop across six competencies and alignment summary.
- Improved examples and guides (example report with metrics, example requests).
- Added Navigation and Metrics guides; refreshed Quick Start and Usage guides.
- Ensured automatic Jira fetching via Atlassian MCP configuration.

## v1.0.0 — Initial Release
- Generated two Markdown reports: work summary and performance analysis for Technical Writers (L1–L3).
- Pulled Jira activity within a date range and grouped by quarter and work area.
- Included accomplishments and unfinished tasks with blocker analysis.
- Loaded LX Technical Writer expectations from `context/technical-writer-career-path.json` for competency comparison.