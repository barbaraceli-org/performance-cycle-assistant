# Setup & Usage Guide

Complete guide for setting up, using, and troubleshooting the Performance Cycle Report Assistant.

## Table of Contents

1. [Setup](#setup)
   - [Atlassian MCP (Jira)](#atlassian-rovo-mcp-configuration)
   - [GitHub Plugin (Required)](#github-plugin-configuration-required)
   - [Slack Plugin (Optional)](#slack-plugin-configuration-optional)
   - [Google Drive Plugin (Optional)](#google-drive-plugin-configuration-optional)
2. [How to Use](#how-to-use)
3. [Understanding Your Reports](#understanding-your-reports)
4. [GitHub Integration](#github-integration)
5. [Slack Integration](#slack-integration)
6. [Google Drive Integration](#google-drive-integration)
7. [Troubleshooting](#troubleshooting)
8. [Customization](#customization)
9. [Best Practices](#best-practices)

---

## Setup

This project integrates with these data sources:
- **Atlassian MCP** (required): Fetches Jira data
- **GitHub plugin** (required, automatic): Fetches PR, commit, and review data
- **Slack plugin** (optional, automatic): Fetches messages/threads showing communication, mentoring, and collaboration
- **Google Drive plugin** (optional, **additional-context only**): Fetches a specific Doc/Slide/Sheet only when you explicitly reference it — never searched automatically

> **✅ Automatic Setup:** This project includes `.mcp.json` which automatically configures the Atlassian Rovo MCP. GitHub, Slack, and Google Drive are connected as **Cursor Plugins** (a different mechanism from `.mcp.json`) — see their dedicated sections below.
>
> Jira and GitHub are **required and automatic**: if either is not connected, the assistant stops and asks you to fix the connection instead of generating a partial report. Slack is **optional and automatic**: if it's not connected, the assistant skips it silently and generates the report from the remaining sources. Google Drive is **optional and manual**: the assistant never searches your Drive on its own; it only fetches a document when you name it or paste its link, the same way you'd add other [additional context](../README.md#additional-context-local-only).

---

## Atlassian Rovo MCP Configuration

The Atlassian Rovo MCP (Model Context Protocol) server automatically fetches Jira and Confluence data.

### Prerequisites

- Cursor IDE (latest version)
- Jira Cloud account with access to your workspace
- Atlassian account with appropriate permissions

### Automatic Configuration

The `.mcp.json` file in this project automatically configures the Atlassian MCP when you open the project in Cursor:

```json
{
  "mcpServers": {
    "Atlassian-MCP-Server": {
      "url": "https://mcp.atlassian.com/v1/sse"
    }
  }
}
```

**No additional setup needed!** Cursor will handle authentication automatically through your Atlassian account.

### Verify Configuration

Test the connection in Cursor Chat:

```
Show me my recent Jira issues
```

If configured correctly, you should see a list of your Jira issues.

---

## Manual Configuration (Only if Needed)

If the automatic configuration doesn't work, you can manually configure the MCP:

### Step 1: Open Cursor's MCP Settings Panel

1. **Open Cursor Settings:**
   - Windows/Linux: `Ctrl+,`
   - Mac: `Cmd+,`

2. **Navigate to MCP Settings:**
   - Search for "MCP" in settings
   - Or go to: Settings → Features → Model Context Protocol
   - Click "Edit Config" to open the MCP configuration

### Step 2: Add Atlassian Rovo MCP Configuration

Add the following configuration to your MCP settings:

**For latest Cursor versions:**

```json
{
  "mcpServers": {
    "Atlassian-MCP-Server": {
      "url": "https://mcp.atlassian.com/v1/sse"
    }
  }
}
```

**For older Cursor versions:**

If the above doesn't work, try this alternative configuration:

```json
{
  "mcpServers": {
    "mcp-atlassian-api": {
      "command": "npx",
      "args": [
        "mcp-remote",
        "https://mcp.atlassian.com/v1/sse"
      ]
    }
  }
}
```

### Step 3: Save and Restart

1. **Save** the configuration
2. **Restart** Cursor's AI assistant or tools pane
3. Cursor will handle authentication automatically through your Atlassian account

### Alternative: Direct Configuration File Edit

You can also edit the MCP config file directly:

**Windows:**
```
%APPDATA%\Cursor\User\globalStorage\mcp-config.json
```

**Mac/Linux:**
```
~/.config/Cursor/User/globalStorage/mcp-config.json
```

Add the same JSON configuration as shown in Step 2.

## Troubleshooting

### "Atlassian MCP not found" or "Connection failed"

**Solution:**
- Ensure you've restarted Cursor after configuration
- Verify you're using the correct configuration format for your Cursor version
- Try the alternative configuration (older version format)
- Check your internet connection

### "Authentication failed"

**Solution:**
- Cursor will prompt for Atlassian authentication automatically
- Ensure you're logged into your Atlassian account in your browser
- Try logging out and back into Atlassian
- Clear Cursor's cache and restart

### "No issues found"

**Solution:**
- Verify you have Jira issues assigned to you
- Check the date range in your query
- Ensure you have appropriate Jira permissions
- Test with a simpler query: "Show me my Jira issues"

### "Permission denied"

**Solution:**
- Verify your Atlassian account has Jira access
- Check with your Jira admin about your permissions
- Ensure your organization allows MCP connections
- Verify you're accessing the correct Atlassian workspace

### Configuration not working?

**Solution:**
- Check Cursor version: Help → About
- Visit [Cursor MCP documentation](https://docs.cursor.com/advanced/model-context-protocol) for latest updates
- Try both configuration formats (latest and older versions)
- Restart Cursor completely (not just the AI assistant)

## Security Best Practices

1. **Authentication is automatic** - No need to store API tokens manually
2. **Review MCP permissions** - Understand what data the MCP can access
3. **Use organization SSO** - If available, for better security
4. **Monitor access** - Check your Atlassian security settings regularly
5. **Revoke access if needed** - Through Atlassian account settings

## Additional Resources

- [Atlassian Rovo MCP](https://mcp.atlassian.com/) - Official Atlassian MCP server
- [Cursor MCP Documentation](https://docs.cursor.com/advanced/model-context-protocol) - Latest Cursor MCP setup
- [Atlassian Rovo Documentation](https://www.atlassian.com/software/rovo) - Learn about Atlassian Rovo
- [Model Context Protocol](https://modelcontextprotocol.io/) - MCP specification

> **Note:** Cursor updates frequently. Always check the [official Cursor MCP documentation](https://docs.cursor.com/advanced/model-context-protocol) for the latest supported features and setup advice.

---

## GitHub Plugin Configuration (Required)

The GitHub plugin fetches pull requests, commits, code reviews, and repository contributions. This is **required** — the assistant validates the GitHub connection before retrieving any data and won't generate a report without it.

> **📝 Use the plugin, not a token.** The GitHub plugin signs you in through GitHub's own authorization flow, so there's no Personal Access Token to generate, store, rotate, or revoke. This is the same mechanism used by the Slack and Google Drive plugins below. A token-based fallback for GitHub Enterprise Cloud is documented at the end of this section.

### What Gets Tracked

When the GitHub plugin is connected, the assistant automatically fetches:
- Pull requests you authored in the review period (merged, open, closed)
- Pull requests you reviewed for others
- Commits to documentation files
- The repositories you contributed to

See [GitHub Integration](#github-integration) for the full breakdown and how this work appears in reports.

### Prerequisites

- GitHub account with access to relevant repositories
- The GitHub plugin enabled and connected in Cursor

### Step 1: Enable and Connect the GitHub Plugin

1. Open **Cursor Settings** (`Ctrl+,`)
2. Navigate to the plugins/integrations panel (Settings → Tools & Integrations, or Settings → Features → Plugins, depending on your Cursor version)
3. Find **GitHub** in the available plugins and enable it
4. Follow the prompt to sign in to GitHub and authorize access to the organizations whose repositories you need
5. Restart Cursor's AI assistant/tools pane if prompted

> **Organization access:** If your documentation repos live in an org that requires approval for third-party access, the authorization step may need an org admin to approve it. Without that approval the plugin connects but returns no results for those repos.

### Step 2: Verify Configuration

Test the connection in Cursor Chat:

```
Show me my recent GitHub pull requests
```

If configured correctly, you should see your recent PRs.

### Troubleshooting the GitHub Plugin

**"GitHub plugin not found" or tools unavailable:**
- Verify the plugin is enabled in Cursor Settings and shows as connected
- Restart Cursor completely (not just the chat pane)
- Re-authenticate: disconnect and reconnect the GitHub plugin

**"No pull requests found":**
- Verify you have PRs in the date range
- Check repository permissions, and confirm org access was approved during authorization
- Try a broader query first (e.g., "Show me my GitHub activity") to confirm the connection works
- Ensure you're authenticated to the correct GitHub account

**Report generation stops with "Unable to connect to GitHub":**
- GitHub is a required source, so the assistant stops rather than generating a partial report. Reconnect the plugin and request the report again.

### Fallback: GitHub MCP Server with a Personal Access Token

Use this only if the plugin can't reach your GitHub instance — most commonly on **GitHub Enterprise Cloud**. It requires you to manage a token yourself, which is why it isn't the default.

**⚠️ NEVER commit GitHub tokens to Git.** Configure this in your **global** Cursor MCP configuration file (`~/.cursor/mcp.json` on Mac/Linux, `C:\Users\YourName\.cursor\mcp.json` on Windows), which lives outside the project directory — never in the project's `.mcp.json`.

1. Generate a token at https://github.com/settings/tokens with the `repo`, `read:org`, and `read:user` scopes, and copy it immediately.
2. Add a `github` entry to the `mcpServers` object in your global config:

```json
{
  "mcpServers": {
    "github": {
      "url": "https://copilot-api.your-enterprise.ghe.com/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_GITHUB_PAT"
      }
    }
  }
}
```

   Use `https://api.githubcopilot.com/mcp/` as the URL for github.com rather than Enterprise Cloud. Keep the `Bearer ` prefix; tokens start with `ghp_` (classic) or `github_pat_` (fine-grained).
3. Save the file, quit Cursor completely, and restart.

**Token hygiene:** rotate tokens periodically, prefer fine-grained tokens with minimal scopes, never share the config file or paste tokens into screenshots, and if a token is exposed, revoke it at https://github.com/settings/tokens before issuing a replacement.

**If the token route fails:** check that the token hasn't expired, that the JSON syntax is valid, that the `Bearer ` prefix and scopes are correct, and that you fully restarted Cursor.

> **Note:** When you use this fallback, the assistant's GitHub tools are namespaced under the server name instead of the plugin. The retrieval steps in `.claude/skills/_shared/data-collection.md` name the plugin's tools (`get_me`, `search_pull_requests`, `search_commits`); the equivalents here are the same tools exposed by the `github` MCP server.

### Alternative: GitHub CLI Authentication

If you have GitHub CLI installed, you can use it as another token-free option, though the plugin above is the recommended path.

If you prefer GitHub CLI, add this to your global `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "github": {
      "command": "gh",
      "args": ["api", "--paginate"]
    }
  }
}
```

Then authenticate with GitHub CLI:
```powershell
# Install GitHub CLI (Windows)
winget install --id GitHub.cli

# Authenticate
gh auth login

# Verify
gh auth status
```

---

## Slack Plugin Configuration (Optional)

The Slack plugin fetches messages, threads, and canvases to surface communication, mentoring, and cross-team collaboration that Jira/GitHub don't capture. This is **optional** but recommended for technical writers whose collaboration happens primarily in Slack.

### What Gets Tracked

When the Slack plugin is connected, the assistant automatically fetches:
- Messages/threads you authored in the review period
- Threads where you replied to someone else's question (mentoring/help signal)
- Messages mentioning a Jira key (e.g., "EDU-123") or PR number, to link discussion back to specific work areas
- Canvases you authored or updated

### Prerequisites

- A Slack workspace account
- The Slack plugin enabled and connected in Cursor

### Step 1: Enable and Connect the Slack Plugin

1. Open **Cursor Settings** (`Ctrl+,`)
2. Navigate to the plugins/integrations panel (Settings → Tools & Integrations, or Settings → Features → Plugins, depending on your Cursor version)
3. Find **Slack** in the available plugins and enable it
4. Follow the prompt to sign in to your Slack workspace and authorize access
5. Restart Cursor's AI assistant/tools pane if prompted

### Step 2: Verify Configuration

Test the connection in Cursor Chat:

```
Search Slack for my recent messages
```

If configured correctly, you should see a list of your recent Slack messages/threads.

### Privacy Note

- Public-channel search (`slack_search_public`) does not require extra consent.
- Searching private channels and DMs (`slack_search_public_and_private`) requires your explicit consent in Cursor. If you don't grant it, the assistant falls back to public-channel search only and notes the narrower scope in the report.

### Troubleshooting Slack Plugin

**"Slack plugin not found" or tools unavailable:**
- Verify the plugin is enabled in Cursor Settings and shows as connected
- Restart Cursor completely (not just the chat pane)
- Re-authenticate: disconnect and reconnect the Slack plugin

**"No messages found":**
- Verify you have activity in the requested date range
- Try a broader query first (e.g., "Show me my recent Slack activity") to confirm the connection works
- Check whether private/DM search consent is needed for the content you're looking for

**Report shows "Slack: not connected":**
- This means the assistant detected the plugin was unavailable and skipped it — reports still generate normally using the remaining sources. Reconnect the plugin and regenerate if you want Slack evidence included.

---

## Google Drive Plugin Configuration (Optional, Additional Context Only)

The Google Drive plugin lets you cite a specific Doc, Slide, or Sheet as supporting evidence for a work area or competency. **The assistant never searches your Drive automatically** — it only fetches a document when you explicitly name it or paste its link, exactly like the manual entries in `context/additional-context.local.md`.

### What Gets Fetched

The assistant only touches Google Drive when you reference a document, for example:
```
Include this doc as evidence for the API Documentation work area:
https://docs.google.com/document/d/1AbCdEfGhIjKlMnOpQrStUvWxYz/edit
```
or by mentioning it in your `context/additional-context.local.md` file. When referenced, the assistant fetches:
- The document's title and last-modified date (metadata)
- A content summary (via `read_file_content`), used to write a one-line supporting-evidence bullet

No automatic search, listing, or aggregation of your Drive files ever happens.

### Prerequisites

- A Google account with Drive access
- The Google Drive plugin enabled and connected in Cursor (only needed at the moment you reference a doc)

### Step 1: Enable and Connect the Google Drive Plugin

1. Open **Cursor Settings** (`Ctrl+,`)
2. Navigate to the plugins/integrations panel (Settings → Tools & Integrations, or Settings → Features → Plugins, depending on your Cursor version)
3. Find **Google Drive** in the available plugins and enable it
4. Follow the prompt to sign in to your Google account and authorize Drive access
5. Restart Cursor's AI assistant/tools pane if prompted

### Step 2: Verify Configuration

Test the connection in Cursor Chat by referencing one specific file you own:

```
Read this Google Doc for me: <paste a Drive link>
```

If configured correctly, you should see a summary of that document's content.

### Troubleshooting Google Drive Plugin

**"Google Drive plugin not found" or tools unavailable:**
- Verify the plugin is enabled in Cursor Settings and shows as connected
- Restart Cursor completely (not just the chat pane)
- Re-authenticate: disconnect and reconnect the Google Drive plugin

**"File not found" when referencing a doc:**
- Double-check the link/ID you provided is correct and you have access to the file
- Try pasting the full Drive URL rather than just the title

**Report shows "Google Drive: not connected — couldn't fetch [title/link]":**
- This means you referenced a doc but the plugin was unavailable — the report still generates normally, using your own description of the document as evidence instead. Reconnect the plugin and regenerate if you want the fetched content included.

---

## How to Use

> **Note:** The Atlassian Rovo MCP is automatically configured via `.mcp.json` in this project. No setup required!
>
> **Callout:** Always state whether you are an individual contributor or a manager. The IC (Technical Writer) track progresses up to **L4**; the manager track starts at **L3** and goes up to **L6**. Use "technical writer", "tech writer", "ic", or "individual collaborator" for IC roles, and "technical writing manager" or "manager" for manager roles.
>
> **Frameworks:** IC requests use `context/technical-writer-career-path.json`; manager requests use `context/technical-writing-manager-career-path.json` (includes Management expectations). Your stated role selects the correct competencies for analysis.

### Basic Usage

1. **Open Cursor Chat:** `Ctrl+L` or `Cmd+L`

2. **Make your request:**

   ```
   Generate my performance cycle report for 2025-01-01 to 2025-06-30.
   I'm a Level 3 Technical Writer.
   ```

   > **Tip:** Jira and GitHub data are fetched automatically. Mention additional activities not tracked in systems (mentoring, presentations, process improvements, team outcomes, etc.).

3. **Review and refine:**
   - Ask for more detail: "Expand the Q2 accomplishments"
   - Add context: "Include these activities: [list]"
   - Focus areas: "Add more detail to Communication competency"

### What Happens Automatically

The assistant will:
1. **Validate Jira and GitHub connections** (automatic check before proceeding)
   - Tests the Atlassian MCP and GitHub plugin connections
   - **If either connection fails, stops immediately and does NOT proceed with any data retrieval or report generation**
   - Provides setup instructions and references this guide for configuration help
   - **Waits for you to fix the connection before proceeding**
2. **Only if both connections succeed:**
   - Fetch your Jira issues (created, updated, or resolved in date range)
   - Fetch your GitHub activity: PRs, commits, reviews
   - Fetch your Slack activity (if the plugin is connected): messages, threads, mentoring/help signals
   - Fetch any specific Google Drive document you referenced by name/link (never searched automatically)
   - Group by calendar quarters (Q1-Q4)
   - Cluster into work areas (based on components, labels, themes, repositories)
   - Generate accomplishment bullets per area
   - Identify unfinished tasks
   - Rate each competency (Not yet meeting expectations / Meets expectations / Exceeds expectations / Performing at the next level) with evidence and actionable improvement steps
   - Save two separate reports to `reports/[YYYY]/` (year from period start date; create subfolder if needed):
     - `work-summary-[date-range].md`
     - `performance-analysis-[date-range].md`

### Brag Documents

Brag docs need the same connections as the performance-cycle reports: Jira and GitHub are validated first and are required; Slack and Google Drive stay optional. Role defaults to Technical Writer and level is optional.

They also read `context/hibob-goals.local.md` to draft the Hibob goals update. Create it from [examples/hibob-goals.example.md](../examples/hibob-goals.example.md) and keep it in sync with Bob; if it's missing, the brag doc skips the Hibob section.

```
Generate my weekly brag doc for last week.
```

Brag docs are saved to `reports/[YYYY]/brag/`. See the [README](../README.md#brag-documents) for cadences and filenames.

### Tips for Best Results

1. **Keep Jira updated** - Add meaningful descriptions, labels, and components
2. **Provide context** - Mention non-Jira/GitHub activities, special projects, challenges
3. **Be specific** - State your role and exact level (Technical Writer L1/L2/L3/L4 or Technical Writing Manager L3/L4/L5/L6)
4. **Review and iterate** - Ask for refinements or additional detail

### Advanced Usage

**Compare periods:**
```
Generate reports for Q1 and Q2 2025. Highlight differences.
```

**Custom focus:**
```
Generate my 2025 report (L3) with emphasis on:
- Technical leadership activities
- Cross-functional collaboration
- Documentation strategy initiatives
```

**Iterative refinement:**
```
Expand the "Communication" competency section with more specific examples.
```

---

## Understanding Your Reports

Reports are saved in year-based subfolders under `reports/` (git-ignored). The folder name is the calendar year of the period **start** date — for example, a Q1 2025 report goes in `reports/2025/`, and a period spanning November 2025 through February 2026 also goes in `reports/2025/`.

### Regenerating a report for the same period

Filenames are derived from the report type and date range, so regenerating a period targets the file that already exists. The policy is:

- **Overwrite by default** — no `-v2` suffixes or timestamped copies are created automatically.
- **You're asked first** if the existing report is more than 7 days old (it may already have been shared or annotated) or if it looks hand-edited.
- **If you choose to keep both**, the new report is saved as `[report-type]-[date-range]-[YYYY-MM-DD].md`, where the suffix is the regeneration date. The original is never deleted.
- The assistant always states the full path and whether the file was created, overwritten, or saved alongside an existing one.

Because `reports/` is git-ignored, there's no version history for these files — if you want to keep a specific version, ask for it to be saved alongside rather than overwritten.

### Work Summary Report

**Structure:**
- **Overview Metrics** - High-level statistics
- **Accomplishments** - Organized by quarter and work area
- **What couldn't be finished** - Unfinished tasks with blocker analysis

**Use for:**
- Performance review conversations
- Tracking quarterly progress
- Identifying blockers and dependencies

### Performance Analysis Report

**Structure:**
- **Evaluation scale** - Not yet meeting expectations (1), Meets expectations (2), Exceeds expectations (3), Performing at the next level (4) — defined at the top of the report
- **Per competency** - Explicit rating, rationale, supporting evidence (Jira/GitHub links), and actionable steps to improve
- **Summary** - Evaluation overview table, cross-cutting themes, and priority development focus

**Evaluation meanings:**

| # | Rating | Meaning |
| --- | --- | --- |
| 1 | Not yet meeting expectations | No consistent examples of achieving this ability in the period |
| 2 | Meets expectations | Consistent examples (typically ≥3) of achieving this ability at the scope/complexity expected for the role/level |
| 3 | Exceeds expectations | Consistent examples plus scope, complexity, or impact beyond what's expected at that level (e.g., higher-complexity work, cross-team scope, measurable efficiency gains). Recognition by others can support the rating but isn't sufficient on its own |
| 4 | Performing at the next level | Exceeds expectations, with evidence matching the next career level's competency expectations (not assignable at the top level, L4 for writers / L6 for managers) |

**Use for:**
- Career development planning
- Identifying skill gaps and concrete next steps
- Setting growth goals tied to moving up the evaluation scale

### Metrics Explained

See **[METRICS_GUIDE.md](../METRICS_GUIDE.md)** for complete details on:
- What each metric means
- How metrics are calculated
- Interpreting your results
- Good ranges and benchmarks

**Advanced Metrics (December 2025):**
- **Carryover & Scope Creep Analysis** - Contextualizes completion rates by distinguishing planned vs. inherited vs. reactive work
- **Review-to-Author Ratio** - Measures "Force Multiplier" behavior through peer review contributions
- **Impact vs. Effort Correlation** - Flags high-priority work with low output or low-priority work with massive changes
- **Semantic Blocker Categorization** - Root cause analysis of impediments using AI to identify patterns beyond status labels

---

## GitHub Integration

The GitHub integration automatically captures your documentation work in code repositories.

### What Gets Tracked

The assistant automatically fetches:

1. **Pull Requests Authored**
   - All PRs you created during the review period
   - Status: merged, open, closed
   - Files changed, lines added/deleted
   - Merge time metrics

2. **Pull Requests Reviewed**
   - PRs where you provided reviews
   - Review types: approve, request changes, comment
   - Repositories and teams you supported

3. **Documentation Commits**
   - Commits to documentation files:
     - `*.md` files
     - `docs/` directories
     - `README*` files
     - `CONTRIBUTING*` files
     - API documentation
   - Lines changed (additions/deletions)
   - Repositories modified

4. **Repository Contributions**
   - All repositories you contributed to
   - Breadth of impact across projects
   - Cross-team collaboration

### Filtered Content

The integration **only tracks documentation-related work**:
- ✅ Markdown files
- ✅ Documentation folders
- ✅ README updates
- ✅ API docs
- ❌ Code files (*.js, *.py, etc.)
- ❌ Configuration files
- ❌ Build files

### How GitHub Work Appears in Reports

GitHub work is included in your accomplishments, and GitHub metrics are displayed in quarterly summaries. Only Jira metrics are shown in the Overview Metrics section at the top of the report.

**Example accomplishments:**
```markdown
#### API Documentation
**Metrics:** 15 completed | 2 in progress | Avg resolution: 6 days

**Accomplishments:**
- Authored comprehensive API reference for Payment Gateway (PR #234)
- Reviewed 5 API documentation PRs from engineering team
- Updated 23 endpoint descriptions with improved examples
- Merged 8 PRs in developer-docs repo updating authentication docs
- Fixed broken links in README files across 3 repositories
```

**Per-quarter breakdown:**
```markdown
### Quarter 2
**Q2 Metrics:** 38 issues completed | 5 in progress | 88% completion rate
**GitHub:** 12 PRs merged | 8 reviews | 3 repositories

#### API Documentation
**Metrics:** 15 completed | 2 in progress | Avg resolution: 6 days

**Accomplishments:**
- Authored comprehensive API reference for Payment Gateway (PR #234)
- Reviewed 5 API documentation PRs from engineering team
```

### Benefits

- **More complete picture**: Captures in-repo work and documentation that lives in code repositories
- **Shows collaboration**: Code reviews demonstrate technical expertise
- **Demonstrates breadth**: Contributions across multiple repositories
- **Proves impact**: Work is included in accomplishments with PR references

### Usage Examples

**Basic request (auto-includes GitHub):**
```
Generate my Q2 2025 report.
I'm a Level 3 Technical Writer.
```

**Emphasize GitHub work:**
```
Generate my H1 2025 report.
I'm L2 IC.

Focus on:
- API documentation PRs in the developer-docs repo
- README improvements across repositories
- Documentation reviews for the engineering team
```

> **Note:** GitHub can't be skipped. It's a required source, so the assistant always fetches it and stops if the connection fails.

---

## Slack Integration

The Slack plugin integration automatically captures communication, mentoring, and cross-team collaboration evidence when connected.

### What Gets Tracked

When the Slack plugin is connected, the assistant automatically fetches:

1. **Messages/Threads Authored**
   - Messages/threads you posted during the review period
   - Channel and permalink for each result

2. **Threads Helped/Answered**
   - Threads started by someone else where you replied
   - Used to compute the thread-help ratio (mentoring signal)

3. **Jira/PR Mentions**
   - Messages mentioning a Jira key (e.g., "EDU-123") or PR number
   - Linked back to the corresponding work area/bullet

4. **Canvases**
   - Canvases you authored or updated, when relevant to a work area

### How Slack Work Appears in Reports

Slack-sourced evidence is folded into existing Jira/GitHub bullets when a Jira key or PR number is mentioned, or added as a standalone "non-Jira activity" bullet (e.g., mentoring, cross-team support) when it isn't. Slack metrics appear in the Overview Metrics section (if the plugin is connected) and in the "Communication"/"Collaboration" competency evidence.

**Example accomplishment:**
```markdown
- Helped unblock 4 teammates on API documentation formatting questions across #dev-docs threads
```

**Optional: Disable Slack integration temporarily:**
```
Generate my Q2 report. I'm L2 IC.
Skip Slack, use only Jira and GitHub.
```

---

## Google Drive Integration

Unlike GitHub and Slack, the Google Drive plugin is **never used for automatic retrieval**. It exists purely to let you cite a specific Doc/Slide/Sheet as evidence — the same role as `context/additional-context.local.md`, but with the assistant fetching the actual content for you instead of you pasting it in.

### What Gets Fetched (Only When You Reference a Document)

1. **A named/linked document**
   - You provide a title, file ID, or Drive link in your request, or list it in `context/additional-context.local.md`
   - The assistant fetches title, last-modified date, and a content summary for that one file

2. **No automatic discovery**
   - The assistant never calls Drive-wide search or "list recent files" to find evidence on its own
   - If you don't reference a document, Google Drive is not touched at all during report generation

### How Google Drive Work Appears in Reports

A user-referenced Drive doc becomes a single supporting-evidence bullet under the work area/competency you associate it with. There is no "Google Drive Activity" Overview Metrics section and no aggregate counts — it's evidence, not a tracked metric source.

**Example request:**
```
Generate my Q2 report. I'm L2 IC.

Also include this doc as evidence for Documentation Strategy:
https://docs.google.com/document/d/1AbCdEfGhIjKlMnOpQrStUvWxYz/edit
```

**Example resulting accomplishment:**
```markdown
- Authored the Q2 documentation style guide (Google Doc), later adopted across 3 work areas
```

**If you don't reference any Drive documents, Google Drive is simply not part of the report — no setup or connection is required.**

**Optional: Disable Google Drive integration temporarily:**
```
Generate my Q2 report. I'm L2 IC.
Skip Google Drive, use only Jira and GitHub.
```

---

## Customization

### For Your Organization

1. **Replace or extend the competency frameworks:**
   - Edit `context/technical-writer-career-path.json` (writers)
   - Edit `context/technical-writing-manager-career-path.json` (managers L3–L6)
   - Add your organization's levels and expectations

2. **Adjust metrics:**
   - Edit the "Metrics Calculation" section of `.claude/skills/generate-work-summary/SKILL.md`
   - Customize which metrics to include/exclude

3. **Modify report structure:**
   - Edit the "Structure" section of `.claude/skills/generate-work-summary/SKILL.md` or `.claude/skills/generate-performance-analysis/SKILL.md`
   - Adjust bullet counts, sections, or formatting

4. **Adjust retrieval or writing rules shared by both reports:**
   - JQL, status normalization, work-area clustering: `.claude/skills/_shared/data-collection.md`
   - Tone, output location, report regeneration policy: `.claude/skills/_shared/writing-standards.md`

> **Note:** There's nothing to regenerate after these edits. Cursor and Claude Code both read `AGENTS.md` and `.claude/skills/` directly, so a change in one place applies to both tools on the next request.

### For Your Workflow

**Add custom work areas:**
```
Group my work into these areas:
- API Documentation
- Developer Guides
- Release Notes
- Internal Documentation
```

**Focus on specific competencies:**
```
Generate my report with extra detail on:
- Technical Writing competency
- Communication competency
```

---

## Best Practices

### Keep Jira Updated

- ✅ Use meaningful issue titles
- ✅ Add descriptions with context
- ✅ Apply relevant labels and components
- ✅ Update status promptly
- ✅ Link related issues

### Provide Context

Include non-Jira/GitHub activities:
- Presentations and workshops
- Mentoring and training
- Process improvements
- Cross-team collaboration
- Learning and certifications

### Review and Refine

- Read through generated reports
- Ask for clarifications or expansions
- Add missing context
- Request specific examples
- Iterate until satisfied

---

## Common Questions

**Q: Can I generate reports for past years?**  
A: Yes! Use any date range. The assistant will fetch Jira data for that period.

**Q: What if I don't have Jira data?**  
A: Provide your activities manually in the request. The assistant will still generate structured reports.

**Q: Can I compare my performance across multiple periods?**  
A: Yes! Request multiple reports and ask for comparison analysis.

**Q: How do I share reports with my manager?**  
A: Reports are saved as Markdown files in `reports/[YYYY]/` (e.g., `reports/2025/work-summary-Q1-2025.md`). Share them directly or convert to PDF.

**Q: Can I customize the competency framework?**  
A: Yes! Replace `context/technical-writer-career-path.json` with your organization's framework.

**Q: What if my level isn't L1-L4 (writers) or L3-L6 (managers)?**  
A: Update the JSON file with your organization's levels and expectations.

---

## Next Steps

- **[Metrics Guide](../METRICS_GUIDE.md)** - Understand all metrics (basic + advanced)
- **[Example work summary](../examples/example-report-with-metrics.md)** - See a complete example report
- **[Example performance analysis](../examples/example-performance-analysis.md)** - See a complete competency evaluation
- **[README](../README.md)** - Project overview and quick start
- **[Changelog](../CHANGELOG.md)** - Release history and updates

