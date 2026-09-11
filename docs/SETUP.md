# Setup & Usage Guide

Complete guide for setting up, using, and troubleshooting the Performance Cycle Report Assistant.

## Table of Contents

1. [Setup](#setup)
   - [Atlassian MCP (Jira)](#atlassian-rovo-mcp-configuration)
   - [GitHub MCP (Optional)](#github-mcp-configuration-optional)
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
- **GitHub MCP** (optional, automatic): Fetches PR, commit, and review data
- **Slack plugin** (optional, automatic): Fetches messages/threads showing communication, mentoring, and collaboration
- **Google Drive plugin** (optional, **additional-context only**): Fetches a specific Doc/Slide/Sheet only when you explicitly reference it — never searched automatically

> **✅ Automatic Setup:** This project includes `mcp.json` which automatically configures the Atlassian Rovo MCP. For GitHub MCP, you need to configure it in your global Cursor MCP configuration file (see below). Slack and Google Drive are connected as **Cursor Plugins** (a different mechanism from `mcp.json`) — see their dedicated sections below.
>
> GitHub and Slack are **optional and automatic**: if either is not connected, the assistant skips it silently and generates the report from the remaining sources — only a failed Jira connection blocks report generation. Google Drive is **optional and manual**: the assistant never searches your Drive on its own; it only fetches a document when you name it or paste its link, the same way you'd add other [additional context](../README.md#additional-context-local-only).

---

## Atlassian Rovo MCP Configuration

The Atlassian Rovo MCP (Model Context Protocol) server automatically fetches Jira and Confluence data.

### Prerequisites

- Cursor IDE (latest version)
- Jira Cloud account with access to your workspace
- Atlassian account with appropriate permissions

### Automatic Configuration

The `mcp.json` file in this project automatically configures the Atlassian MCP when you open the project in Cursor:

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

## GitHub MCP Configuration (Optional)

The GitHub MCP server fetches pull requests, commits, code reviews, and repository contributions. This is **optional** but recommended for technical writers who work in code repositories.

> **📝 Global Configuration Required:** GitHub MCP must be configured in your **global** Cursor MCP configuration file (`~/.cursor/mcp.json` on Mac/Linux, `C:\Users\YourName\.cursor\mcp.json` on Windows). This keeps your token secure and makes GitHub MCP available across all projects.

### Prerequisites

- GitHub account with access to relevant repositories
- GitHub Personal Access Token (PAT)

### Security: Token Management

**⚠️ NEVER commit GitHub tokens to Git!**

The GitHub MCP server requires authentication using a Personal Access Token. You'll add the token directly to your global MCP configuration file, which is stored outside your project directory and never committed to Git.

### Step 1: Generate a GitHub Personal Access Token

1. Go to https://github.com/settings/tokens
2. Click **"Generate new token (classic)"**
3. Give it a descriptive name (e.g., "Cursor Performance Cycle Reports")
4. Select scopes:
   - ✅ `repo` - Full control of private repositories
   - ✅ `read:org` - Read org and team membership
   - ✅ `read:user` - Read user profile data
5. Click **"Generate token"**
6. **Copy the token immediately** (you won't see it again)

### Step 2: Configure GitHub MCP in Global Settings

**Locate your global MCP configuration file:**

- **Windows:** `C:\Users\YourName\.cursor\mcp.json`
- **Mac/Linux:** `~/.cursor/mcp.json`

**If the file doesn't exist, create it.**

**Add GitHub MCP configuration:**

Open the file and add the GitHub MCP server configuration. If you already have other MCP servers configured, add the `github` entry to the existing `mcpServers` object:

```json
{
  "mcpServers": {
    "github": {
      "url": "https://api.githubcopilot.com/mcp/",
      "headers": {
        "Authorization": "Bearer YOUR_GITHUB_PAT"
      }
    }
  }
}
```

**Replace `YOUR_GITHUB_PAT` with your actual Personal Access Token from Step 1.**

**Important:**
- Keep the `Bearer ` prefix
- Keep the quotes around the entire value
- Your token should start with `ghp_` (classic tokens) or `github_pat_` (fine-grained tokens)

**For GitHub Enterprise:**

If using GitHub Enterprise Cloud, change the URL:

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

### Step 3: Save and Restart Cursor

1. **Save** the global `mcp.json` file
2. **Close Cursor completely** (not just the window - fully quit the application)
3. **Restart Cursor**
4. The GitHub MCP will now be available in all your projects

### Verify GitHub Configuration

Test the connection in Cursor Chat:

```
Show me my recent GitHub pull requests
```

If configured correctly, you should see your recent PRs.

### Security Best Practices

**DO:**
- ✅ Store tokens in your global `~/.cursor/mcp.json` (not in project files)
- ✅ Add `.cursor/` to `.gitignore` if you version control your home directory
- ✅ Rotate tokens periodically
- ✅ Use fine-grained tokens with minimal scopes when possible
- ✅ Revoke tokens immediately if compromised
- ✅ Keep your global MCP config file private

**DON'T:**
- ❌ Commit tokens to Git (global config is outside project directory)
- ❌ Share tokens in screenshots or documentation
- ❌ Use tokens with more permissions than needed
- ❌ Store tokens in project-level `mcp.json` files
- ❌ Share your global MCP configuration file

### Troubleshooting GitHub MCP

#### "GitHub MCP not found"

**Solution:**
- Verify GitHub MCP is configured in your **global** `~/.cursor/mcp.json` file (Windows: `C:\Users\YourName\.cursor\mcp.json`)
- Ensure you've **fully restarted Cursor** after configuration (quit completely, not just close the window)
- Check that the token in the config file is valid and not expired
- Verify the JSON syntax is correct (use a JSON validator if needed)
- Make sure the file path is correct for your operating system

#### "Authentication failed"

**Solution:**
- Verify your token is valid: https://github.com/settings/tokens
- Check token has required scopes (`repo`, `read:org`, `read:user`)
- Regenerate token if expired
- Ensure the token in your global `mcp.json` file is correct (check for typos, extra spaces, or missing `Bearer ` prefix)

#### "No pull requests found"

**Solution:**
- Verify you have PRs in the date range
- Check repository permissions
- Try: "Show me my GitHub activity"
- Ensure you're authenticated to the correct GitHub account

#### Token Compromised?

**Immediate action:**
1. Go to https://github.com/settings/tokens
2. Find and **revoke** the compromised token
3. Generate a new token
4. Update the token in your global `~/.cursor/mcp.json` file
5. Restart Cursor

### Alternative: GitHub CLI Authentication

If you have GitHub CLI installed, you can use it for authentication. However, the recommended approach is to use the hosted GitHub MCP server with a Personal Access Token as described above.

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

> **Note:** The Atlassian Rovo MCP is automatically configured via `mcp.json` in this project. No setup required!
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
1. **Validate Jira connection** (automatic check before proceeding)
   - Tests Atlassian MCP connection
   - **If connection fails, stops immediately and does NOT proceed with any data retrieval or report generation**
   - Provides setup instructions and references this guide for configuration help
   - **Waits for you to fix the connection before proceeding**
2. **Only if connection succeeds:**
   - Fetch your Jira issues (created, updated, or resolved in date range)
   - Fetch your GitHub activity (if configured): PRs, commits, reviews
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

The GitHub MCP integration automatically captures your documentation work in code repositories when configured.

### What Gets Tracked

When GitHub MCP is configured, the assistant automatically fetches:

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

**Optional: Disable GitHub integration temporarily:**
```
Generate my Q2 report. I'm L2 IC.
Use only Jira data, skip GitHub.
```

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

