# Changelog

## Unreleased
- Added guidance to specify role (Technical Writer vs Technical Writing Manager) and level in report requests.
- Documented manager track availability (manager track starts at L3) across Quick Start, README, Usage Guide, and Navigation.
- Updated `.cursorrules` to load expectations from both writer and manager career-path CSVs based on role.

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
- Loaded LX Technical Writer expectations from `context/Technical writer career path.csv` for competency comparison.