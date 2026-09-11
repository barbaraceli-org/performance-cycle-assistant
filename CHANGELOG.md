# Changelog

## v2.6.0 — 2026-09-10

### Added
- **Claude Code support:** `CLAUDE.md` (entry point) and `AGENTS.md` (non-negotiable rules and project layout), plus two Skills under `.claude/skills/` — `generate-work-summary` and `generate-performance-analysis` — sharing retrieval and writing rules from `.claude/skills/_shared/`. `.mcp.json` mirrors `mcp.json` under Claude Code's expected filename.
- **Single source of truth for every agent:** `AGENTS.md` + `.claude/skills/` are now the only place report logic lives, with no generated copies. Cursor reads `AGENTS.md` as an always-applied root rule and loads `.claude/skills/` as Agent Skills (it reads Claude's skills directory for compatibility), which is what Claude Code already did — so one edit applies to both tools with nothing to regenerate or keep in sync.
- **Report regeneration policy:** Regenerating a report for a period now overwrites the existing file by default, but asks first when that file is more than 7 days old or looks hand-edited. Opting to keep both saves the new report as `[report-type]-[date-range]-[YYYY-MM-DD].md` and never deletes the original. Documented in `.claude/skills/_shared/writing-standards.md`, `README.md`, and `docs/SETUP.md`.
- **Example performance analysis report** (`examples/example-performance-analysis.md`): a full L2 Technical Writer evaluation covering all 14 IC competencies and all 5 dimensions, using the same period and data as the existing work summary example. Demonstrates all four ratings, the conservative dimension roll-up, and how "Limited evidence" is handled.
- **Competency vocabulary mapping** in `METRICS_GUIDE.md`: an explicit table mapping the friendly names used throughout the guide (Collaboration, Technical Writing, Documentation Strategy, and the dimension labels) to the actual competency keys per track. Metric-to-competency references throughout the guide now name the specific key rather than a dimension.
- **Privacy note** in `README.md` summarizing what `.gitignore` already excludes (generated `reports/`, local context files, tokens) for anyone forking or reusing the template.
- **Slack integration (optional):** Messages, threads, and canvases are now fetched via the Slack plugin (if connected) as an additional evidence source. Tracks messages/threads authored, threads helped/answered (mentoring signal), Jira/PR mentions, and channel distribution. New **thread-help ratio** metric (threads helped/answered ÷ threads started) mirrors the GitHub review-to-author ratio interpretation. Falls back to public-channel-only search if private/DM consent isn't granted. Not connecting the plugin does not block report generation — the "Slack Activity" section is simply omitted.
- **Google Drive integration (optional, additional-context only):** The Google Drive plugin can fetch a specific Doc/Slide/Sheet's title, last-modified date, and content summary — but only when the user explicitly references it by name or link (in the chat request or in `context/additional-context.local.md`). There is no automatic search, aggregation, or "Google Drive Activity" metrics section; referenced docs are added as single supporting-evidence bullets for the work area/competency the user names.
- **Dimension-level evaluation (IC track):** The performance analysis report now assigns an explicit rating to each career-path dimension (from `dimensions[]`), shown at the top of each dimension section and in a new "Dimension evaluation overview" table in the Summary. Ratings use a **conservative roll-up**: a dimension exceeds "Meets expectations" only when its competencies are *consistently* above bar (all "Exceeds" for "Exceeds expectations"), a single standout competency never lifts the dimension, and any below-bar competency caps the dimension at "Meets expectations". Not applicable to the Technical Writing Manager track, which has no dimension layer.

### Changed
- **Performance analysis report:** Each competency now receives an explicit rating (**Not yet meeting expectations**, **Meets expectations**, **Exceeds expectations**, or **Performing at the next level**) with rationale, supporting evidence, and actionable improvement steps. Replaces per-dimension Strengths/Areas to develop and Summary of alignment sections with an evaluation overview and priority development focus.
- **Evaluation scale expanded to 4 levels:** Renamed the 3-level scale (Need focus / On Track / Outperform) to a 4-level scale (Not yet meeting expectations / Meets expectations / Exceeds expectations / Performing at the next level). The new top level requires evidence matching the next career level's competency expectations (`levels[nextLevel].competencies[key]`) and is not assignable at the top of each track (Technical Writer L4, Technical Writing Manager L6).
- **Exceeds expectations criteria redefined:** The distinction between "Meets expectations" and "Exceeds expectations" is now based on scope, complexity, or measurable impact beyond what's expected at the user's level (e.g., higher-complexity work, cross-team scope, efficiency gains) rather than third-party recognition alone. Recognition by others can still support a rating but is no longer sufficient by itself.
- **Evidence-count rule clarified:** Manually provided evidence — items from the chat request, `context/additional-context.local.md`, or a Drive doc the user referenced — explicitly counts toward the ≥3 threshold on equal footing with Jira/GitHub/Slack evidence, provided each item is a distinct, period-relevant instance with a named artifact or outcome. The per-competency Evidence line now reports a manually-provided count alongside the automatic sources.
- **Customization instructions repointed:** `README.md`, `docs/SETUP.md`, and `METRICS_GUIDE.md` now direct edits to `AGENTS.md` and `.claude/skills/` instead of `.cursorrules` sections, which no longer exist.

### Removed
- **Legacy `.cursorrules` file** (~40 KB), which mirrored the skill content by hand and loaded in full on every request, including ones unrelated to reports. Cursor documents it as legacy and slated for deprecation. Nothing replaces it: `AGENTS.md` covers the always-on rules and the Skills load on demand, so only the small core file is now in context by default.

### Fixed
- **Incorrect IC level ceiling in `docs/SETUP.md`:** the usage callout stated that Technical Writer levels end at L3, and the FAQ referenced "L1, L2, or L3". The IC track runs L1-L4 (as `context/technical-writer-career-path.json`, `README.md`, and the report logic all state); the manager track runs L3-L6. Corrected in all three places.

## v2.5.0 — 2025-12-23

### Added
- **Carryover & Scope Creep Analysis**:
  - Added **Carryover issues** metric: Issues already "In Progress" at period start
  - Added **New issues started** metric: Issues that moved to "In Progress" during the period
  - Added **Scope creep** metric: Issues created and assigned after period start
  - Enhanced completion rate context to include carryover information
  - Low completion rates with high carryover (>30%) now highlight complexity and persistence, not poor performance
  - High scope creep (>40%) flags reactive work patterns and planning opportunities
- **Review-to-Author Ratio (GitHub)**:
  - Added **Review-to-author ratio** metric: PRs reviewed ÷ PRs authored
  - Defined interpretation thresholds:
    - >1.5 = Force Multiplier (L2/L3 trait)
    - 0.8-1.5 = Balanced contribution (L1-L2)
    - <0.5 = Potential siloed work
  - Quantifies peer review contribution, a key but often invisible activity
  - Provides evidence for "Responsibility & Scope" competency
- **Impact vs. Effort Correlation**:
  - Added **Impact vs. Effort flags** to identify outliers:
    - High-priority issues with <50 lines changed
    - Low-priority issues with >1000 lines changed
  - Integrated into Data Processing rules for automatic flagging
  - Surfaces invisible complexity (architecture, research, coordination)
  - Identifies potential over-engineering or misaligned priorities
  - Strengthens "Autonomy & Execution" competency assessment
- **Semantic Blocker Categorization**:
  - Enhanced blocker analysis to use **semantic grouping** from descriptions, comments, and labels
  - Added **Root causes** subsection for each blocker category
  - Added **Recurring impediment patterns** section to identify systemic issues
  - Provides actionable insights into obstacle patterns
  - Enables better mitigation strategies and process improvements
- **Advanced Metrics Interpretation Guide**:
  - Carryover & Scope Creep Context interpretation
  - Review-to-Author Ratio thresholds and meanings
  - Impact vs. Effort flag usage
  - Semantic Blocker Pattern analysis
  - Links metrics to specific competencies
  - Provides coaching-oriented insights, not just numbers

### Changed
- Updated Jira metrics calculations:
  - Carryover: `status = "In Progress" AND updated < period_start`
  - New starts: `first "In Progress" transition within period`
  - Scope creep: `created >= period_start AND assignee = currentUser()`
- Updated GitHub metrics calculations:
  - Review-to-author ratio: `count(reviewed_prs) / count(authored_prs)`, 1 decimal place
  - Impact vs. effort: Compare Jira priority field with PR `additions + deletions`
- Enhanced semantic analysis requirements:
  - Parse issue descriptions, comments, and labels
  - Group by theme using semantic similarity
  - Rank by frequency and impact
  - Provide top 3-5 categories with root causes

### Documentation
- Updated **`.cursorrules`** with all four improvements including detailed calculation rules and interpretation guidance
- Updated **`METRICS_GUIDE.md`** with new metrics definitions, thresholds, calculation rules, and interpretation guidelines; includes comprehensive advanced metrics section
- Updated **`README.md`** with overview and quick start guide
- Updated **`docs/SETUP.md`** with comprehensive setup, usage, and troubleshooting guide
- Updated **`examples/example-report-with-metrics.md`** with all new metrics including semantic blocker analysis

## v2.4.1 — 2025-12-23

### Removed
- **Quality indicators** from work summary reports:
  - Removed first-time-right rate metric
  - Removed issues reopened count
  - Removed multiple status changes tracking
  - Removed feedback items resolved count
  - Simplified metrics to focus on the most useful quantitative indicators (completion rate, resolution time, work volume)
