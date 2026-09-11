---
name: generate-performance-analysis
description: Generate the Performance Analysis report for a performance cycle — one competency-scale evaluation per competency, with supporting evidence and actionable steps, for a Technical Writer or Technical Writing Manager, using the VTEX career-path frameworks. Use when the user asks for a performance analysis, competency evaluation, or performance-cycle report and names a date range, role, and level.
---

# Generate Performance Analysis

Link every competency bullet to a specific Jira issue, GitHub PR, Slack thread, or Drive doc.

## Steps

1. Read `../_shared/data-collection.md` in full and follow it exactly: validate the Atlassian MCP connection first (mandatory — stop and report if it fails), then retrieve and process Jira/GitHub/Slack/Drive data, and load the correct career-path JSON for the user's role.
2. Read `../_shared/writing-standards.md` for tone, evidence-linking, and output-location rules.
3. Rate every competency using the evaluation scale below, build the report using the structure and rules below.
4. Save to `reports/[YYYY]/performance-analysis-[date-range].md` (year folder per `writing-standards.md`).

## Competency Evaluation Scale (MANDATORY per competency)

Assign **exactly one** rating per competency using this scale:

| # | Evaluation | When to assign |
| --- | --- | --- |
| 1 | **Not yet meeting expectations** | Does not have consistent examples of how this ability was achieved in the period |
| 2 | **Meets expectations** | Has consistent examples (≥3 distinct, period-relevant instances) of how this ability was achieved at the scope/complexity appropriate for the user's role/level |
| 3 | **Exceeds expectations** | Has consistent examples (≥3) **and** the work shows scope, complexity, or impact beyond what's typically expected at the user's level (e.g., handling higher-complexity issues than peers at that level, taking on cross-team or cross-repo scope, driving measurable efficiency/quality gains, disproportionate peer impact such as review-to-author ratio >1.5, resolving or preventing systemic blockers for others). Third-party recognition (explicit praise, mentorship attribution, leadership callouts) can support the rating but is not sufficient on its own — it must be backed by a concrete scope/impact signal |
| 4 | **Performing at the next level** | Meets the bar for "Exceeds expectations" **and** the evidence demonstrates competencies matching the next career level's expectations (compare against `levels[nextLevel].competencies[key]` in the career-path JSON, when a next level exists) |

Display each rating as: **`Evaluation: [Not yet meeting expectations | Meets expectations | Exceeds expectations | Performing at the next level]`** (include the number in parentheses on first mention in the report intro table only, if used). If the user is already at the top level of their track (Technical Writer L4 or Technical Writing Manager L6), "Performing at the next level" is not assignable — cap ratings at "Exceeds expectations" and note the level ceiling in the rationale when relevant.

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

**Technical Writer (IC):** One `#` section per entry in `dimensions[]` (use `label` as heading). Each dimension section MUST open with a dimension-level evaluation (rating + rationale) rolled up from its competencies, then contain one `##` subsection per granular competency key listed in `dimensions[].competencies`. Compare against `levels[userLevel].competencies[key]`.

**Technical Writing Manager:** One `#` section per competency key in the manager framework (each key is already dimension-level; no nested `##` unless the JSON defines sub-keys). The manager framework has no separate dimension layer, so no dimension-level roll-up is produced.

# [Dimension label — IC only; omit extra heading for manager if section title equals competency]

**Dimension evaluation:** [Not yet meeting expectations | Meets expectations | Exceeds expectations | Performing at the next level]

**Rationale:** [2-4 sentences rolling up the competency ratings in this dimension per the conservative roll-up rule below; name each competency's rating and the standout signal or the below-bar gap]

## [competency_key — human-readable label, e.g., Pattern recognition]

**Evaluation:** [Not yet meeting expectations | Meets expectations | Exceeds expectations | Performing at the next level]

**Rationale:** [2-4 sentences: why this rating was assigned against the scale above; cite evidence count and, for Exceeds expectations or higher, the specific scope/complexity/impact signal that goes beyond the expected level]

**Evidence:** X Jira issues, Y GitHub PRs, Z Slack threads, W user-provided Drive docs, V manually provided items (omit any source with zero count or that is not connected; Drive count only reflects docs the user explicitly referenced; show "Limited evidence" if total <3); if rated **Performing at the next level**, also cite which next-level competency expectations were matched

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
| [dimension label] | [Not yet meeting expectations | Meets expectations | Exceeds expectations | Performing at the next level] |

## Competency evaluation overview

| Competency | Evaluation |
| --- | --- |
| [key or label] | [Not yet meeting expectations | Meets expectations | Exceeds expectations | Performing at the next level] |

## Cross-cutting themes
- [Patterns across competencies: strongest areas, systemic gaps, metrics that explain multiple ratings]

## Priority development focus
- [Top 3 competencies rated Not yet meeting expectations or Meets expectations with highest impact on level expectations, with 1-sentence why each matters now]
```

## Competency Analysis Rules

- **One evaluation per competency** — never leave a competency without an explicit rating from the scale
- **Dimension roll-up (IC only) — be conservative:** After rating every competency in a dimension, assign the dimension exactly one rating using a **conservative** roll-up. A dimension is rated higher than "Meets expectations" ONLY when its competencies are *consistently* above bar; a single standout competency does not lift the dimension.
  - **Not yet meeting expectations:** any competency in the dimension is "Not yet meeting expectations" AND the remaining ones do not clearly compensate — err toward flagging the gap; the dimension is at most "Meets expectations" whenever a below-bar competency exists.
  - **Meets expectations:** competencies are at least a mix of "Meets" and "Exceeds" but NOT consistently above bar (e.g., one "Meets" + one "Exceeds"), or all "Meets". This is the default; when in doubt between two dimension ratings, choose the lower one.
  - **Exceeds expectations:** ALL competencies in the dimension are rated "Exceeds expectations" (or higher).
  - **Performing at the next level:** ALL competencies are "Performing at the next level".
  - In the dimension rationale, name each competency's rating and call out the standout signal (for higher ratings) or the below-bar gap to close first (for lower ratings). Never round a dimension up on the strength of one competency.
- **Supporting evidence:** 3-5 bullets per competency; link Jira keys/PR numbers in parentheses
- **Actionable steps to improve (required per competency):**
  - **Not yet meeting expectations → Meets expectations:** Steps to build consistent evidence (specific behaviors, artifact types, cadence, example issue types to own)
  - **Meets expectations → Exceeds expectations:** Steps to expand scope, complexity, or measurable impact beyond the current level (own higher-complexity issues, take on cross-team/cross-repo work, drive efficiency or quality gains, help unblock others) — not just steps to get noticed
  - **Exceeds expectations → Performing at the next level:** Steps to close the gap with next-level competency expectations (scope expansion, cross-team influence, ownership of next-level artifact types — cite the specific gap from `levels[nextLevel].competencies[key]`)
  - **Performing at the next level:** Steps to sustain and multiply impact (mentoring others, codifying practices, expanding scope, preparing a promotion case)
  - Each step must be **specific and doable** in the next review period — avoid generic advice ("communicate better")
- **Neutral tone:** Coaching-oriented, not judgmental or flattering
- **Evidence tracking:** Count distinct examples per competency; <3 consistent examples → maximum rating is **Not yet meeting expectations**; ≥3 at expected scope/complexity → **Meets expectations**; ≥3 with a concrete scope/complexity/impact signal beyond the expected level (recognition alone does not qualify) → **Exceeds expectations**; ≥3 meeting the "Exceeds expectations" bar **and** evidence matching next-level competency expectations → **Performing at the next level**. Manually provided evidence — items from the chat request, `context/additional-context.local.md`, or a Drive doc the user referenced — **does count toward the ≥3 threshold** on equal footing with Jira/GitHub/Slack evidence, provided each item is a distinct, period-relevant instance with a named artifact or outcome; a single doc or activity cited under several competencies still counts once per competency, and vague claims without an artifact or outcome ("mentored the team") don't count at all.
- **Limited evidence:** If <3 total data points, still assign **Not yet meeting expectations** and state "Limited evidence" in the Evidence line; actionable steps must address how to generate traceable examples in Jira/GitHub
- **Top-level ceiling:** For Technical Writer L4 or Technical Writing Manager L6 (no next level defined in the career-path JSON), do not assign "Performing at the next level"; cap at "Exceeds expectations"

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
