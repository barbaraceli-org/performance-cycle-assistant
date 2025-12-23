# 📊 Example Work Summary with Metrics

This example shows what a work summary report looks like with the quantitative metrics feature, including the **new advanced metrics**:
- **Carryover & Scope Creep Analysis** - Contextualizes completion rates
- **Review-to-Author Ratio** - Measures "Force Multiplier" behavior
- **Semantic Blocker Categorization** - Root cause analysis of impediments

**Use this to:**
- See what your reports will look like
- Understand metric formatting
- Learn how work is organized by quarter and area
- See blocker analysis in action
- Understand the new advanced metrics

**Related:**
- [Example Requests](example-request.md) - How to request reports
- [Metrics Guide](../METRICS_GUIDE.md) - Understanding all metrics (basic + advanced)
- [Setup & Usage](../docs/SETUP.md) - Complete guide

---

# Work summary

**Period:** 2025-01-01 to 2025-06-30

## Overview Metrics

### Jira Activity
- **Total issues worked on:** 87 (issues that were "In Progress" at any point during the period, including carryover work from before the period)
- **Carryover issues:** 23 (26% - issues already "In Progress" at period start)
- **New issues started:** 64 (issues that moved to "In Progress" during the period)
- **Scope creep:** 18 (28% of new starts - issues created and assigned after period start)
- **Issues completed:** 72
- **Issues in progress:** 11 (actively being worked on at period end; may include blocked issues)
- **Issues blocked:** 4 (blocked status at period end; may overlap with in progress)
- **Issues unfinished:** 0 (backlog/other non-completed issues at period end, excluding in progress and blocked)
- **Completion rate:** 83% (72 completed / 87 worked on; 11 issues still in progress, 23 carryover issues)
- **Average resolution time:** 8.5 days (resolutiondate - in_progress_date)
- **Work areas covered:** 5
- **Issue type breakdown:** Update: 54 (62%), New: 18 (21%), Task: 12 (14%), Review: 3 (3%)
- **Priority distribution:** High: 15 (17%), Medium: 58 (67%), Low: 14 (16%)

### GitHub Activity
- **Pull requests authored:** 45 total (38 merged, 5 open, 2 closed)
- **Pull requests reviewed:** 23 across 8 repositories
- **Review-to-author ratio:** 0.5:1 (23 reviews / 45 authored PRs)
- **Documentation commits:** 127 commits in 38 PRs, 215 files modified
- **Repositories contributed to:** 8
- **Average PR merge time:** 2.3 days
- **Lines changed:** +12,450 / -3,280
- **PR status breakdown:** Merged: 38 (84%), Open: 5 (11%), Closed: 2 (5%)
- **Repository distribution:** vtex-docs: 28 (62%), faststore: 12 (27%), developer-portal: 5 (11%)

## Accomplishments

### Quarter 1
**Jira:** 38 issues completed | 5 in progress | 88% completion rate
**GitHub:** 22 PRs merged | 3 open | 12 reviews | 5 repositories

#### Store Framework Documentation
**Jira metrics:** 15 completed | 2 in progress | Avg resolution: 6.2 days

**Accomplishments:**
- Reviewed and updated Store Framework getting started guides, including setting up development environment, configuring accounts, and creating Store Theme projects
- Created comprehensive documentation for customizing 404 pages in Store Framework stores
- Developed category cover page for "Using advanced layouts" to improve navigation and content discoverability
- Updated Store Theme documentation to include project structure details for better developer onboarding
- Resolved multiple documentation feedback issues related to Store Framework components and configurations

#### FastStore Documentation & WebOps
**Jira metrics:** 12 completed | 1 in progress | Avg resolution: 7.8 days

**Accomplishments:**
- Created comprehensive documentation for secrets management in FastStore via WebOps, including AWS Secrets Manager integration
- Updated FastStore WebOps Dashboard documentation to include new Secrets section and Settings features
- Documented go-live process updates for FastStore WebOps, including domain configuration steps
- Created release notes for WebOps Dashboard Settings feature and Headless CMS integration improvements
- Developed troubleshooting documentation for CMS plugin installation errors

#### UI Components Documentation
**Jira metrics:** 8 completed | 1 in progress | Avg resolution: 5.1 days

**Accomplishments:**
- Created documentation for new FastStore UI components including Textarea, TextareaField, Tooltip, RatingField, and ModalFooter
- Updated Rating component documentation with new design tokens
- Completed localization reviews for multiple UI component documentation pages
- Updated category cover pages within UI Components section to include newly documented components

#### Content Reviews & Localization
**Jira metrics:** 3 completed | 1 in progress | Avg resolution: 4.5 days

**Accomplishments:**
- Conducted peer reviews for multiple documentation updates across Store Framework and FastStore
- Completed localization reviews for FastStore documentation including My Account extensions and secrets management
- Reviewed and resolved documentation feedback forms addressing errors, improvements, and missing information
- Updated cross-border store documentation based on SMB Professional Services materials

### Quarter 2
**Jira:** 34 issues completed | 6 in progress | 79% completion rate
**GitHub:** 16 PRs merged | 2 open | 11 reviews | 3 repositories

#### FastStore Platform & Features
**Jira metrics:** 14 completed | 3 in progress | Avg resolution: 9.3 days

**Accomplishments:**
- Documented My Account feature extensibility for FastStore, including page and section customization
- Created comprehensive documentation for Delivery Promises feature implementation
- Updated FastStore WebOps documentation to reflect better integration with Headless CMS deployments
- Documented performance improvements for product listing pages (PLP) and product detail pages (PDP)
- Created migration guide for FastStore Platform documentation to Developer Portal

#### Store Framework Best Practices
**Jira metrics:** 11 completed | 2 in progress | Avg resolution: 10.5 days

**Accomplishments:**
- Developed Analytics best practices documentation for Store Framework, focusing on Google Tag Manager integration
- Reviewed and updated Google Tag Manager installation and configuration guides
- Created Accessibility best practices documentation for VTEX stores
- Updated Store Framework glossary with essential terminology and concepts
- Completed review of defining styles and making store themes public documentation

#### Developer Experience & Tooling
**Jira metrics:** 6 completed | 1 in progress | Avg resolution: 6.8 days

**Accomplishments:**
- Updated VTEX IO CLI installation documentation with corrected download links and installation procedures
- Created documentation for Site Editor content loss prevention through vtex plugins command updates
- Updated Contributing with Developer Portal documentation to align with external contributor guidelines
- Resolved issues with interface creation for app settings documentation
- Updated redirect documentation to clarify querystring handling limitations

#### Technical Writing Process Improvements
**Jira metrics:** 3 completed | 0 in progress | Avg resolution: 12.3 days

**Accomplishments:**
- Conducted external interviews for 25H1KR3 Phase 1 research initiative
- Defined communication channels for 25H1KR3 Phase 2 implementation
- Prepared content proposals for Foresight Commerce Academy training materials
- Created training scripts for Developer Portal exploration and discovery modules
- Developed troubleshooting guide for order cancellation scenarios following new guidelines

## What couldn't be finished

**Unfinished work metrics:**
- **Jira:** 15 unfinished total (In progress: 11 | Blocked: 4 | Backlog: 0) | Avg age: 45 days
- **GitHub:** 5 open PRs | Draft: 2 | Avg age: 18 days
- **Primary blockers:** External dependencies (6 issues), Awaiting decisions (5 issues), Resource constraints (4 issues)

#### Store Framework Documentation
**Metrics:** 4 unfinished Jira issues | 2 open PRs | Avg age: 62 days

**Unfinished tasks:**
- Creating "Essential Concepts" documentation aggregating composition, interface, properties, and slots guides remains on hold pending content strategy decisions
- Cross-border documentation review within Content Management System category requires further alignment on information architecture
- Sitemap IO documentation clarification about automatic generation needs additional engineering validation
- Modal Layout documentation enhancement with reference image code examples awaits product team input

#### Legacy CMS & Portal Documentation
**Metrics:** 4 unfinished Jira issues | 1 open PR | Avg age: 58 days

**Unfinished tasks:**
- Banner configuration documentation improvements require additional field descriptions and visual examples
- Collection registration documentation needs clarification about department selection behavior
- Product comparison control documentation requires updates to specify SearchResult-only functionality
- Separating CMS-specific content from promotion configuration documentation is pending content reorganization

#### App Documentation & Maintenance
**Metrics:** 3 unfinished Jira issues | 1 open PR | Avg age: 71 days

**Unfinished tasks:**
- Zendesk Chat app documentation enhancement requires settings functionality explanation and is blocked by lack of original team availability
- Delivery Window Blocker app ownership and support status documentation is pending deprecation decision
- SafeData app title correction in README requires repository access and formatting fixes

#### Documentation Feedback Backlog
**Metrics:** 4 unfinished Jira issues | 1 open PR | Avg age: 23 days

**Unfinished tasks:**
- Favicon configuration documentation for FastStore and VTEX IO stores needs consolidation into single comprehensive guide
- Interface creation for app settings documentation requires public access key configuration examples
- Managing SEO documentation page showing as 404 needs content restoration or proper redirect implementation
- Boosting performance landing page requires content implementation to replace empty placeholder

### Blocker Analysis

**Blocker categories (semantically grouped from descriptions, comments, and labels):**
- **Awaiting API Specifications:** 6 issues | Avg resolution: 45 days
  - *Root causes:* Engineering teams prioritizing feature development over documentation specs; incomplete API design before documentation requests; lack of API-first development process
  - *Mitigation:* Establish regular check-ins with engineering teams, request API specs earlier in development cycle, create documentation templates that prompt for required technical details
- **Stakeholder Review Bottlenecks:** 5 issues | Avg resolution: 38 days
  - *Root causes:* Subject matter experts with competing priorities; unclear review responsibilities; lack of review SLAs
  - *Mitigation:* Escalate decision requests to stakeholders, provide decision frameworks with pros/cons, set explicit review deadlines, identify backup reviewers
- **Resource Constraints:** 4 issues | Avg resolution: 52 days
  - *Root causes:* Team capacity limitations during peak periods; competing high-priority projects; insufficient technical writing resources for scope
  - *Mitigation:* Prioritize work based on business impact, communicate capacity limitations to stakeholders early, explore automation opportunities for repetitive tasks

**Recurring impediment patterns:**
- **Awaiting API Specifications** appears in 40% of blocked issues, indicating need for earlier technical writer involvement in API design process
- **Review bottlenecks** consistently delay work by 30+ days, suggesting need for review SLAs and backup reviewer assignments
- **Resource constraints** during Q2 coincided with major product launches, highlighting need for capacity planning aligned with product roadmap

