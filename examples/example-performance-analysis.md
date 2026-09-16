# 🎯 Example Performance Analysis

This example shows what a performance analysis report looks like for an **L2 Technical Writer** over the same period as the [work summary example](example-report-with-metrics.md) — the two reports are generated from one request and share the same underlying Jira/GitHub data.

**Use this to:**
- See how each competency gets exactly one rating from the 4-point scale
- Understand the **conservative dimension roll-up** (a single standout competency never lifts the dimension)
- See how metrics from the work summary become competency evidence
- See what "Limited evidence" looks like and how actionable steps differ per rating

**Related:**
- [Example work summary](example-report-with-metrics.md) - The companion report for this same period
- [Example Requests](example-request.md) - How to request reports
- [Metrics Guide](../METRICS_GUIDE.md) - Metric definitions and the competency vocabulary mapping
- [Setup & Usage](../docs/SETUP.md) - Complete guide

> **Note:** Ratings, Jira keys, and PR numbers below are illustrative. The competency keys and dimensions are the real ones from `context/technical-writer-career-path.json`.

---

# Performance analysis

This analysis is based on the **VTEX Technical Writer Career Path**, using the **Technical Writer** framework for IC roles and **Technical Writing Manager** framework for manager roles. The manager track includes an additional **Management** competency.

**Period:** 2025-01-01 to 2025-06-30
**Role and level:** Technical Writer, L2 (Technical Writer II)

## Evaluation scale

| # | Evaluation | Description |
| --- | --- | --- |
| 1 | Not yet meeting expectations | Doesn't have consistent examples on how this ability was achieved |
| 2 | Meets expectations | Has consistent examples on how this ability was achieved at the scope/complexity expected for the level |
| 3 | Exceeds expectations | Has consistent examples and the work shows scope, complexity, or impact beyond what's expected at that level |
| 4 | Performing at the next level | Exceeds expectations at the current level and shows evidence matching the next career level's expectations |

# Abstraction & Modeling

**Dimension evaluation:** Not yet meeting expectations

**Rationale:** Pattern recognition meets expectations, with repeated evidence of anticipating downstream documentation impact before making changes. System design is below bar: the period contains no example of modeling a scalable process or delivering a scoped solution independently, only individually-scoped page work. Per the conservative roll-up, one at-bar competency does not compensate for a below-bar one, so the dimension is rated at the lower level. Closing the system design gap is the single highest-leverage change in this dimension.

## Pattern recognition

**Evaluation:** Meets expectations

**Rationale:** Six distinct instances of anticipating the consequences of product changes on existing documentation and planning around them, which is the L2 expectation. The work consistently traced a change back to every affected page rather than patching the page in front of it. Scope stayed within owned surfaces, with no cross-team or ecosystem-level pattern work, so this does not reach the "Exceeds expectations" bar.

**Evidence:** 9 Jira issues, 4 GitHub PRs

### Supporting evidence
- Mapped every Store Framework page affected by the development-environment setup change before editing, avoiding four downstream inconsistencies (EDU-1042, EDU-1051, PR #312)
- Identified that the WebOps Secrets section change also invalidated the go-live and domain-configuration steps, and updated all three together (EDU-1188, EDU-1194, PR #367)
- Flagged that new FastStore UI components required matching updates to the category cover pages, preventing orphaned entries (EDU-1223, EDU-1230)
- Recognized the recurring "querystring handling" confusion across redirect issues and resolved it with one clarification rather than three separate fixes (EDU-1401)

### Actionable steps to improve
- Take one issue per month where the affected surface spans two work areas (for example, Store Framework plus Developer Portal) and own the full impact analysis
- Write a short impact note in the Jira description before starting any Update-type issue, listing every page the change touches, so the analysis is traceable evidence
- Pair with a senior writer on one cross-team change (such as the Developer Portal migration) to observe ecosystem-level pattern analysis
- Propose one reusable content pattern (for example, a standard structure for component reference pages) based on repetition observed this period

## System design

**Evaluation:** Not yet meeting expectations

**Rationale:** The L2 expectation is modeling scalable team processes and delivering scoped solutions with moderate guidance; the period contains only two partial examples, below the three-instance bar. Work was consistently framed as individual pages rather than as systems, and the one process contribution (the migration guide) was executed to someone else's plan rather than modeled independently. This is a gap of opportunity as much as of skill: no issue in the period was scoped as a process or model.

**Evidence:** 2 Jira issues, 1 GitHub PR — Limited evidence (2 distinct examples, below the 3-instance bar; PR #421 belongs to EDU-1355)

### Supporting evidence
- Created the migration guide for moving FastStore Platform documentation to the Developer Portal, following a plan defined by another writer (EDU-1355, PR #421)
- Proposed the "Essential Concepts" aggregation structure for composition, interface, properties, and slots guides, though it remains on hold pending content strategy decisions (EDU-1120)
- No evidence in the period of modeling a repeatable team process or a content model applied by others

### Actionable steps to improve
- Take the on-hold "Essential Concepts" issue (EDU-1120) to a decision next period: write the proposed structure, get it reviewed, and either ship it or close it with a documented rationale
- Own one Epic-type issue end to end rather than a set of Update-type issues, so scoping and sequencing decisions are yours and visible in the changelog
- Turn one repeated task into a documented template (for example, the release-note format used across WebOps issues) and land it in the team guidelines
- Ask the team lead to be assigned one "limited scope solution" issue per quarter explicitly, since none were assigned this period

# Responsibility & Scope

**Dimension evaluation:** Meets expectations

**Rationale:** Domain mastery exceeds expectations, with end-to-end ownership of the FastStore WebOps surface that is closer to L3 breadth than L2. Ecosystem collaboration and operational excellence both meet expectations, supported by consistent peer review and process contributions but without the go-to-person or process-ownership signal that would lift them. Because the competencies are a mix of "Exceeds" and "Meets" rather than consistently above bar, the dimension rolls up to "Meets expectations".

## Domain mastery

**Evaluation:** Exceeds expectations

**Rationale:** Twenty-six completed issues across FastStore WebOps, Store Framework, and UI Components show the L2 expectation of completing medium-to-large projects across familiar and unfamiliar products. The scope signal beyond level is the WebOps surface: this was the only writer covering it, spanning secrets management, dashboard settings, go-live, and CMS integration, which is single-owner breadth normally expected at L3. Two of those areas were unfamiliar at period start and were picked up without a handover.

**Evidence:** 26 Jira issues, 19 GitHub PRs

### Supporting evidence
- Sole owner of FastStore WebOps documentation for the full period: secrets management with AWS Secrets Manager, Dashboard Settings, go-live domain configuration, and Headless CMS integration (EDU-1188, EDU-1194, EDU-1201, EDU-1244, PRs #367, #374, #388)
- Delivered Delivery Promises and My Account extensibility documentation for an unfamiliar product area with no prior handover (EDU-1289, EDU-1302)
- Covered three work areas in Q1 and three in Q2 with a 6.2-day average resolution in the largest area, indicating breadth did not cost throughput
- Documented PLP and PDP performance improvements, requiring engineering context outside the usual documentation scope (EDU-1330)

### Actionable steps to improve
- Document the WebOps surface's ownership map and open questions so the breadth is transferable, which is the L3 expectation of end-to-end module understanding
- Take one cross-system feature with unclear ownership (the Zendesk Chat app documentation is a candidate) and drive the ownership decision, not just the content
- Present the WebOps documentation architecture to the writing chapter to convert individual breadth into shared team knowledge
- Set a measurable target for the next period, such as reducing WebOps feedback-form issues by half, to attach an outcome to the ownership

## Ecosystem collaboration

**Evaluation:** Meets expectations

**Rationale:** Twenty-three PR reviews across eight repositories and consistent participation in localization review meet the L2 expectation of actively collaborating to deliver team work. The review-to-author ratio of 0.5 sits at the boundary of the siloed-work threshold, meaning review contribution is real but not disproportionate, so there is no force-multiplier signal to support a higher rating. Contribution to activities beyond core scope, such as hiring or chapter work, is absent from the period.

**Evidence:** 7 Jira issues, 23 GitHub PR reviews

### Supporting evidence
- Reviewed 23 PRs across 8 repositories, concentrated in vtex-docs and faststore (review-to-author ratio 0.5:1)
- Conducted peer reviews for Store Framework and FastStore updates as a standing part of the Content Reviews work area (EDU-1097, EDU-1156)
- Completed localization reviews for My Account extensions and secrets management documentation, unblocking the localization queue (EDU-1163, EDU-1170)
- Interviewed external participants for the 25H1KR3 Phase 1 research initiative alongside another team (EDU-1370)

### Actionable steps to improve
- Raise the review-to-author ratio above 1.0 next period by claiming two reviews per week from the team queue, which is the concrete step toward the force-multiplier signal
- Volunteer as onboarding buddy for the next writer joining the team, since mentoring evidence is entirely absent this period
- Join one hiring loop or writing chapter session per quarter and record it in `context/additional-context.local.md` so it counts as evidence
- Leave substantive review comments (structure, accuracy) rather than approvals, and keep a note of the two or three reviews that changed a PR's direction

## Operational excellence

**Evaluation:** Meets expectations

**Rationale:** Three process contributions in the period — the localization review workflow, the WebOps release-note cadence, and the 25H1KR3 communication channels — match the L2 expectation of identifying improvement opportunities and driving changes. The improvements were adopted within the writer's own work areas rather than by the team as a whole, and the recurring blocker pattern below was surfaced but not escalated, which keeps this at bar rather than above it.

**Evidence:** 5 Jira issues, 1 GitHub PR

### Supporting evidence
- Defined the communication channels for 25H1KR3 Phase 2 implementation, giving the initiative a standing process (EDU-1382)
- Established a repeatable release-note format for WebOps Dashboard features, reused across four subsequent releases (EDU-1244, EDU-1251)
- Created a troubleshooting guide for CMS plugin installation errors that reduced repeat support questions in the work area (EDU-1215)
- Surfaced the "Awaiting API Specifications" blocker pattern (6 issues, 45-day average) in issue comments, though without a formal escalation (EDU-1120, EDU-1401)

### Actionable steps to improve
- Escalate the "Awaiting API Specifications" pattern formally: bring the 6-issue, 45-day figure to the team lead with a proposed intake change, since recurring blockers are the clearest operational-excellence evidence available
- Convert the WebOps release-note format into a team-wide template so the improvement lands beyond your own work area
- Take one of the four blocked issues per month and drive it to unblocked or closed, rather than letting age accumulate (current average age is 45 days)
- Propose a review SLA for the "Stakeholder Review Bottlenecks" category (5 issues, 38-day average) at the next team retro

# Autonomy & Execution

**Dimension evaluation:** Meets expectations

**Rationale:** All three competencies — professional mastery, execution and delivery reliability, and decision making — are rated "Meets expectations", supported by an 83% completion rate, 28% scope creep, and consistent independent delivery within assigned scope. Nothing in the dimension is below bar, and nothing shows the scope, complexity, or measurable impact that would lift a competency above it. Execution and delivery reliability is the closest to exceeding and the best target for concentrated effort.

## Professional mastery

**Evaluation:** Meets expectations

**Rationale:** Four instances of proactively seeking feedback and applying it, which is the explicit L2 expectation. Feedback was sought before delivery rather than after review comments arrived, and the resulting revisions are visible in PR history. There is no evidence of learning applied beyond the immediate task, such as adopting a new tool or technique that changed how work is done, which is what would push toward the next level.

**Evidence:** 8 Jira issues, 6 GitHub PRs

### Supporting evidence
- Requested engineering review on the secrets management draft before opening the PR, incorporating three technical corrections pre-review (EDU-1188, PR #367)
- Sought and applied product-team feedback on the Delivery Promises documentation structure before writing (EDU-1289)
- Iterated the Accessibility best practices page across two rounds of chapter feedback (EDU-1318, PR #402)
- Consumed English-language engineering specs directly as source material across the FastStore work area

### Actionable steps to improve
- Adopt one new tool or technique this period (structured authoring, a linting rule, a diagram-as-code workflow) and apply it to at least three pages, so learning is visible as changed output
- Ask for feedback on one piece of work from outside the writing team each month, since all four current examples come from immediate collaborators
- Write a short retro note after each large project identifying what you would do differently, and cite it in the next period's report
- Complete one course or certification relevant to the FastStore domain and apply it to a specific issue

## Execution and delivery reliability

**Evaluation:** Meets expectations

**Rationale:** An 83% completion rate across 87 issues worked on, with a 2.3-day average PR merge time, shows the L2 expectation of consistently meeting deadlines with correct complexity estimation. Scope creep of 28% sits in the moderate band and did not derail delivery. Carryover of 26% is within normal range and is explained by genuinely blocked work rather than by stalled effort, but there is no evidence yet of leading planning for a large project with defined success metrics, which is the L3 expectation.

**Evidence:** 87 Jira issues worked on, 45 GitHub PRs

### Supporting evidence
- Completed 72 of 87 issues worked on (83%), with an 8.5-day average resolution time (Q1: 88% completion, Q2: 79%)
- Absorbed 18 unplanned issues (28% of new starts) without missing delivery on planned work
- Maintained a 2.3-day average PR merge time across 38 merged PRs, indicating drafts arrived review-ready
- The 15 unfinished issues are concentrated in externally blocked categories (API specifications, stakeholder review, resource constraints) rather than in abandoned work

### Actionable steps to improve
- Lead planning for one large content project next period, defining goals and success metrics up front, which is the specific L3 expectation not yet demonstrated
- Reduce work in progress: close or hand off the two oldest carryover issues (currently 62 and 71 days old) before starting new work
- Publish an estimate on each issue at start and compare it to actuals at close, to build an estimation track record beyond the aggregate completion rate
- Attach one measurable outcome (support-ticket reduction, page-view change, time-to-first-success) to a delivered project, since impact evidence is what separates this rating from "Exceeds"

## Decision making

**Evaluation:** Meets expectations

**Rationale:** Five instances of identifying and addressing relevant work before assignment, matching the L2 expectation of taking initiative aligned with team priorities and understanding internal workflows. Decisions were sound and independent within assigned scope, and help was sought appropriately when blocked. The scope of the decisions stayed within individual issues rather than affecting the team's direction, which keeps this at bar.

**Evidence:** 11 Jira issues, 5 GitHub PRs

### Supporting evidence
- Identified and fixed the incorrect VTEX IO CLI download links without being assigned the issue (EDU-1344, PR #415)
- Chose to consolidate rather than duplicate the favicon guidance across FastStore and VTEX IO stores after finding overlapping requests (EDU-1420)
- Decided to restore the Managing SEO page content rather than redirect it, after checking traffic data (EDU-1428)
- Escalated the Delivery Window Blocker app ownership question rather than guessing at deprecation status (EDU-1437)
- Recognized when the Zendesk Chat app work was blocked by team unavailability and moved on rather than stalling (EDU-1433)

### Actionable steps to improve
- Bring one decision per quarter to the team that affects more than your own work areas, such as the information architecture question blocking the cross-border review
- Document the trade-offs considered in the Jira issue when you make a judgment call, so the reasoning is evidence and not just the outcome
- Take one issue currently blocked on someone else's decision and propose two concrete options with a recommendation, instead of waiting for direction
- Shadow the team lead's prioritization session once per quarter to build the organizational context the L3 expectation assumes

# Communication

**Dimension evaluation:** Meets expectations

**Rationale:** Expectation management, problem framing, and strategic influence are all rated "Meets expectations": bottlenecks were consistently surfaced to stakeholders, technical problems were articulated clearly with supporting detail, and feedback culture was actively supported in team rituals. No competency is below bar and none shows impact beyond the immediate team, so the dimension sits at the default rating. The 0.5 review-to-author ratio is the shared limiting factor across all three.

## Expectation management

**Evaluation:** Meets expectations

**Rationale:** Consistent evidence across at least five issues of making peers and stakeholders aware of current bottlenecks, which is the L2 expectation. Blockers were reported in issue comments with enough context for others to act, and the semantic blocker analysis for the period was possible precisely because the reasons were written down. Communication was reactive to blockers rather than proactive about project status, which is the L3 step up.

**Evidence:** 9 Jira issues, 3 GitHub PRs

### Supporting evidence
- Documented the specific missing API details on each of the six issues in the "Awaiting API Specifications" category, making the blocker pattern analyzable (EDU-1120, EDU-1401)
- Notified the product team when the Modal Layout documentation stalled awaiting reference examples, rather than letting the issue age silently (EDU-1116)
- Flagged the engineering validation needed for the Sitemap IO automatic-generation claim before publishing an unverified statement (EDU-1108)
- Communicated capacity limits during the Q2 launch period, which is reflected in the resource-constraint blocker category (4 issues)

### Actionable steps to improve
- Send a written status summary for each work area at the end of every sprint, so status communication is proactive rather than blocker-triggered
- Record decisions and their rationale in the Jira issue at the moment they are made, which is the explicit L3 expectation
- Set an explicit "waiting since" date on every blocked issue and follow up when it exceeds two weeks, rather than at the 38-to-52-day averages seen this period
- Agree on review deadlines with stakeholders up front on the next three issues that require sign-off

## Problem framing

**Evaluation:** Meets expectations

**Rationale:** Four instances of articulating a technical problem clearly enough for others to act, which meets the L2 expectation of having clear conversations and stating one's own position. Explanations were grounded in specifics rather than general concerns. The framing consistently addressed immediate collaborators; there is no example of framing a problem with data and analysis for a wider or more senior audience, which is the L3 expectation.

**Evidence:** 6 Jira issues, 4 GitHub PRs

### Supporting evidence
- Articulated the Site Editor content-loss risk precisely enough for engineering to scope a fix to the `vtex plugins` command (EDU-1351, PR #418)
- Framed the querystring redirect limitation as a user-facing expectation problem rather than a documentation gap, changing how it was resolved (EDU-1401)
- Explained the interface-creation documentation gap in terms of the missing public access key example, giving reviewers a concrete ask (EDU-1424)
- Presented external interview findings from the 25H1KR3 research to the team with specific user quotes (EDU-1370)

### Actionable steps to improve
- Bring data to the next framing: quantify one problem (for example, feedback-form volume on a page) before proposing the fix
- Write one short problem statement document for a recurring issue and share it with product, not only within the writing team
- Present the 25H1KR3 research findings to a stakeholder audience outside the immediate team
- Practice stating a recommendation alongside the problem, rather than describing the problem and awaiting direction

## Strategic influence

**Evaluation:** Meets expectations

**Rationale:** Active participation in team rituals and consistent contribution to feedback culture across four instances meet the L2 expectation. Feedback given in PR reviews was constructive and specific. The 0.5 review-to-author ratio caps how far this influence reaches — half as many reviews as authored PRs limits the surface on which influence can be demonstrated — so this sits at bar rather than above it.

**Evidence:** 5 Jira issues, 23 GitHub PR reviews

### Supporting evidence
- Gave structural feedback on peer PRs in the vtex-docs and faststore repositories across 23 reviews
- Participated consistently in localization review as a standing team ritual (EDU-1163, EDU-1170)
- Contributed content proposals for the Foresight Commerce Academy training materials, extending influence beyond the documentation surface (EDU-1388)
- Created training scripts for Developer Portal exploration modules, shaping how others learn the product (EDU-1394)

### Actionable steps to improve
- Increase review volume to bring the ratio above 1.0, since influence scales with the number of PRs you touch
- Ensure internal team processes you rely on are documented and owned, which is the explicit L3 expectation and currently has no evidence
- Give your manager specific, constructive input on one peer's work per cycle, framed against the career-path expectations
- Take a visible role in one team ritual — for example, running the localization review — rather than participating in it

# Writing

**Dimension evaluation:** Exceeds expectations

**Rationale:** All three competencies are above bar: content strategy and editorial governance both exceed expectations, and content quality is performing at the next level. The dimension therefore qualifies for "Exceeds expectations" under the roll-up rule, which requires every competency to be at least "Exceeds". It does not reach "Performing at the next level", which would require all three competencies at that rating; content strategy and editorial governance are the two to develop for that.

## Content strategy

**Evaluation:** Exceeds expectations

**Rationale:** Consistent evidence of the L2 systemic mindset — planning each change against the broader documentation landscape — across at least six issues. The scope signal beyond level is the Developer Portal migration: mapping and sequencing the move of the entire FastStore Platform documentation set is cross-surface planning normally expected at L3, and it required coordinating dependencies outside the writer's own work areas. This is scope beyond level, not recognition, so it meets the "Exceeds" bar.

**Evidence:** 14 Jira issues, 11 GitHub PRs

### Supporting evidence
- Planned and executed the FastStore Platform documentation migration to the Developer Portal, mapping cross-surface dependencies before moving content (EDU-1355, PR #421)
- Built the category cover page for "Using advanced layouts", a navigation-level rather than page-level intervention (EDU-1063)
- Kept UI Components category cover pages current as new components were documented, treating the section as a system rather than a set of pages (EDU-1223, EDU-1230)
- Proposed consolidating favicon guidance across two product surfaces into a single guide instead of maintaining parallel pages (EDU-1420)
- Identified the cross-border documentation information-architecture question as a strategy decision rather than an editing task (EDU-1127)

### Actionable steps to improve
- Ground the next strategy decision in user research rather than internal reasoning, since articulating jobs to be done from research is the specific L3 expectation not yet met
- Produce a documented target-state map for one work area (what the ideal documentation body looks like) and track progress against it quarterly
- Resolve the cross-border information-architecture question you identified (EDU-1127), converting a surfaced strategy issue into an owned decision
- Map cross-team dependencies explicitly for the next migration, in writing, so the planning is reviewable

## Editorial governance

**Evaluation:** Exceeds expectations

**Rationale:** Consistent application of VTEX guidelines across the period meets the L2 bar, and the scope signal beyond level is contribution to the guidelines themselves rather than only compliance with them: the Accessibility and Analytics best-practices pages and the Store Framework glossary now function as team-wide reference material. Shared ownership in evolving guidelines is an L3 expectation, demonstrated here in three concrete artifacts.

**Evidence:** 8 Jira issues, 7 GitHub PRs

### Supporting evidence
- Authored the Accessibility best practices documentation for VTEX stores, now referenced by other writers as a standard (EDU-1318, PR #402)
- Authored the Analytics best practices documentation for Store Framework, establishing the Google Tag Manager guidance pattern (EDU-1311, PR #396)
- Built the Store Framework glossary, standardizing terminology used across the work area (EDU-1325)
- Enforced guideline consistency across 23 PR reviews, with corrections concentrated on structure and terminology
- Aligned the Contributing with Developer Portal documentation to external contributor guidelines (EDU-1361)

### Actionable steps to improve
- Mentor one writer on editorial standards using the guidelines you authored, which is the L4 expectation and the direction of travel from here
- Propose one change to the shared VTEX guidelines based on patterns seen in your 23 reviews, moving from contributing artifacts to shaping the guidelines themselves
- Run a consistency audit across one full category and publish the findings, converting per-PR enforcement into systemic evidence
- Define review criteria for the best-practices pages so others can maintain them without you

## Content quality

**Evaluation:** Performing at the next level

**Rationale:** The L2 bar (clear, correct content that may require revision, presenting complex ideas simply) is comfortably exceeded, with a 2.3-day average merge time across 38 merged PRs indicating drafts consistently arrived near-final rather than needing rework. Beyond that, the evidence matches the L3 expectations of "creates and revises compelling and unambiguous communication materials following VTEX guidelines" and "writes documentation requiring minimal support to ensure content quality": the secrets management, Delivery Promises, and troubleshooting content were technically complex, published with minimal editorial intervention, and are reused as reference examples by the team.

**Evidence:** 31 Jira issues, 24 GitHub PRs — matches L3 `content_quality` expectations for unambiguous materials produced with minimal support

### Supporting evidence
- Documented secrets management in FastStore via WebOps, including AWS Secrets Manager integration, a high-complexity topic published with no substantive review corrections (EDU-1188, PR #367)
- Produced the CMS plugin installation troubleshooting guide, which reduced repeat questions in the work area (EDU-1215)
- Averaged 2.3 days from PR open to merge across 38 merged PRs, well below the team norm, indicating minimal revision cycles
- Documented PLP and PDP performance improvements accurately enough to require no engineering corrections (EDU-1330)
- Delivered the order-cancellation troubleshooting guide following new guidelines without supervision (EDU-1445)

### Actionable steps to improve
- Sustain the low-revision delivery rate while taking on higher-complexity topics, so the signal holds at greater difficulty
- Codify what makes these drafts review-ready into a short pre-PR checklist the team can adopt
- Mentor one writer through a complex topic end to end, converting individual quality into team capability
- Experiment with one non-prose format (interactive example, diagram-as-code, embedded snippet) on a high-traffic page and measure the effect

# Summary

## Dimension evaluation overview

| Dimension | Evaluation |
| --- | --- |
| Abstraction & Modeling | Not yet meeting expectations |
| Responsibility & Scope | Meets expectations |
| Autonomy & Execution | Meets expectations |
| Communication | Meets expectations |
| Writing | Exceeds expectations |

## Competency evaluation overview

| Competency | Evaluation |
| --- | --- |
| Pattern recognition | Meets expectations |
| System design | Not yet meeting expectations |
| Domain mastery | Exceeds expectations |
| Ecosystem collaboration | Meets expectations |
| Operational excellence | Meets expectations |
| Professional mastery | Meets expectations |
| Execution and delivery reliability | Meets expectations |
| Decision making | Meets expectations |
| Expectation management | Meets expectations |
| Problem framing | Meets expectations |
| Strategic influence | Meets expectations |
| Content strategy | Exceeds expectations |
| Editorial governance | Exceeds expectations |
| Content quality | Performing at the next level |

## Cross-cutting themes
- **Writing craft is the clear strength.** All three Writing competencies are above bar, and content quality already matches L3 expectations — the 2.3-day average merge time across 38 merged PRs is the single strongest quantitative signal in the period.
- **Breadth is ahead of depth of ownership.** Domain mastery exceeds expectations on the strength of covering the WebOps surface alone, but system design has limited evidence: work is consistently scoped as pages, not as models or processes.
- **A 0.5 review-to-author ratio caps three separate competencies.** Ecosystem collaboration, strategic influence, and expectation management all sit at "Meets expectations" partly because peer-facing surface area is limited. Raising review volume is one action with three competency effects.
- **Blockers were documented but not escalated.** The "Awaiting API Specifications" pattern (6 issues, 45-day average) was well recorded in issue comments — which is why the analysis is possible — but was never brought forward as a process problem, which limits operational excellence.
- **Delivery is reliable and contextualized.** The 83% completion rate holds up against 26% carryover and 28% scope creep, both within normal bands, so the unfinished work reflects external blockers rather than execution problems.

## Priority development focus
- **System design** — the only competency below bar, and it caps the entire Abstraction & Modeling dimension regardless of how strong pattern recognition is; owning one Epic-scoped or process-scoped piece of work would close it.
- **Ecosystem collaboration** — raising the review-to-author ratio above 1.0 is a single, measurable change that also lifts the evidence base for strategic influence and expectation management.
- **Execution and delivery reliability** — the closest competency to exceeding; leading the planning for one large project with defined success metrics is the specific L3 expectation missing today, and it is the natural next step given an already-solid completion record.
