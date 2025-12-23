# Setup & Usage Guide

Complete guide for setting up, using, and troubleshooting the Performance Cycle Report Assistant.

## Table of Contents

1. [Setup](#setup)
   - [Atlassian MCP (Jira)](#atlassian-rovo-mcp-configuration)
   - [GitHub MCP (Optional)](#github-mcp-configuration-optional)
2. [How to Use](#how-to-use)
3. [Understanding Your Reports](#understanding-your-reports)
4. [GitHub Integration](#github-integration)
5. [Troubleshooting](#troubleshooting)
6. [Customization](#customization)
7. [Best Practices](#best-practices)

---

## Setup

This project integrates with two MCP servers:
- **Atlassian MCP** (required): Fetches Jira data
- **GitHub MCP** (optional): Fetches PR, commit, and review data

> **✅ Automatic Setup:** This project includes `mcp.json` which automatically configures the Atlassian Rovo MCP. For GitHub MCP, you need to configure it in your global Cursor MCP configuration file (see below).

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

## How to Use

> **Note:** The Atlassian Rovo MCP is automatically configured via `mcp.json` in this project. No setup required!
>
> **Callout:** Always state whether you are an individual contributor or a manager. IC (Technical Writer) levels end at **L3**; the manager track starts at **L3**. Use "technical writer", "tech writer", "ic", or "individual collaborator" for IC roles, and "technical writing manager" or "manager" for manager roles.
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
   - Group by calendar quarters (Q1-Q4)
   - Cluster into work areas (based on components, labels, themes, repositories)
   - Generate accomplishment bullets per area
   - Identify unfinished tasks
   - Analyze competency areas with strengths and development areas
   - Save two separate reports to `reports/`:
     - `work-summary-[date-range].md`
     - `performance-analysis-[date-range].md`

### Tips for Best Results

1. **Keep Jira updated** - Add meaningful descriptions, labels, and components
2. **Provide context** - Mention non-Jira/GitHub activities, special projects, challenges
3. **Be specific** - State your role and exact level (Technical Writer L1/L2/L3 or Technical Writing Manager L3/L4/L5/L6)
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
- **Competency Areas** - Strengths and development areas for each
- **Summary of Alignment** - Overall assessment against level expectations

**Use for:**
- Career development planning
- Identifying skill gaps
- Setting growth goals

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

## Customization

### For Your Organization

1. **Replace or extend the competency frameworks:**
   - Edit `context/technical-writer-career-path.json` (writers)
   - Edit `context/technical-writing-manager-career-path.json` (managers L3–L6)
   - Add your organization's levels and expectations

2. **Adjust metrics:**
   - Edit `.cursorrules` section 4.2
   - Customize which metrics to include/exclude

3. **Modify report structure:**
   - Edit `.cursorrules` sections 4.1 and 5.1
   - Adjust bullet counts, sections, or formatting

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
A: Reports are saved as Markdown files in `reports/`. Share them directly or convert to PDF.

**Q: Can I customize the competency framework?**  
A: Yes! Replace `context/technical-writer-career-path.json` with your organization's framework.

**Q: What if my level isn't L1, L2, or L3?**  
A: Update the JSON file with your organization's levels and expectations.

---

## Next Steps

- **[Metrics Guide](../METRICS_GUIDE.md)** - Understand all metrics (basic + advanced)
- **[Examples](../examples/example-report-with-metrics.md)** - See a complete example report
- **[README](../README.md)** - Project overview and quick start
- **[Changelog](../CHANGELOG.md)** - Release history and updates

