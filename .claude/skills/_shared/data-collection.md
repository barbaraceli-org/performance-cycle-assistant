# Shared: Connection Validation & Data Retrieval

Used by `generate-work-summary`, `generate-performance-analysis`, and `generate-brag-doc`. Read this file in full and follow it exactly before retrieving any data or generating a report.

## Connection Validation (MANDATORY FIRST STEP)

**Before proceeding with ANY data retrieval or report generation, you MUST:**

1. **Test Atlassian MCP connection:**
   - Call `mcp_Atlassian-MCP-Server_getAccessibleAtlassianResources` immediately
   - If the call fails or returns an error, **STOP immediately** and inform the user

2. **Test GitHub connection:**
   - Call `get_me` (GitHub plugin) immediately
   - If the call fails, returns an error, or the GitHub plugin isn't connected, **STOP immediately** and inform the user

3. **If either connection fails:**
   - **DO NOT proceed** with report generation
   - **DO NOT attempt** to retrieve Jira data
   - **DO NOT attempt** to retrieve GitHub or Slack data
   - **DO NOT generate** any reports or partial reports
   - **STOP all processing** and wait for user input
   - Inform the user which connection failed:
     - Jira: "Unable to connect to Jira. Please check your Atlassian MCP server connection. See `docs/SETUP.md` for configuration instructions."
     - GitHub: "Unable to connect to GitHub. Please connect the GitHub plugin. See `docs/SETUP.md` for configuration instructions."
   - Reference the setup documentation: The project's `.mcp.json` configures Atlassian automatically for both Cursor and Claude Code; GitHub is connected as a plugin. If connection fails, check:
     - Verify `.mcp.json` exists in the project root with the Atlassian MCP configuration
     - Verify the GitHub plugin is installed and authenticated (or, on GitHub Enterprise Cloud, that the fallback GitHub MCP server is configured in your global MCP config with a valid Personal Access Token)
     - Check your tool's MCP and plugin settings
     - Restart the tool completely after configuration changes
     - See `docs/SETUP.md` for complete setup instructions and troubleshooting
   - **Wait for the user to fix the connection before proceeding**

4. **If both connections succeed:**
   - Proceed with normal data retrieval below
   - Slack is an **optional automatic** evidence source: if the plugin/connection is unavailable or returns errors, skip that source silently, note it as "not connected" in the report's data-source context, and continue with the remaining sources. Only a failed Jira or GitHub connection blocks the whole run.
   - Google Drive is **never searched automatically**. It is only used to fetch a specific document when the user explicitly references it (see item 4 below).

**This check is mandatory and must happen before ANY data retrieval attempts (Jira, GitHub, OR Slack).**

---

## Inputs

**User provides:** Date range, role (Technical Writer L1-L4 or Technical Writing Manager L3-L6), level, and optional context.

**Brag docs** (`generate-brag-doc`) take a cadence and period instead; role defaults to Technical Writer and level is optional. Resolve the period to a date range and use it wherever this file says [[date range]].

## Automatic Retrieval

0. **`context/additional-context.local.md` (ALWAYS check — not just when the user mentions it):**
   - Read this file at the start of the workflow, before or alongside the Jira/GitHub retrieval below, for **every** work-summary, performance-analysis, and brag-doc request. If the file doesn't exist, skip silently (it's optional/git-ignored).
   - **Filter by period, per entry:** Parse each entry's `Date(s)` field and keep the entry only if at least one of its dates falls within the requested [[date range]] (inclusive of start and end). Skip entries entirely outside the period silently — do not mention skipped entries in the report. For entries with a date range (e.g., "Opened X, closed Y" or "X to Y"), keep the entry if that range overlaps the requested period at all.
   - Use each kept entry as supporting evidence in the report per its `Suggested competency linkage` (performance analysis) or matching work area (work summary) — same treatment as a user-referenced Drive doc: it counts toward evidence totals (see evidence-tracking rule in `generate-performance-analysis/SKILL.md`) but is never aggregated into Overview Metrics.
   - If any entry references a Drive/Slack/GitHub link, resolve it per the relevant source's rules below only if needed to enrich the bullet — the entry's own `Summary`/`Resolution/Outcome` fields are usually sufficient on their own.

1. **Jira activities** (Atlassian MCP):
   - Get Cloud ID: `mcp_Atlassian-MCP-Server_getAccessibleAtlassianResources`
   - Search: `mcp_Atlassian-MCP-Server_searchJiraIssuesUsingJql`
   - **Primary JQL** (use period **start** for the first date in each pair and period **end** for the second; the two `on` dates are period start and period end respectively):
     `(assignee = currentUser() OR assignee was currentUser() during ("YYYY-MM-DD", "YYYY-MM-DD")) AND (statusCategory changed to "In Progress" during ("YYYY-MM-DD", "YYYY-MM-DD") OR statusCategory was "In Progress" on "YYYY-MM-DD" OR statusCategory was "In Progress" on "YYYY-MM-DD" OR (resolved >= "YYYY-MM-DD" AND resolved <= "YYYY-MM-DD")) ORDER BY updated DESC`
   - **Optional scope-creep JQL** (run if needed; merge with primary results and dedupe by issue key): `assignee = currentUser() AND created >= "YYYY-MM-DD" AND created <= "YYYY-MM-DD" AND statusCategory != "In Progress" AND NOT statusCategory changed to "In Progress" during ("YYYY-MM-DD", "YYYY-MM-DD") ORDER BY created DESC`
   - If `statusCategory` is unavailable in your Jira instance, replace `statusCategory` clauses with explicit `status changed to` / `status was` using your workflow's in-progress status names (see `METRICS_GUIDE.md` → Technical Implementation Details).
   - **If the Primary JQL returns a 400 error** (some Jira Cloud instances reject the `changed to` / `was ... on` temporal operators on `statusCategory` specifically, even though the field itself exists and plain equality works): fall back to this broader query, then filter client-side —
     `assignee = currentUser() AND (status changed to "In Progress" DURING ("YYYY-MM-DD", "YYYY-MM-DD") OR statusCategory = "In Progress" OR (resolutiondate >= "YYYY-MM-DD" AND resolutiondate <= "YYYY-MM-DD")) ORDER BY updated DESC`
     This is intentionally wider than the Primary JQL — `statusCategory = "In Progress"` matches every issue currently in that category regardless of when it got there, not just ones touched in the period. After retrieving results, **keep only issues whose `updated` or `resolutiondate` falls inside [[date range]]**; drop the rest (they're old in-progress issues untouched this period). Note in the completion message that this fallback was used, since `updated` is a proxy for "was in progress during the period" rather than a direct confirmation.
   - Fields: `["summary", "description", "status", "issuetype", "priority", "created", "updated", "resolutiondate", "labels", "components", "parent", "changelog"]`
   - Extract "in progress" date: changelog → updated date → comment dates → created date (track fallback method)

2. **GitHub activities** (GitHub plugin — required source):
   - PRs authored: `search_pull_requests` with `author:@me created:YYYY-MM-DD..YYYY-MM-DD`
   - PRs reviewed: `search_pull_requests` with `reviewed-by:@me created:YYYY-MM-DD..YYYY-MM-DD`
   - Commits: `search_commits` with `author:@me committer-date:YYYY-MM-DD..YYYY-MM-DD`
   - Filter documentation files: `*.md`, `**/docs/**`, `**/documentation/**`, `README*`, `CONTRIBUTING*`

3. **Slack activities** (Slack plugin, if connected — optional evidence source):
   - Purpose: surface communication, collaboration, mentoring, and cross-team work that Jira/GitHub don't capture.
   - Messages/threads authored: `slack_search_public_and_private` (fallback: `slack_search_public` if private search isn't consented) with `from:@me after:YYYY-MM-DD before:YYYY-MM-DD`. If private-channel/DM search requires consent the user hasn't given, use `slack_search_public` only and note the narrower scope in the report.
   - Threads where the user replied/helped others: same search with `is:thread`, cross-checked against `slack_read_thread` for parent context to confirm the user's role (asker vs. answerer).
   - Mentions of Jira keys or PR numbers in messages: search for the literal key/number pattern (e.g., `EDU-`) `during:YYYY-MM..YYYY-MM` to link Slack discussion to specific issues/PRs.
   - Canvases authored/updated: search `has:file type:canvases from:@me` in the same date range; read with `slack_read_canvas` for content when linking to a work area.
   - Extract for each result: channel, timestamp, permalink, thread role (started/replied), and any Jira key/PR number mentioned.
   - If the Slack plugin is not connected/authenticated, skip this source (do not block the report) and note "Slack: not connected" in Overview Metrics.

4. **Google Drive activities** (Google Drive plugin — additional-context source ONLY, never searched automatically):
   - Purpose: let the user cite specific documentation artifacts (docs, guides, specs, decks) as supporting evidence, without running any automatic Drive-wide search.
   - **DO NOT** call `search_files` or `list_recent_files` to proactively discover documents. Google Drive is not part of automatic retrieval.
   - Only fetch a Drive file when the user explicitly references it by name, link, or file ID — either directly in the chat request or in a kept (in-period) entry of their `context/additional-context.local.md` file (see step 0 above).
   - Given a Drive URL or file ID from the user, call `get_file_metadata` and/or `read_file_content` (set `includeComments: true` if useful) to pull the title, summary, and last-modified date needed to write the supporting bullet.
   - If the user only names a document without a link/ID, ask them for the link/ID or the exact title before attempting any lookup — do not guess a `fileId`.
   - Treat each user-provided Drive doc as a single piece of supporting evidence tied to the work area/competency the user associates it with; do not aggregate Drive activity into Overview Metrics.
   - If the Google Drive plugin is not connected/authenticated when the user references a doc, note "Google Drive: not connected — couldn't fetch [title/link]" and proceed with the rest of the report using the user's own description of the document as evidence.

5. **Brag docs** (work-summary and performance-analysis requests only; a brag doc never reads other brag docs):
   - **Which files to read:** the files in `reports/[YYYY]/brag/` whose period overlaps the requested [[date range]]. Take each file's period from its filename (see `generate-brag-doc/SKILL.md` → Filenames) and check every year folder the range touches. If none exist, skip silently.
   - **Source of truth:** Jira and GitHub stay the source of truth. Merge brag bullets into the retrieved data by Jira key and PR number. Only brag content with no Jira or GitHub match (Slack, extra context) counts as new evidence, treated the same as a kept `additional-context.local.md` entry (step 0). Brag docs never feed the Overview Metrics.
   - **Competency tags:** treat them as hints for the Performance Analysis, not as ratings. Competencies that never appear in any brag doc during the period are worth flagging as gaps.

6. **Load expectations:**
   - Technical Writer → `context/technical-writer-career-path.json` (L1-L4). Use top-level `dimensions` to group granular `competencies` keys into report sections (`dimensions[].label`). Compare `levels[userLevel].competencies[key]` against evidence for each key listed in `dimensions[].competencies`.
   - Technical Writing Manager → `context/technical-writing-manager-career-path.json` (L3-L6). Competency keys are already dimension-level.

## Status Normalization (case-insensitive)

- **Completed:** Done, Resolved, Closed, Completed, Fixed, Verified, Deployed, Published, Released, Accepted, Approved, Merged, Shipped, Delivered, Finished, Finalized
- **In Progress:** In Progress, In Review, In Development, In Testing, Active, Working, Under Review, Reviewing, Developing, Testing, In Work, Assigned, Started, Open (if actively worked)
- **Blocked:** Blocked, On Hold, Waiting, Impediment, Paused, Deferred, Delayed, Stalled, Waiting for Input, Awaiting, Dependency, External Dependency, Needs Decision
- **Backlog:** Backlog, To Do, Open (if not actively worked), New, Created, Draft, Proposed, Requested
- **Rule:** "To Do" is always "Backlog", never "In Progress"

## Data Processing

- Normalize all statuses before processing
- Group by quarter (Q1=Jan-Mar, Q2=Apr-Jun, Q3=Jul-Sep, Q4=Oct-Dec)
- Cluster into work areas (priority: explicit grouping → components → labels → repositories → issue links → text similarity → project/epic)
- Work area validation: 3-15 issues per area (merge if <3, split if >20)
- Link Jira-GitHub: Parse PR descriptions for Jira keys (e.g., "EDU-123"), match by work area/theme
- Link Jira/GitHub-Slack: Parse Slack messages/threads for Jira keys or PR numbers; if none mentioned, match by work area/theme via text similarity against the message/thread content
- Link Jira/GitHub-Drive (user-provided docs only): If the user's referenced Drive doc's title/content mentions a Jira key, epic name, or component, fold it into that work area/bullet; otherwise use the work area/competency the user explicitly associated it with
- Deduplication: PR+Jira = combined bullet; standalone = separate bullet. Slack threads that clearly support an existing Jira/PR bullet are folded into that bullet's evidence (not a separate bullet); Slack activity with no Jira/GitHub match becomes its own bullet under "non-Jira activities" or the relevant work area. User-provided Drive docs are always added as supporting evidence for the work area/competency the user names, never auto-matched beyond that
- **Impact vs. Effort Analysis:** Correlate Jira priority/story points with GitHub lines changed. Flag high-priority issues with low output (<50 lines) or low-priority issues with massive PRs (>1000 lines) to identify potential over-engineering, invisible complexity, or misaligned priorities
- **Semantic Blocker Categorization:** Analyze blocker reasons from issue descriptions, comments, and labels using semantic grouping (e.g., "Awaiting API Specs", "Review Bottlenecks", "System Downtime", "Resource Constraints"). Identify recurring impediment patterns beyond status labels

## Non-Jira Activities to Include

- **Technical Writer:** Mentoring, community participation, process improvements, cross-team collaboration, strategy work, content audits, user research, speaking/presentations, guidelines/standards, tools/automation
- **Manager:** Team management, hiring/onboarding, decision-making, process leadership, stakeholder management, coaching/mentoring, strategy/roadmap, crisis leadership
- **Slack-sourced evidence (automatic):** Mentoring/helping others (threads where the user answered questions), cross-team collaboration (active participation in other teams' channels), announcements/process rollouts (messages authored in team-wide channels), async communication cadence
- **Drive-sourced evidence (only when the user provides a link/reference — never auto-searched):** Documentation strategy artifacts (specs, guides, style guides), content audits, presentations/decks, planning docs the user names in their request or in `context/additional-context.local.md`

## Date Interpretation

Include activities that fell within the user's responsibility during [[date range]], regardless of creation/completion dates.
