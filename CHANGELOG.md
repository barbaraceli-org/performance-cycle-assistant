# Changelog

## Unreleased

### Changed
- **Performance analysis report:** Each competency now receives an explicit rating (**Not yet meeting expectations**, **Meets expectations**, **Exceeds expectations**, or **Performing at the next level**) with rationale, supporting evidence, and actionable improvement steps. Replaces per-dimension Strengths/Areas to develop and Summary of alignment sections with an evaluation overview and priority development focus.
- **Evaluation scale expanded to 4 levels:** Renamed the 3-level scale (Need focus / On Track / Outperform) to a 4-level scale (Not yet meeting expectations / Meets expectations / Exceeds expectations / Performing at the next level). The new top level requires evidence matching the next career level's competency expectations (`levels[nextLevel].competencies[key]`) and is not assignable at the top of each track (Technical Writer L4, Technical Writing Manager L6).
- **Exceeds expectations criteria redefined:** The distinction between "Meets expectations" and "Exceeds expectations" is now based on scope, complexity, or measurable impact beyond what's expected at the user's level (e.g., higher-complexity work, cross-team scope, efficiency gains) rather than third-party recognition alone. Recognition by others can still support a rating but is no longer sufficient by itself.

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
