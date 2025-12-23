# Metrics Guide

This guide explains the quantitative metrics automatically included in your work summary reports.

## Overview

The Performance Cycle Report Assistant automatically calculates and includes quantitative metrics from Jira and GitHub (if configured) at three levels:

1. **Overall metrics** - Summary of all work during the review period
2. **Per-quarter metrics** - Breakdown by calendar quarter
3. **Per-work-area metrics** - Detailed metrics for each project/initiative

## Data Sources

- **Jira** (required): Task tracking, issue management, project work
- **GitHub** (optional): Pull requests, code reviews, documentation commits
  - Automatically included if GitHub MCP server is configured
  - See [Setup Guide](docs/SETUP.md) for GitHub configuration

## Metric Definitions

### Jira Metrics

These metrics track your Jira activity:

| Metric | Definition | Why It Matters |
|--------|------------|----------------|
| **Total issues worked on** | Issues that were "In Progress" at any point during the review period, including issues that moved to "In Progress" during the period and issues already "In Progress" at period start (carryover work) | Shows actual work volume - reflects all work that was "on your plate" during the period, regardless of when originally started |
| **Carryover issues** | Issues already "In Progress" at period start | Contextualizes completion rate - high carryover (>30%) indicates inherited complexity or persistent work |
| **New issues started** | Issues that moved to "In Progress" during the period | Shows new work initiated during the period |
| **Scope creep** | Issues created and assigned after period start | Flags reactive work - high scope creep (>40%) may indicate poor planning or shifting priorities |
| **Issues completed** | Issues with status Done/Resolved/Closed during the period | Demonstrates productivity and delivery |
| **Issues in progress** | Issues actively being worked on at period end | Shows current workload and pipeline |
| **Issues blocked/unfinished** | Issues that couldn't be completed | Highlights obstacles and dependencies |
| **Completion rate** | (Completed ÷ Worked on) × 100 | Measures efficiency and delivery success - counts all issues that were actively being worked on during the period (including carryover from before the period) |
| **Average resolution time** | Mean time from "in progress" to completion (days) | Indicates actual work velocity and complexity |
| **Work areas covered** | Number of distinct projects/initiatives | Shows breadth of contribution |

### Issue Type Breakdown

Shows distribution of work by type. Reports automatically display this breakdown in the "Issue type breakdown" section.

**Your Jira instance uses these issue types:**

| Issue Type | Description | Use Case |
|------------|-------------|----------|
| **Epic** | Large initiatives containing multiple issues | Major documentation projects, multi-phase work |
| **New** | New documentation or content creation | Creating new guides, tutorials, API docs |
| **Update** | Updates to existing documentation | Revisions, improvements, content refreshes |
| **Review** | Review and revision work | Content reviews, technical reviews, editorial work |
| **Task** | General work items | Miscellaneous documentation tasks |

**Example output:**
```markdown
**Issue type breakdown:**
- New: 58 (58%)
- Epic: 18 (18%)
- Review: 15 (15%)
- Task: 6 (6%)
- Update: 3 (3%)
```

**Note:** Historical reports may show different issue types (e.g., "Story", "Bug") if your Jira configuration has changed over time. Reports automatically adapt to show whatever types are present in your data - no configuration changes needed.

### Priority Distribution

Shows how work was prioritized:

- **High** - Critical or urgent items
- **Medium** - Standard priority work
- **Low** - Nice-to-have improvements

Helps demonstrate focus on high-impact work.

### GitHub Metrics (Optional)

These metrics track your GitHub contributions when GitHub MCP is configured:

| Metric | Definition | Why It Matters |
|--------|------------|----------------|
| **Pull requests authored** | PRs you created (total, merged, open) | Shows documentation contributions in code repos |
| **Pull requests reviewed** | PRs you reviewed for others | Demonstrates collaboration and technical review skills |
| **Review-to-author ratio** | PRs reviewed ÷ PRs authored | Measures "Force Multiplier" behavior - ratio >1.5 indicates strong team support (key L2/L3 trait); <0.5 may indicate siloed work |
| **Documentation commits** | Commits to .md, docs/, README files | Tracks direct documentation updates |
| **Repositories contributed to** | Distinct repos where you contributed | Shows breadth of impact across projects |
| **Average PR merge time** | Mean time from PR creation to merge (days) | Indicates efficiency and collaboration speed |
| **Lines changed** | Lines added/deleted in documentation files | Shows volume of documentation work |
| **Impact vs. Effort flags** | High-priority issues with low lines changed (<50) or low-priority issues with massive changes (>1000) | Identifies invisible complexity, over-engineering, or misaligned priorities |

**Documentation file types tracked:**
- Markdown files (*.md)
- README files
- API documentation
- Contributing guides
- Documentation site content

**Per-repository breakdown:**
- Contributions by repository
- PR counts per repo
- Documentation files modified per repo

### Per-Quarter Metrics

Each quarter section includes:

- **Issues completed** - Finished during that quarter
- **In progress** - Active at quarter end
- **Completion rate** - Success rate for that quarter

Format: `**Q1 Metrics:** 38 issues completed | 5 in progress | 88% completion rate`

### Per-Work-Area Metrics

Each work area (project/initiative) includes:

- **Completed** - Issues finished in this area
- **In progress** - Issues currently active
- **Avg resolution** - Mean time from "in progress" to completion (days)

Format: `**Metrics:** 15 completed | 2 in progress | Avg resolution: 6 days`

*Note: Work areas with fewer than 3 issues may omit metrics to avoid statistical noise.*

### Unfinished Work Metrics

The "What couldn't be finished" section includes:

| Metric | Definition | Why It Matters |
|--------|------------|----------------|
| **Total unfinished issues** | All non-completed issues | Shows scope of remaining work |
| **In progress** | Count and % of active issues | Indicates work that's moving forward |
| **Blocked** | Count and % of blocked issues | Highlights obstacles needing attention |
| **Average age** | Mean time since creation (days) | Shows how long items have been pending |
| **Primary blockers** | Top 3-5 semantic categories of blockers | Identifies patterns in obstacles based on root causes |

Each unfinished work area shows:
- Count of unfinished issues
- Average age in days

Format: `**Metrics:** 4 unfinished issues | Avg age: 62 days`

### Semantic Blocker Analysis

The blocker analysis uses AI to categorize blockers by **root cause** rather than just status labels. This provides deeper insights into systemic issues.

**How it works:**
- Analyzes issue descriptions, comments, and labels
- Groups blockers by semantic themes (e.g., "Awaiting API Specs", "Review Bottlenecks", "System Downtime")
- Identifies recurring impediment patterns
- Provides root cause analysis and mitigation strategies

**Example blocker categories:**
- **Awaiting API Specifications** - Documentation blocked by missing technical specs
- **Review Bottlenecks** - Work waiting for stakeholder or SME review
- **System Downtime** - Technical infrastructure issues preventing work
- **Resource Constraints** - Insufficient capacity or competing priorities
- **External Dependencies** - Waiting on other teams or vendors

**Recurring impediment patterns** help identify:
- Systemic issues requiring process changes
- Frequent blockers that need proactive mitigation
- Opportunities for process improvement or automation

## How Metrics Are Calculated

### Date Ranges and Inclusion

Issues are included if they had **any activity** during the review period, regardless of when they were created or completed. This reflects "when the task was on your plate."

### Handling Issues Worked on Before the Period

**"Total issues worked on" includes carryover work:**

Issues that were already "In Progress" at the start of the review period (started before the period) are counted in "Total issues worked on" if they:
- Were completed during the period, OR
- Remained "In Progress" during the period (even if not completed)

This ensures the metric reflects all work that was actively being worked on during the period, not just newly started work.

**Example:**
- Period: January 2025
- 12 issues moved to "In Progress" during January (new starts)
- 8 issues were already "In Progress" at January 1st and were completed during January (carryover)
- **Total issues worked on:** 20 (12 + 8)
- **Carryover issues:** 8 (40% of worked on)
- **New issues started:** 12
- **Issues completed:** 20
- **Completion rate:** 100% (20/20)

This approach provides a fairer view of your productivity by including all work that was "on your plate" during the period, regardless of when it was originally started.

### Understanding Carryover and Scope Creep

**Carryover issues** help contextualize your completion rate:
- **High carryover (>30%)** + low completion rate → Indicates complex, persistent work or inherited workload, not poor performance
- **Low carryover** + high completion rate → Demonstrates strong planning and execution

**Scope creep** identifies reactive vs. planned work:
- **High scope creep (>40%)** → Indicates reactive work patterns, shifting priorities, or poor planning
- **Low scope creep** → Demonstrates good planning and scope management

**Example:**
- Period: Q1 2025
- Total worked on: 50 issues
- Carryover: 20 issues (40% - high)
- New starts: 30 issues
- Scope creep: 15 issues (50% of new starts - high)
- Completed: 35 issues (70% completion rate)

**Interpretation:** The 70% completion rate is reasonable given high carryover (complex inherited work) and high scope creep (reactive additions). Focus analysis on managing scope and addressing persistent blockers.

### Calculation Rules

- **Percentages**: Rounded to nearest whole number (e.g., 83%)
- **Days**: Rounded to 1 decimal place (e.g., 8.5 days)
- **Missing data**: Shows "N/A" or omits metric if calculation not possible
- **Zero values**: Shown explicitly as "0" rather than omitted
- **Rounding**: Conservative (round down for rates, round up for time)

### GitHub-Specific Calculation Rules

- **PR merge time**: Calculated only for merged PRs (created_at to merged_at)
- **Documentation commits**: Filtered by file patterns (*.md, docs/**, README*, CONTRIBUTING*, etc.)
- **Lines changed**: Sum of additions and deletions in documentation files only
- **Repository counting**: Each distinct repository where you had activity counts once
- **Review counting**: Includes all review types (approve, request changes, comment); excludes self-authored PRs
- **Review-to-author ratio**: (PRs reviewed ÷ PRs authored), rounded to 1 decimal place
- **Date filtering**: GitHub activities included if they occurred during the review period
- **Impact vs. Effort analysis**: Correlates Jira priority with GitHub lines changed to identify outliers

### Average Resolution Time Calculation

**Important:** Average resolution time measures **actual work time**, not total issue lifetime.

**Formula:** `avg(resolutiondate - in_progress_date)` for completed issues

**How "in progress" date is determined:**
1. **Primary method**: Extract from Jira changelog the first time the issue status changed to "In Progress" (or "In Review", "In Development")
2. **Fallback method**: If changelog is unavailable, use the `updated` date when the issue status is "In Progress"

**Why this matters:**
- More accurately reflects your actual work velocity
- Excludes time when issues were waiting in backlog or not yet assigned
- Aligns with the principle that metrics should reflect "when the task was on your plate"
- Provides fairer comparison across different work types and dependencies

**Example:**
- Issue created: 2025-01-01
- Moved to "In Progress": 2025-01-15 (14 days in backlog)
- Completed: 2025-01-25
- **Resolution time**: 10 days (not 24 days)

### Status Mapping

**Completed statuses**: Done, Resolved, Closed, Completed

**In Progress statuses**: In Progress, In Review, In Development, In Testing

**Blocked statuses**: Blocked, On Hold, Waiting, Impediment

**Unfinished statuses**: Backlog, To Do, Open (at period end)

## Using Metrics in Performance Reviews

### What Metrics Show

✅ **Good indicators:**
- Volume and consistency of work (Jira + GitHub)
- Efficiency and velocity
- Breadth of contribution across systems
- Ability to complete work
- Focus on priorities
- Collaboration through code reviews (GitHub)
- Technical engagement with repositories (GitHub)

❌ **What metrics DON'T show:**
- Quality of work (requires qualitative review)
- Complexity or difficulty
- Strategic impact
- Communication effectiveness
- Learning and growth
- Impact of mentoring or leadership

### Interpreting Your Metrics

**High completion rate (>80%)**: Demonstrates strong execution and realistic scope management

**Lower completion rate (<70%)**: May indicate scope creep, dependencies, or overly ambitious planning (not necessarily negative). Check carryover and scope creep metrics for context.

**High carryover (>30%)**: Indicates complex inherited work or persistent issues. Contextualizes lower completion rates as complexity, not poor performance.

**High scope creep (>40%)**: Suggests reactive work patterns or shifting priorities. May indicate need for better planning or scope protection.

**Fast average resolution (<5 days)**: Shows efficiency, may indicate simpler tasks or strong expertise. Note: This measures time from "in progress" to completion, not total issue lifetime.

**Slow average resolution (>15 days)**: May indicate complex work, dependencies, or need for process improvement. Remember: This reflects actual work time, so longer times may indicate genuinely complex tasks.

**Many blocked issues**: Highlights need for stakeholder engagement or dependency management

**High work area count**: Demonstrates versatility and broad contribution

**Low work area count**: May indicate deep focus and specialization (not necessarily negative)

**High PR count (GitHub)**: Shows active engagement with code repositories and developer workflows

**High review count (GitHub)**: Demonstrates collaboration, technical expertise, and team support

**Review-to-author ratio >1.5 (GitHub)**: "Force Multiplier" behavior - unblocking others more than creating own work. Key trait for L2/L3 Technical Writers.

**Review-to-author ratio <0.5 (GitHub)**: May indicate siloed work or limited team collaboration. Development area for Communication competency.

**Fast PR merge time (GitHub)**: Indicates efficient collaboration and clear documentation

**Many repositories (GitHub)**: Shows broad impact across multiple projects or teams

**Impact vs. Effort flags**: High-priority issues with low output may indicate invisible complexity (architecture, research); low-priority issues with massive output may indicate over-engineering.


## Example Interpretation

```
**Q1 Metrics:** 38 issues completed | 5 in progress | 88% completion rate
```

**What this tells you:**
- Strong delivery (88% completion)
- Healthy pipeline (5 in progress)
- Consistent productivity (38 completed in 3 months ≈ 3 per week)

```
**Metrics:** 15 completed | 2 in progress | Avg resolution: 6 days
```

**What this tells you:**
- Efficient execution (6-day average)
- Active work area (2 in progress)
- Significant contribution (15 issues)

```
**Unfinished work metrics:**
- **Total unfinished issues:** 15
- **In progress:** 11 (73%)
- **Blocked:** 4 (27%)
- **Average age:** 45 days
```

**What this tells you:**
- Most unfinished work is actively progressing (73%)
- Small number of blockers (4 issues)
- Some items have been pending for a while (45 days average)
- May need to address blockers or re-prioritize old items

## Best Practices

### Do:
- ✅ Use metrics to support your accomplishment narratives
- ✅ Explain context when metrics seem unusual
- ✅ Highlight trends across quarters
- ✅ Reference metrics when discussing efficiency or scope
- ✅ Use blocker analysis to identify process improvements

### Don't:
- ❌ Rely solely on metrics without qualitative context
- ❌ Compare your metrics directly to others (different work types)
- ❌ Treat high volume as automatically better than low volume
- ❌ Ignore low completion rates without investigation
- ❌ Use metrics to justify poor quality work

## Questions?

If you have questions about:
- **How metrics are calculated**: See "How Metrics Are Calculated" above
- **What metrics mean**: See "Metric Definitions" above
- **How to interpret your results**: See "Using Metrics in Performance Reviews" above
- **Advanced metrics deep dive**: See "Advanced Metrics Deep Dive" section above
- **Quick metric lookup**: See "Advanced Metrics Quick Reference" section above
- **Customizing metrics**: Modify `.cursorrules` section 4.2

---

## Advanced Metrics Deep Dive

The following advanced metrics were introduced in December 2025 to provide deeper insights into performance patterns and work dynamics.

### 1. Carryover & Scope Creep Analysis

**Purpose:** Distinguish between planned work, inherited work, and reactive additions to provide fair context for completion rates.

#### Carryover Issues
**Definition:** Issues already "In Progress" at the start of the review period.

**Why it matters:** High carryover contextualizes lower completion rates as complexity or inherited workload, not poor performance.

**Thresholds:**
- **Low (<20%):** Most work is newly started during the period
- **Moderate (20-30%):** Balanced mix of new and ongoing work
- **High (>30%):** Significant inherited complexity or persistent work

**Interpretation:**
- High carryover + low completion rate → Emphasize complexity and persistence
- Low carryover + high completion rate → Strong planning and execution

#### Scope Creep
**Definition:** Issues created AND assigned to the user after the period start date.

**Why it matters:** Flags reactive work patterns and planning challenges.

**Thresholds:**
- **Low (<20%):** Strong scope protection and planning
- **Moderate (20-40%):** Normal reactive work
- **High (>40%):** Significant reactive work or poor planning

**Interpretation:**
- High scope creep + low completion rate → Highlight reactive work patterns
- Low scope creep + high completion rate → Strong planning skills

**Example:**
```markdown
**Period:** Q1 2025
**Total worked on:** 50 issues
**Carryover:** 20 issues (40% - high)
**New starts:** 30 issues
**Scope creep:** 15 issues (50% of new starts - high)
**Completed:** 35 issues (70% completion rate)

**Interpretation:** The 70% completion rate is reasonable given:
- High carryover (40%) indicates inherited complex work
- High scope creep (50%) shows significant reactive additions
- Focus analysis on managing scope and addressing persistent blockers
```

---

### 2. Review-to-Author Ratio

**Purpose:** Measure "Force Multiplier" behavior—how much a Technical Writer unblocks others through peer reviews.

**Definition:** (PRs reviewed ÷ PRs authored)

**Why it matters:** Quantifies peer review contribution, a key but often invisible activity. High ratios indicate team support and collaboration skills.

**Thresholds:**

| Ratio | Interpretation | Level Alignment |
|-------|---------------|-----------------|
| **>1.5** | Force Multiplier - unblocking others more than creating own work | L2/L3 "Responsibility & Scope" |
| **0.8-1.5** | Balanced contribution - appropriate mix of creation and review | L1-L2 |
| **<0.5** | Potential siloed work or limited team collaboration | Development area |

**Competency Mapping:**

**High ratio (>1.5) provides evidence for:**
- **Responsibility & Scope** - Supporting team success beyond individual contributions
- **Communication** - Active engagement with team members
- **Technical Writing** - Providing technical review and guidance

**Low ratio (<0.5) may indicate development needs in:**
- **Communication** - Limited peer interaction
- **Responsibility & Scope** - Focus on individual work over team support

**Example:**
```markdown
**Review-to-author ratio:** 2.3:1 (46 reviews / 20 authored PRs)

**Interpretation:** Strong "Force Multiplier" behavior. Reviewed more than twice as many PRs as authored, demonstrating:
- Active team support and unblocking
- Technical expertise applied to peer review
- L2/L3 trait: Responsibility beyond individual contributions

**Competency evidence:**
- Responsibility & Scope: Supporting team velocity through reviews
- Communication: Active engagement across 8 repositories
- Technical Writing: Providing technical guidance to peers
```

---

### 3. Impact vs. Effort Correlation

**Purpose:** Avoid vanity metrics by correlating Jira priority with actual GitHub output to identify invisible complexity or misaligned priorities.

**How It Works:**

The system automatically flags outliers:

1. **High-priority Jira issues with low lines changed (<50 lines)**
   - Potential invisible complexity (architecture, research, coordination)
   - Blocked work with minimal output
   - Strategic work not reflected in code changes

2. **Low-priority Jira issues with massive changes (>1000 lines)**
   - Potential over-engineering
   - Misaligned priorities (low-priority work consuming high effort)
   - Scope creep within individual issues

**Interpretation Guidelines:**

#### High Priority + Low Output
**Investigate for:**
- Architecture and design work (invisible in line counts)
- Research and discovery activities
- Cross-team coordination and alignment
- Blocked work with minimal progress
- Strategic planning and documentation

**Performance analysis:**
- Not necessarily negative—may indicate high-value strategic work
- Look for evidence of complexity in issue descriptions
- Consider mentioning invisible work in accomplishments

#### Low Priority + High Output
**Investigate for:**
- Over-engineering simple tasks
- Misaligned priority labels
- Scope creep within issues
- Inefficient approaches

**Performance analysis:**
- May indicate need for better prioritization
- Could suggest efficiency improvements
- Opportunity to align effort with business impact

**Example:**
```markdown
**Impact vs. Effort Flags:**

**High-priority, low output:**
- EDU-234 (High priority, 23 lines): API documentation architecture - extensive research and cross-team alignment, minimal code changes
- EDU-456 (High priority, 15 lines): Documentation strategy proposal - strategic planning work

**Analysis:** High-priority work with low line counts reflects strategic and coordination activities. This is valuable invisible complexity, not poor execution.

**Low-priority, high output:**
- EDU-789 (Low priority, 1,245 lines): README updates across 15 repositories

**Analysis:** Low-priority work consumed significant effort. Consider whether priority label accurately reflects scope, or if work could be more efficient.
```

**Use in Performance Analysis:**

**Autonomy & Execution competency:**
- Effective prioritization: High-priority work receives appropriate attention
- Efficiency: Effort aligns with priority and business impact
- Judgment: Recognizing when to invest deeply vs. move quickly

---

### 4. Semantic Blocker Categorization

**Purpose:** Move beyond status labels to understand root causes of blockers using AI-powered semantic analysis.

**How It Works:**

Instead of just listing "Blocked" issues, the system:

1. **Analyzes issue descriptions, comments, and labels**
2. **Groups blockers by semantic theme** (e.g., "Awaiting API Specs", "Review Bottlenecks")
3. **Identifies root causes** from issue content
4. **Ranks by frequency and impact**
5. **Provides top 3-5 categories with mitigation strategies**

**Common Categories:**

| Category | Description | Typical Root Causes |
|----------|-------------|---------------------|
| **Awaiting API Specifications** | Documentation blocked by missing technical specs | Engineering prioritizing features over specs; incomplete API design; lack of API-first process |
| **Stakeholder Review Bottlenecks** | Work waiting for SME or stakeholder review | Competing priorities; unclear review responsibilities; no review SLAs |
| **System Downtime** | Technical infrastructure preventing work | Deployment issues; environment unavailable; tooling failures |
| **Resource Constraints** | Insufficient capacity or competing priorities | Team capacity limitations; competing high-priority projects; insufficient staffing |
| **External Dependencies** | Waiting on other teams or vendors | Cross-team coordination delays; vendor delays; external approvals |

**Output Format:**
```markdown
### Blocker Analysis

**Blocker categories (semantically grouped from descriptions, comments, and labels):**

- **Awaiting API Specifications:** 6 issues | Avg resolution: 45 days
  - *Root causes:* Engineering teams prioritizing feature development over documentation specs; incomplete API design before documentation requests; lack of API-first development process
  - *Mitigation:* Establish regular check-ins with engineering teams, request API specs earlier in development cycle, create documentation templates that prompt for required technical details

- **Stakeholder Review Bottlenecks:** 5 issues | Avg resolution: 38 days
  - *Root causes:* Subject matter experts with competing priorities; unclear review responsibilities; lack of review SLAs
  - *Mitigation:* Escalate decision requests to stakeholders, provide decision frameworks with pros/cons, set explicit review deadlines, identify backup reviewers

**Recurring impediment patterns:**
- **Awaiting API Specifications** appears in 40% of blocked issues, indicating need for earlier technical writer involvement in API design process
- **Review bottlenecks** consistently delay work by 30+ days, suggesting need for review SLAs and backup reviewer assignments
```

**Use in Performance Analysis:**

**Responsibility & Scope competency:**
- **Recurring blockers** → Assess ability to escalate and resolve systemic issues
- **Diverse blockers** → Context-dependent challenges requiring adaptability
- **Mitigation strategies** → Proactive problem-solving and process improvement

**Areas to develop:**
- Identify patterns in blocker categories
- Provide actionable feedback on obstacle management
- Suggest process improvements to prevent recurring blockers

**Benefits:**
1. **Deeper insights** - Understand WHY work is blocked, not just THAT it's blocked
2. **Actionable feedback** - Specific mitigation strategies based on root causes
3. **Pattern recognition** - Identify systemic issues requiring process changes
4. **Fair assessment** - Context for lower completion rates due to external factors

---

## Using Advanced Metrics in Performance Reviews

### Integration with Competency Analysis

The advanced metrics enhance competency assessment:

#### Abstraction & Modeling
- **Impact vs. Effort flags** → Evidence of strategic thinking and complexity management

#### Responsibility & Scope
- **Review-to-author ratio** → Team support and "Force Multiplier" behavior
- **Semantic blocker patterns** → Ability to escalate and resolve systemic issues

#### Autonomy & Execution
- **Carryover & scope creep** → Planning and scope management skills
- **Impact vs. Effort flags** → Prioritization and efficiency

#### Communication
- **Review-to-author ratio** → Peer collaboration and engagement
- **Semantic blocker analysis** → Stakeholder management and escalation

**Example Performance Analysis:**
```markdown
# Autonomy & Execution

**Evidence:** 87 Jira issues, 45 GitHub PRs

## Strengths
- Demonstrated strong planning with low scope creep (28% vs. 40% threshold), indicating effective scope protection and proactive work management
- Maintained 83% completion rate despite high carryover (26%), showing persistence on complex inherited work
- Efficient execution with 8.5-day average resolution time, balancing speed with quality
- Review-to-author ratio of 0.5:1 shows balanced focus on individual contributions

## Areas to develop
- High carryover (26%) suggests opportunities to reduce work-in-progress and increase throughput by completing more issues before starting new ones
- Impact vs. Effort analysis flagged 3 low-priority issues with >1000 lines changed, indicating potential over-engineering or misaligned priorities
- Semantic blocker analysis shows recurring "Awaiting API Specifications" pattern (40% of blocked issues), suggesting need for earlier involvement in API design process
```

---

## Advanced Metrics Quick Reference

### Carryover & Scope Creep

| Metric | Formula | Good | Moderate | High | Interpretation |
|--------|---------|------|----------|------|----------------|
| **Carryover %** | (carryover / worked on) × 100 | <20% | 20-30% | >30% | High = inherited complexity, contextualizes low completion rates |
| **Scope Creep %** | (scope creep / new starts) × 100 | <20% | 20-40% | >40% | High = reactive work, poor planning |

**Quick interpretation:**
- High carryover + low completion = Complexity, not poor performance
- High scope creep + low completion = Reactive work patterns

### Review-to-Author Ratio

| Ratio | Interpretation | Level | Action |
|-------|---------------|-------|--------|
| **>1.5** | Force Multiplier | L2/L3 trait | Highlight in "Responsibility & Scope" |
| **0.8-1.5** | Balanced | L1-L2 | Appropriate contribution mix |
| **<0.5** | Siloed work | Development area | Increase peer review participation |

**Formula:** PRs reviewed ÷ PRs authored

**Competency evidence:**
- High ratio → Responsibility & Scope, Communication
- Low ratio → Development area for Communication

### Impact vs. Effort Flags

| Pattern | Lines Changed | Investigation | Likely Cause |
|---------|--------------|---------------|--------------|
| **High priority** | <50 | Invisible complexity? | Architecture, research, coordination |
| **Low priority** | >1000 | Over-engineering? | Misaligned priorities, scope creep |

**Use in analysis:**
- Autonomy & Execution competency
- Prioritization and efficiency assessment

### Semantic Blocker Categories

**Common categories:**
- Awaiting API Specifications
- Stakeholder Review Bottlenecks
- System Downtime
- Resource Constraints
- External Dependencies

**Analysis includes:**
- Root causes from issue content
- Mitigation strategies
- Recurring impediment patterns

**Use in analysis:**
- Responsibility & Scope (escalation/resolution ability)
- Areas to develop (obstacle management)

---

## Best Practices for Advanced Metrics

### For Report Generation

1. **Trust the automation** - Advanced metrics are calculated automatically from your Jira and GitHub data
2. **Provide context** - Explain unusual patterns (e.g., high carryover due to team transition)
3. **Use for coaching** - Frame metrics as insights, not judgments
4. **Look for patterns** - Single-period anomalies matter less than trends over time

### For Interpretation

1. **Consider context** - High carryover may indicate complexity, not poor performance
2. **Look holistically** - No single metric tells the full story
3. **Focus on growth** - Use metrics to identify development opportunities
4. **Celebrate strengths** - High review ratios and effective blocker management are achievements

### For Career Development

1. **Track over time** - Compare metrics across quarters to show growth
2. **Set goals** - Use thresholds to set improvement targets (e.g., increase review-to-author ratio)
3. **Link to competencies** - Connect metrics to specific competency areas for promotion cases
4. **Document invisible work** - Use Impact vs. Effort flags to highlight strategic contributions

---

## Frequently Asked Questions

**Q: What if my review-to-author ratio is low?**  
A: Low ratios (<0.5) may indicate siloed work. Consider increasing peer review participation to develop collaboration skills and support team velocity.

**Q: Is high carryover bad?**  
A: Not necessarily. High carryover (>30%) may indicate complex, persistent work or inherited workload. Context matters—look at completion rates and blocker patterns.

**Q: How do I improve my scope creep metric?**  
A: High scope creep (>40%) suggests reactive work. Strategies: better sprint planning, scope protection, stakeholder alignment, saying "no" to low-priority additions.

**Q: What if Impact vs. Effort flags many of my issues?**  
A: Investigate each flag. High-priority/low-output may be strategic work (good). Low-priority/high-output may indicate over-engineering (development area).

**Q: How accurate is semantic blocker categorization?**  
A: The AI analyzes issue descriptions, comments, and labels to identify themes. Accuracy improves with detailed issue documentation. Review and provide context if categories seem off.

**Q: Can I customize the thresholds?**  
A: Yes! Edit `.cursorrules` to adjust thresholds for carryover, scope creep, and review ratios based on your team's norms.

---

## Technical Implementation Details

### Data Sources

- **Jira**: Issue status history (changelog), creation dates, assignment dates, resolution dates
- **GitHub**: PR authorship, review activity, lines changed, file types
- **AI Analysis**: Issue descriptions, comments, labels for semantic blocker categorization

### Calculation Methods

**Carryover:**
```
Issues with status = "In Progress" AND updated < period_start_date
Carryover % = (carryover_issues / total_worked_on) × 100
```

**Scope Creep:**
```
Issues with created >= period_start_date AND assignee = currentUser()
Scope creep % = (scope_creep_issues / new_issues_started) × 100
```

**Review-to-Author Ratio:**
```
PRs reviewed (excluding self-authored) / PRs authored
Rounded to 1 decimal place
```

**Impact vs. Effort:**
```
High-priority issues with (additions + deletions) < 50
Low-priority issues with (additions + deletions) > 1000
```

**Semantic Blocker Analysis:**
```
1. Extract issue descriptions, comments, labels for blocked issues
2. Use AI to identify semantic themes
3. Group by similarity
4. Rank by frequency and impact
5. Generate root cause analysis
```

---

*Last updated: December 2025*
