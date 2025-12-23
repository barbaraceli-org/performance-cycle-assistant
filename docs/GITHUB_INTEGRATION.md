# GitHub Integration Guide

This guide explains how the GitHub MCP integration enhances your performance cycle reports.

## Overview

The GitHub MCP integration automatically captures your documentation work in code repositories:
- Pull requests you authored
- Pull requests you reviewed
- Documentation commits
- Repository contributions
- Code review activity

## What Gets Tracked

### Automatically Included

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

## Metrics Added to Reports

The system tracks the following GitHub activity data, which is used to inform accomplishments but **not displayed as separate metrics**:

### GitHub Activity
- **Pull requests authored:** 24 (18 merged, 6 open)
- **Pull requests reviewed:** 15 across 4 repositories
- **Documentation commits:** 47 commits, 89 files modified
- **Repositories contributed to:** 6 repositories
- **Average PR merge time:** 2.3 days
- **Lines changed:** +2,847 / -1,203 in documentation files

**Documentation file types:**
- Markdown (*.md): 67 files
- README files: 12 files
- API documentation: 10 files

**Note:** This data is used to create accomplishments and is displayed as metrics in quarterly summaries, but **NOT** in the Overview Metrics section at the top of the report. Only Jira metrics are shown in the Overview Metrics section.

## How GitHub Work Appears in Reports

### Work Summary - Accomplishments

GitHub work is included in your accomplishments, and GitHub metrics are displayed in quarterly summaries. Only Jira metrics are shown in the Overview Metrics section at the top of the report.

Your accomplishments will include GitHub work like this:

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

### Note on Metrics

**GitHub metrics are displayed in quarterly summaries**, but NOT in the Overview Metrics section at the top. The system:
- ✅ Analyzes GitHub work (PRs, reviews, commits)
- ✅ Includes GitHub work in accomplishments
- ✅ Displays GitHub metrics in quarterly summaries (e.g., "GitHub: 12 PRs merged | 8 reviews | 3 repositories")
- ✅ Uses GitHub data to inform work areas
- ❌ Does NOT display GitHub metrics in the Overview Metrics section

Only **Jira metrics** are shown in the Overview Metrics section at the top of the report.

### Per-Quarter Breakdown

GitHub metrics are included in each quarter's summary:

```markdown
### Quarter 2
**Q2 Metrics:** 38 issues completed | 5 in progress | 88% completion rate
**GitHub:** 12 PRs merged | 8 reviews | 3 repositories

#### API Documentation
**Metrics:** 15 completed | 2 in progress | Avg resolution: 6 days

**Accomplishments:**
- Authored comprehensive API reference for Payment Gateway (PR #234)
- Reviewed 5 API documentation PRs from engineering team
- Updated 23 endpoint descriptions with improved examples
- Merged 8 PRs in developer-docs repo updating authentication docs
```

### Per-Work-Area Contributions

Work areas include GitHub contributions in accomplishments, but no separate GitHub metrics:

```markdown
#### API Documentation
**Metrics:** 15 completed | 2 in progress | Avg resolution: 6 days

**Accomplishments:**
- Authored comprehensive API reference for Payment Gateway (PR #234)
- Reviewed 5 API documentation PRs from engineering team
- Updated 23 endpoint descriptions with improved examples
- Merged 8 PRs in developer-docs repo updating authentication docs
```

## How It Works

### Data Retrieval

When you request a report, the assistant:

1. **Fetches Jira data** (as before)
2. **Fetches GitHub data** (if configured):
   - Searches for your PRs in the date range
   - Searches for PRs you reviewed
   - Searches for your commits
   - Filters for documentation-related files
3. **Merges the data**:
   - Groups by quarter
   - Clusters into work areas
   - Correlates GitHub PRs with Jira issues when possible
4. **Calculates metrics**:
   - Jira metrics (completion rate, resolution time, etc.)
   - GitHub work is analyzed and included in accomplishments
   - Note: GitHub metrics are NOT displayed, only Jira metrics are shown

### Work Area Clustering

The assistant intelligently groups work:

- Jira issues by components/labels
- GitHub PRs by repository
- Merges related work (e.g., Jira issue + related PR)

Example:
```markdown
#### Developer Portal Documentation
**Metrics:** 8 completed | 2 in progress | Avg resolution: 5 days

**Accomplishments:**
- Completed developer portal redesign (JIRA-123)
- Merged 12 PRs updating API documentation
- Reviewed 6 PRs from engineering team
- Updated sidebar navigation across documentation portal
```

## Setup Instructions

**Quick Setup:** See **[SETUP.md](SETUP.md)** for complete step-by-step instructions including:
- Generating GitHub Personal Access Token
- Configuring GitHub MCP in global Cursor settings
- Security best practices
- Troubleshooting

**Summary:**
1. Generate GitHub token with `repo`, `read:org`, `read:user` scopes
2. Add GitHub MCP configuration to your global `~/.cursor/mcp.json` file (Windows: `C:\Users\YourName\.cursor\mcp.json`)
3. Replace `YOUR_GITHUB_PAT` with your actual token
4. Restart Cursor

## Usage Examples

### Basic Request (Auto-Includes GitHub)

```
Generate my Q2 2025 report.
I'm a Level 3 Technical Writer.
```

The assistant automatically includes GitHub data if configured.

### Emphasize GitHub Work

```
Generate my H1 2025 report.
I'm L2 IC.

Focus on:
- API documentation PRs in the developer-docs repo
- README improvements across repositories
- Documentation reviews for the engineering team
```

### GitHub + Additional Context

```
Generate my 2025 report. I'm L3 Technical Writer.

GitHub work is tracked automatically.

Also include:
- Led documentation strategy workshop
- Mentored 2 new technical writers
- Presented at Engineering All-Hands
```

## Benefits

### More Complete Picture

- **Captures in-repo work**: Documentation that lives in code repositories
- **Shows collaboration**: Code reviews demonstrate technical expertise
- **Demonstrates breadth**: Contributions across multiple repositories
- **Proves impact**: Work is included in accomplishments with PR references

### Better Competency Evidence

GitHub data strengthens evidence for:

- **Technical Writing**: In-repo documentation, API docs, README files
- **Communication**: Code review comments, PR descriptions
- **Collaboration**: Reviews provided, cross-team contributions
- **Autonomy**: Self-directed repository improvements
- **Scope**: Breadth across multiple projects/repositories

### Automatic Tracking

- No manual data entry
- No need to remember PR numbers
- GitHub work automatically included in accomplishments
- Integrated with Jira data for complete picture

## Troubleshooting

### No GitHub Data Appearing

**Check:**
1. Is GitHub MCP configured in your **global** `~/.cursor/mcp.json` file? (Windows: `C:\Users\YourName\.cursor\mcp.json`)
2. Is your Personal Access Token correctly set in the config file (replaced `YOUR_GITHUB_PAT`)?
3. Did you fully restart Cursor after configuration (quit completely, not just close window)?
4. Do you have PRs/commits in the date range?
5. Verify the token is valid: https://github.com/settings/tokens

### Authentication Errors

**Solutions:**
- Verify token is valid: https://github.com/settings/tokens
- Check token has required scopes: `repo`, `read:org`, `read:user`
- Regenerate token if expired
- Ensure the token in your global `mcp.json` file is correct (check for typos, extra spaces, or missing `Bearer ` prefix)

### Only Seeing Code PRs, Not Docs

**This is expected!** The integration filters for documentation files only:
- `*.md`
- `docs/**`
- `README*`
- `CONTRIBUTING*`

If your documentation work is in other file types, mention it explicitly in your request.

### GitHub Data Not Merged with Jira

**This is normal.** GitHub and Jira data are shown separately unless:
- A PR description references a Jira issue (e.g., "Fixes JIRA-123")
- The work area is clearly the same (e.g., same component/repository)

The assistant groups related work when possible, but they may appear in separate sections.

## Privacy & Security

### What Gets Accessed

The GitHub MCP can access:
- Public repositories (always)
- Private repositories (if token has `repo` scope)
- Your PRs, commits, and reviews
- Repository names and file paths

### What Doesn't Get Accessed

- Code content (only documentation files)
- Private repositories from other organizations (unless you have access)
- Other users' private activity
- Repository secrets or sensitive data

### Security Best Practices

- ✅ Store tokens in global `~/.cursor/mcp.json` (not in project files)
- ✅ Use minimal required scopes
- ✅ Rotate tokens periodically
- ✅ Revoke tokens immediately if compromised
- ✅ Keep your global MCP config file private
- ❌ Never commit tokens to Git (global config is outside project directory)
- ❌ Never share tokens in screenshots
- ❌ Never share your global MCP configuration file

## Optional: Disable GitHub Integration

If you want to generate a report **without** GitHub data:

1. **Temporary**: Just mention in your request:
   ```
   Generate my Q2 report. I'm L2 IC.
   Use only Jira data, skip GitHub.
   ```

2. **Permanent**: Remove GitHub MCP configuration from your global `~/.cursor/mcp.json` file

## Next Steps

- **[Setup Guide](SETUP.md)** - Detailed GitHub MCP configuration
- **[Metrics Guide](../METRICS_GUIDE.md)** - Understanding GitHub metrics
- **[Usage Guide](USAGE_GUIDE.md)** - Advanced usage patterns
- **[Examples](../examples/example-request.md)** - Sample requests with GitHub

---

**Questions?** See the main [README](../README.md) or [SETUP.md](SETUP.md) for more information.

