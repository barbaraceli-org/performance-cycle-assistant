# Changelog

## Unreleased

## v2.4.0 — 2025-12-22

### Added
- **Status name normalization** for accurate issue categorization:
  - Comprehensive mapping of custom Jira status names to standard categories (Completed, In Progress, Blocked, Backlog)
  - Case-insensitive matching with whitespace handling
  - Context-aware classification (e.g., issues with resolution dates treated as Completed)
  - Support for user-provided custom status mappings
  - Tracking of unmapped statuses for data quality reporting
  - "To Do" status is always treated as "Backlog", never as "In Progress"
- **Enhanced changelog extraction** with robust fallback chain:
  - Multi-tier fallback system: changelog → updated date → comment dates → created date
  - Data quality tracking of which fallback method was used per issue
  - More accurate "in progress" date extraction for resolution time calculations
  - Added explanatory note in Data Quality Notes section clarifying difference between changelog data (exact timestamps) and fallback date methods (estimated dates)
- **Improved work area clustering** with validation:
  - Multi-signal clustering using components, labels, repositories, issue links, text similarity, and epics
  - Validation rules: 3-15 issues per work area (merge if <3, split if >20)
  - User override capability: "Group these issues together" or "Split this work area"
- **Data quality warnings** in reports:
  - New "Data Quality Notes" section at top of work summary reports
  - Tracks status normalization, changelog availability, date completeness, unmapped statuses
  - Warns when data quality issues are significant (>20% of issues)
  - Helps users assess metric reliability
- **Enhanced blocker analysis**:
  - Extract blocker reasons from issue descriptions, comments, labels, and PR descriptions
  - Categorize blockers into 5 categories: External dependencies, Resource constraints, Technical blockers, Process blockers, Awaiting decisions
  - Track blocker resolution time (time from blocked to unblocked/resolved)
  - Provide mitigation strategies per category
  - Display blocker analysis section in "What couldn't be finished" with category breakdowns
- **Competency evidence linking**:
  - Link each competency bullet to specific Jira issues (e.g., "EDU-12345") or GitHub PRs (e.g., "PR #456")
  - Show evidence count per competency: "Evidence: X Jira issues, Y GitHub PRs"
  - Flag competencies with <3 evidence items as "Limited evidence"
  - Makes competency analysis more traceable and verifiable
- **Quality indicators**:
  - First-time-right rate: Percentage of issues completed without status regressions
  - Issues reopened: Count of issues moved from Completed back to In Progress/Blocked
  - Issues with multiple status changes: Count of issues with >3 status transitions (indicates rework)
  - Feedback items resolved: Count of issues with "feedback" in title/description/labels
  - New metrics section in Overview Metrics showing quality indicators

### Changed
- All Jira metrics now use normalized statuses for consistent categorization
- Resolution time calculations use improved fallback chain for better accuracy
- Work area clustering algorithm enhanced with multiple signals and validation
- Data quality metrics added to Jira metrics section
- Performance analysis report now includes evidence counts and issue/PR references in competency bullets
- "What couldn't be finished" section includes detailed blocker analysis with categories and mitigation strategies
- Overview Metrics now includes quality indicators
- Updated issue type breakdown to use actual Jira issue types (New, Epic, Review, Task, Sub-task, Update) instead of generic categories

### Removed
- Impact assessment metrics (customer-facing vs internal work, high priority work counts)
- "High Impact Work" section from work summary reports

### Documentation
- Updated `.cursorrules` with status normalization rules and enhanced data processing instructions

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