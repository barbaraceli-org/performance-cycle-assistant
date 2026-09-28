---
name: generate-performance-analysis
description: Generate the Performance Analysis report for a performance cycle — one competency-scale evaluation per competency, with supporting evidence and actionable steps, for a Technical Writer or Technical Writing Manager, using the VTEX career-path frameworks. Use when the user asks for a performance analysis, competency evaluation, or performance-cycle report and names a date range, role, and level.
---

# Generate Performance Analysis

Link every competency bullet to a specific Jira issue, GitHub PR, Slack thread, or Drive doc.

## Steps

0. This report is generated together with `generate-work-summary` by default for any "generate my performance cycle report" style request — don't ask the user which report(s) they want unless they explicitly asked for only one.
1. Read `../_shared/data-collection.md` in full and follow it exactly. Run its "Connection Validation" section first, unabridged: **both** the Atlassian MCP and the GitHub connection are mandatory, and a failure of either one stops the run before any data retrieval. Then check `context/additional-context.local.md` (step 0 there, filtered to the requested period), retrieve/process Jira/GitHub/Slack/Drive data, and load the correct career-path JSON for the user's role.
2. Read `../_shared/writing-standards.md` for tone, evidence-linking, and output-location rules.
3. Rate every competency using the evaluation scale below, build the report using the structure and rules below.
4. Save to `reports/[YYYY]/performance-analysis-[date-range].md` (year folder per `writing-standards.md`).

## Evidence Threshold (compute before rating anything)

The number of distinct instances a competency needs — written as **[[threshold]]** throughout this file — depends on how long the period is. A fixed bar of 3 is right for a full cycle and mechanically rebases every competency to the bottom of the scale on a short one, which is a false negative, not a finding.

| Period length | [[threshold]] |
| --- | --- |
| 90 days or more (quarter, semester, year) | 3 distinct instances |
| 28 to 89 days (one or two months) | 2 distinct instances |
| Under 28 days | Not eligible — see below |

**Periods under 28 days are too short for a competency evaluation.** Don't generate the report with everything at the bottom of the scale. Tell the user the period is too short to evidence competencies, suggest a brag doc (`generate-brag-doc`) for that period or a longer range for the analysis, and stop. This applies to the Performance Analysis only; the Work Summary has no minimum period.

## Competency Evaluation Scale (MANDATORY per competency)

Assign **exactly one** rating per competency using this scale:

| # | Evaluation | When to assign |
| --- | --- | --- |
| 1 | **Not yet meeting expectations** | Has some period-relevant evidence, but fewer than [[threshold]] distinct instances — or enough instances that nonetheless fall short of the scope/complexity expected at the user's role/level |
| 2 | **Meets expectations** | Has consistent examples (≥[[threshold]] distinct, period-relevant instances) of how this ability was achieved at the scope/complexity appropriate for the user's role/level |
| 3 | **Exceeds expectations** | Has consistent examples (≥[[threshold]]) **and** the work shows scope, complexity, or impact beyond what's typically expected at the user's level (e.g., handling higher-complexity issues than peers at that level, taking on cross-team or cross-repo scope, driving measurable efficiency/quality gains, disproportionate peer impact such as review-to-author ratio >1.5, resolving or preventing systemic blockers for others). Third-party recognition (explicit praise, mentorship attribution, leadership callouts) can support the rating but is not sufficient on its own — it must be backed by a concrete scope/impact signal |
| 4 | **Performing at the next level** | Meets the bar for "Exceeds expectations" **and** the evidence demonstrates competencies matching the next career level's expectations (compare against `levels[nextLevel].competencies[key]` in the career-path JSON, when a next level exists) |

Display each rating as: **`Evaluation: [Not yet meeting expectations | Meets expectations | Exceeds expectations | Performing at the next level | Insufficient evidence for this period]`** (include the number in parentheses on first mention in the report intro table only, if used). If the user is already at the top level of their track (Technical Writer L4 or Technical Writing Manager L6), "Performing at the next level" is not assignable — cap ratings at "Exceeds expectations" and note the level ceiling in the rationale when relevant.

### Insufficient evidence (not a rating)

A competency with **zero** period-relevant evidence gets `Evaluation: Insufficient evidence for this period` instead of a number on the scale. This is deliberately distinct from "Not yet meeting expectations": one says the data can't support a conclusion, the other says the work didn't demonstrate the ability. Sparse-but-present evidence (at least one instance, below [[threshold]]) is always "Not yet meeting expectations" with "Limited evidence" on the Evidence line — never "Insufficient evidence".

- Write the rationale as what was looked for and where, not as a judgment: name the sources searched and state that nothing in the period maps to this competency.
- Actionable steps still apply, and they address how to generate traceable examples next period.
- Competencies in this state are excluded from the dimension roll-up (see below) and are shown as `Insufficient evidence for this period` in the overview tables.
- **If more than half the competencies land here**, the period didn't produce enough traceable work to evaluate. Say so in one sentence directly under the report's Evaluation scale table, before the first dimension, so the reader doesn't mistake thin data for weak performance.

## Structure

```markdown
# Performance analysis

This analysis is based on the **VTEX Technical Writer Career Path**, using the **Technical Writer** framework for IC roles and **Technical Writing Manager** framework for manager roles. The manager track includes an additional **Management** competency.

## Evaluation scale

| # | Evaluation | Description |
| --- | --- | --- |
| 1 | Not yet meeting expectations | Doesn't have consistent examples on how this ability was achieved |
| 2 | Meets expectations | Has consistent examples on how this ability was achieved at the scope/complexity expected for the level |
| 3 | Exceeds expectations | Has consistent examples and the work shows scope, complexity, or impact beyond what's expected at that level |
| 4 | Performing at the next level | Exceeds expectations at the current level and shows evidence matching the next career level's expectations |
| — | Insufficient evidence for this period | No period-relevant evidence was found for this competency, so no rating is assigned |

**Technical Writer (IC):** One `#` section per entry in `dimensions[]` (use `label` as heading). Each dimension section MUST open with a dimension-level evaluation (rating + rationale) rolled up from its competencies, then contain one `##` subsection per granular competency key listed in `dimensions[].competencies`. Compare against `levels[userLevel].competencies[key]`.

**Technical Writing Manager:** One `#` section per competency key in the manager framework (each key is already dimension-level; no nested `##` unless the JSON defines sub-keys). The manager framework has no separate dimension layer, so no dimension-level roll-up is produced.

# [Dimension label — IC only; omit extra heading for manager if section title equals competency]

**Dimension evaluation:** [Not yet meeting expectations | Meets expectations | Exceeds expectations | Performing at the next level | Insufficient evidence for this period]

**Rationale:** [2-4 sentences applying the lowest-competency roll-up rule below; name each competency's rating, say which one set the dimension's rating, and give the standout signal or the gap to close first]

## [competency_key — human-readable label, e.g., Pattern recognition]

**Evaluation:** [Not yet meeting expectations | Meets expectations | Exceeds expectations | Performing at the next level | Insufficient evidence for this period]

**Rationale:** [2-4 sentences: why this rating was assigned against the scale above; cite evidence count and, for Exceeds expectations or higher, the specific scope/complexity/impact signal that goes beyond the expected level]

**Evidence:** X Jira issues, Y GitHub PRs, Z Slack threads, W user-provided Drive docs, V manually provided items (omit any source with zero count or that is not connected; Drive count only reflects docs the user explicitly referenced; show "Limited evidence" if the total is below [[threshold]]); if rated **Performing at the next level**, also cite which next-level competency expectations were matched

### Supporting evidence
- [3-5 bullets with Jira keys/PR numbers when applicable; for Not yet meeting expectations, include what exists even if sparse]

### Actionable steps to improve
- [3-5 concrete, prioritized actions tied to this competency and rating — see rules below]

[Repeat `## [competency]` for each competency in the dimension]

# Summary

## Dimension evaluation overview

[IC only; omit for the Technical Writing Manager framework, which has no dimension layer]

| Dimension | Evaluation |
| --- | --- |
| [dimension label] | [Not yet meeting expectations | Meets expectations | Exceeds expectations | Performing at the next level | Insufficient evidence for this period] |

## Competency evaluation overview

| Competency | Evaluation |
| --- | --- |
| [key or label] | [Not yet meeting expectations | Meets expectations | Exceeds expectations | Performing at the next level | Insufficient evidence for this period] |

## Cross-cutting themes
- [Patterns across competencies: strongest areas, systemic gaps, metrics that explain multiple ratings]

## Priority development focus
- [Top 3 competencies rated Not yet meeting expectations or Meets expectations with highest impact on level expectations, with 1-sentence why each matters now]
```

## Competency Analysis Rules

- **One evaluation per competency** — never leave a competency blank: every one carries either a rating from the scale or "Insufficient evidence for this period"
- **Dimension roll-up (IC only) — the lowest competency sets the dimension:** After rating every competency in a dimension, the dimension's rating is **the lowest rating among its competencies**. No weighing, no compensation, no judgment call: one below-bar competency puts the whole dimension at that level, and a single standout never lifts it. Run the rule mechanically, then write the rationale.
  - This makes the upper ratings self-enforcing: "Exceeds expectations" requires *every* competency at "Exceeds" or above, and "Performing at the next level" requires *every* competency at that level. A dimension holding one "Meets" and one "Exceeds" is "Meets expectations"; one holding one "Meets" and one "Not yet meeting expectations" is "Not yet meeting expectations".
  - **Competencies rated "Insufficient evidence for this period" are skipped**, and the dimension rolls up from the remaining ones. If *every* competency in the dimension is in that state, the dimension is `Insufficient evidence for this period` too.
  - In the dimension rationale, name each competency's rating, identify which one set the dimension's rating, and call out the standout signal (for higher ratings) or the gap to close first (for lower ratings).
- **Supporting evidence:** 3-5 bullets per competency; link Jira keys/PR numbers in parentheses
- **Actionable steps to improve (required per competency):**
  - **Insufficient evidence for this period → any rating:** Steps to make the work traceable next period (which issue types to own, where to record non-Jira work, what to log in `context/additional-context.local.md`) — the problem to solve is visibility, so don't prescribe behaviour change as if the ability were missing
  - **Not yet meeting expectations → Meets expectations:** Steps to build consistent evidence (specific behaviors, artifact types, cadence, example issue types to own)
  - **Meets expectations → Exceeds expectations:** Steps to expand scope, complexity, or measurable impact beyond the current level (own higher-complexity issues, take on cross-team/cross-repo work, drive efficiency or quality gains, help unblock others) — not just steps to get noticed
  - **Exceeds expectations → Performing at the next level:** Steps to close the gap with next-level competency expectations (scope expansion, cross-team influence, ownership of next-level artifact types — cite the specific gap from `levels[nextLevel].competencies[key]`)
  - **Performing at the next level:** Steps to sustain and multiply impact (mentoring others, codifying practices, expanding scope, preparing a promotion case)
  - Each step must be **specific and doable** in the next review period — avoid generic advice ("communicate better")
- **Neutral tone:** Coaching-oriented, not judgmental or flattering
- **Evidence tracking:** Count distinct examples per competency against [[threshold]] (see "Evidence Threshold" above); zero examples → **Insufficient evidence for this period**; at least one but fewer than [[threshold]] → maximum rating is **Not yet meeting expectations**; ≥[[threshold]] at expected scope/complexity → **Meets expectations**; ≥[[threshold]] with a concrete scope/complexity/impact signal beyond the expected level (recognition alone does not qualify) → **Exceeds expectations**; ≥[[threshold]] meeting the "Exceeds expectations" bar **and** evidence matching next-level competency expectations → **Performing at the next level**. Manually provided evidence — items from the chat request, `context/additional-context.local.md`, or a Drive doc the user referenced — **does count toward [[threshold]]** on equal footing with Jira/GitHub/Slack evidence, provided each item is a distinct, period-relevant instance with a named artifact or outcome; a single doc or activity cited under several competencies still counts once per competency, and vague claims without an artifact or outcome ("mentored the team") don't count at all.
- **Limited evidence:** If the competency has at least one data point but fewer than [[threshold]], assign **Not yet meeting expectations** and state "Limited evidence" in the Evidence line; actionable steps must address how to generate traceable examples in Jira/GitHub. With zero data points, use "Insufficient evidence for this period" instead (see the scale section)
- **Top-level ceiling:** For Technical Writer L4 or Technical Writing Manager L6 (no next level defined in the career-path JSON), do not assign "Performing at the next level"; cap at "Exceeds expectations"
- **Missing level expectation:** If `levels[userLevel].competencies[key]` is an empty string in the career-path JSON, there is no bar to evaluate against at that level. Inherit the description from the nearest lower level that defines one, rate against that, and say so in the first sentence of the rationale (e.g., "L4 defines no expectation for this competency; rated against the L3 description."). Never invent an expectation and never skip the competency

## Advanced Metrics Interpretation

**Carryover & Scope Creep Context:**
- High carryover (>30%) + low completion rate → Emphasize complexity and persistence, not poor performance
- High scope creep (>40%) + low completion rate → Highlight reactive work patterns, suggest proactive planning
- Low carryover + high completion rate → Strong execution and planning skills

**Review-to-Author Ratio (Technical Writers):**
- Ratio >1.5 → "Force Multiplier" behavior, evidence for L2/L3 "Responsibility & Scope" competency
- Ratio 0.8-1.5 → Balanced contribution, appropriate for L1-L2
- Ratio <0.5 → Potential siloed work, development area for "Communication" and "Collaboration"

**Impact vs. Effort Flags:**
- High-priority + low lines changed → Investigate for invisible complexity (architecture, research, coordination)
- Low-priority + high lines changed → Flag for over-engineering or scope misalignment
- Use in "Autonomy & Execution" analysis to assess prioritization and efficiency

**Semantic Blocker Patterns:**
- Recurring blockers → Systemic issues, evidence for "Responsibility & Scope" (ability to escalate/resolve)
- Diverse blockers → Context-dependent challenges, assess mitigation strategies
- Use in competency rationales and actionable steps when blockers limited demonstrated ability

**Slack Thread-Help Ratio (Technical Writers, only if Slack connected):**
- Ratio (threads helped/answered ÷ threads started) >1.5 → "Force Multiplier" behavior via async support, evidence for "Communication" and "Collaboration" competencies, similar in spirit to the GitHub review-to-author ratio
- Ratio <0.5 with GitHub review-to-author ratio also <0.5 → Compounding signal of siloed work; flag as a development area rather than treating each ratio in isolation

**Drive Authorship Signal (only for docs the user explicitly references — not auto-searched):**
- A user-referenced doc that is broadly shared (multiple viewers/commenters) → Evidence for "Technical Writing" and "Documentation Strategy" competencies, especially when the doc maps to a shipped Jira epic or GitHub docs migration
- If the user only references docs they commented/edited (not authored) → Evidence for reviewing/collaboration competencies, but flag as a gap for "ownership" competencies expecting the user to originate artifacts
