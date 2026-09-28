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

## Date Boundaries (read before writing any query)

Jira reads a bare `"YYYY-MM-DD"` as **midnight at the start of that day**, so `resolved <= "[[end]]"` silently excludes everything that happened during the last day of the period. A single-day period (any daily brag doc) returns nothing at all. To avoid this, every query below uses three resolved values:

- **`[[start]]`** — the period's first day, `YYYY-MM-DD`.
- **`[[end]]`** — the period's last day, `YYYY-MM-DD`. Used only for display, never as a query upper bound.
- **`[[end+1]]`** — the day *after* the period's last day, `YYYY-MM-DD`. Every upper bound uses this with a strict `<`, or as the `on`/`during` argument when the intent is "state at period end".

Rules: upper bounds are always `< "[[end+1]]"`, never `<= "[[end]]"`. `during (...)` windows always run `("[[start]]", "[[end+1]]")`. "State at period start" is `on "[[start]]"` (midnight before the first day); "state at period end" is `on "[[end+1]]"` (midnight after the last day), **not** `on "[[end]]"`. GitHub search is different — its `created:A..B` / `merged:A..B` ranges are inclusive of both whole days, so pass `[[start]]..[[end]]` there.

## Automatic Retrieval

0. **`context/additional-context.local.md` (ALWAYS check — not just when the user mentions it):**
   - Read this file at the start of the workflow, before or alongside the Jira/GitHub retrieval below, for **every** work-summary, performance-analysis, and brag-doc request. If the file doesn't exist, skip silently (it's optional/git-ignored).
   - **Filter by period, per entry:** Parse each entry's `Date(s)` field and keep the entry only if at least one of its dates falls within the requested [[date range]] (inclusive of start and end). Skip entries entirely outside the period silently — do not mention skipped entries in the report. For entries with a date range (e.g., "Opened X, closed Y" or "X to Y"), keep the entry if that range overlaps the requested period at all.
   - Use each kept entry as supporting evidence in the report per its `Suggested competency linkage` (performance analysis) or matching work area (work summary) — same treatment as a user-referenced Drive doc: it counts toward evidence totals (see evidence-tracking rule in `generate-performance-analysis/SKILL.md`) but is never aggregated into Overview Metrics.
   - If any entry references a Drive/Slack/GitHub link, resolve it per the relevant source's rules below only if needed to enrich the bullet — the entry's own `Summary`/`Resolution/Outcome` fields are usually sufficient on their own.

1. **Jira activities** (Atlassian MCP):
   - Get Cloud ID: `mcp_Atlassian-MCP-Server_getAccessibleAtlassianResources`
   - Search: `mcp_Atlassian-MCP-Server_searchJiraIssuesUsingJql`
   - **Primary JQL** (substitute the values defined in "Date Boundaries" above):
     `(assignee = currentUser() OR assignee was currentUser() during ("[[start]]", "[[end+1]]")) AND (statusCategory changed to "In Progress" during ("[[start]]", "[[end+1]]") OR statusCategory was "In Progress" on "[[start]]" OR statusCategory was "In Progress" on "[[end+1]]" OR (resolved >= "[[start]]" AND resolved < "[[end+1]]")) ORDER BY updated DESC`
   - **Optional scope-creep JQL** (run if needed): `assignee = currentUser() AND created >= "[[start]]" AND created < "[[end+1]]" AND statusCategory != "In Progress" AND NOT statusCategory changed to "In Progress" during ("[[start]]", "[[end+1]]") ORDER BY created DESC`
     - **Tag the provenance of every issue it returns as `scope-creep-supplement`.** This query deliberately returns only issues that were created in the period and *never* started — backlog additions, not work performed. Merge them into the issue set and dedupe by key, but they feed **only** the scope-creep metric. They are never counted in "Total issues worked on", "Issues completed", "Issues in progress", per-quarter metrics, or any work area, and they never produce an accomplishment bullet. Issues created inside the period that *did* start come from the Primary JQL and are the other half of the scope-creep count (see `generate-work-summary/SKILL.md` → Metrics Calculation).
   - If `statusCategory` is unavailable in your Jira instance, replace `statusCategory` clauses with explicit `status changed to` / `status was` using your workflow's in-progress status names (see `METRICS_GUIDE.md` → Technical Implementation Details).
   - **If the Primary JQL returns a 400 error** (some Jira Cloud instances reject the `changed to` / `was ... on` temporal operators on `statusCategory` specifically, even though the field itself exists and plain equality works): fall back to this broader query, then filter client-side —
     `assignee = currentUser() AND (status changed to "In Progress" DURING ("[[start]]", "[[end+1]]") OR statusCategory = "In Progress" OR (resolutiondate >= "[[start]]" AND resolutiondate < "[[end+1]]")) ORDER BY updated DESC`
     This is intentionally wider than the Primary JQL — `statusCategory = "In Progress"` matches every issue currently in that category regardless of when it got there, not just ones touched in the period. After retrieving results, **keep only issues whose `updated` or `resolutiondate` falls inside [[date range]]**; drop the rest (they're old in-progress issues untouched this period).
     - **The fallback changes what two metrics mean, so say so.** It drops `assignee was currentUser() during (...)`, so work owned during the period but since reassigned is missing entirely; and filtering on `updated` drops stale carryover (issues in progress all period with no update in it), which the Primary JQL captures on purpose. State both limitations in the completion message and add a one-line note under Overview Metrics: `*Retrieved via fallback JQL: reassigned work and untouched carryover may be missing.*`
   - **Paginate until the result set is complete.** The search endpoint returns one page per call. Keep requesting pages until you have every issue, and compare the number of issues you hold against the `total` the API reports. If you cannot retrieve them all, **do not publish the metrics as if they were complete** — say how many of how many issues the report is based on, in the completion message and under Overview Metrics.
   - Fields: `["summary", "description", "status", "issuetype", "priority", "created", "updated", "resolutiondate", "labels", "components", "parent"]`
   - **Changelog is a second call, not a search field.** `changelog` is not returned by the search endpoint; request it per issue (`getJiraIssue` with the changelog expanded). Do this for every issue that feeds a time-based metric — carryover, new issues started, average resolution time, issues in progress at period end. Resolve each issue once and reuse the result.
   - Extract "in progress" date: changelog → updated date → comment dates → created date. Track which method was used per issue; if the changelog was unavailable for any issue, note under Overview Metrics that `N` issues used a proxy date, since `updated` is a weak stand-in for "entered progress" and it inflates or deflates resolution time unpredictably.

2. **GitHub activities** (GitHub plugin — required source):
   - GitHub search ranges are inclusive of both whole days, so use `[[start]]..[[end]]` here (not `[[end+1]]`).
   - **Query by the event each metric is defined on, not by creation date.** A PR opened in December and merged in January belongs to January's "PRs merged"; a review given in March on a PR opened in February belongs to March's review count. Filtering everything by `created:` — as earlier versions of this file did — silently drops both, and understates the review-to-author ratio that three competencies depend on.
     - PRs authored/opened in the period: `search_pull_requests` with `author:@me created:[[start]]..[[end]]`
     - PRs merged in the period: `search_pull_requests` with `author:@me merged:[[start]]..[[end]]`
     - PRs authored and still open at period end: derive this from the authored set rather than querying `is:open`, which reports today's state and would be wrong for any past period. A PR counts as open at period end when it was created on or before `[[end]]` and was neither merged nor closed on or before `[[end]]`.
     - PRs reviewed in the period: `search_pull_requests` with `reviewed-by:@me updated:[[start]]..[[end]]`, then keep only those whose review by the user is dated inside [[date range]] (confirm with `pull_request_read` when the search result alone doesn't show the review date). Exclude self-authored PRs.
   - Commits: `search_commits` with `author:@me committer-date:[[start]]..[[end]]`
   - Paginate every search until complete, same rule as Jira above.
   - Filter documentation files: `*.md`, `**/docs/**`, `**/documentation/**`, `README*`, `CONTRIBUTING*`
   - **Lines changed and files modified need a per-PR call.** Search results don't carry additions/deletions or file lists. Fetch them with `pull_request_read` for the merged PRs that feed the metric; if that's not possible for all of them, report "Lines changed: N/A" rather than extrapolating from a subset.

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
