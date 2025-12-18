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

### Work Summary Metrics

Your work summary will include:

```markdown
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
```

### Per-Quarter Breakdown

GitHub metrics are included in each quarter's summary:

```markdown
### Quarter 2
**Q2 Metrics:** 38 issues completed | 5 in progress | 88% completion rate
**GitHub:** 12 PRs merged | 8 reviews | 3 repositories
```

### Per-Work-Area Metrics

Work areas now include GitHub contributions:

```markdown
#### API Documentation
**Metrics:** 15 completed | 2 in progress | Avg resolution: 6 days
**GitHub:** 8 PRs merged in developer-docs repo

**Accomplishments:**
- Authored comprehensive API reference for Payment Gateway (PR #234)
- Reviewed 5 API documentation PRs from engineering team
- Updated 23 endpoint descriptions with improved examples
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
   - Jira metrics (completion rate, etc.)
   - GitHub metrics (PR merge time, etc.)
   - Combined view of your work

### Work Area Clustering

The assistant intelligently groups work:

- Jira issues by components/labels
- GitHub PRs by repository
- Merges related work (e.g., Jira issue + related PR)

Example:
```markdown
#### Developer Portal Documentation
**Metrics:** 8 Jira issues completed | 12 GitHub PRs merged

**Accomplishments:**
- Completed developer portal redesign (JIRA-123)
- Merged 12 PRs updating API documentation
- Reviewed 6 PRs from engineering team
```

## Setup Instructions

**Quick Setup:** See **[SETUP.md](SETUP.md)** for complete step-by-step instructions including:
- Generating GitHub Personal Access Token
- Setting environment variables (Windows/Mac/Linux)
- Configuring global Cursor MCP
- Security best practices
- Troubleshooting

**Summary:**
1. Generate GitHub token with `repo`, `read:org`, `read:user` scopes
2. Set `GITHUB_TOKEN` environment variable
3. Add GitHub MCP to global `~/.cursor/mcp.json`
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
- **Proves impact**: Quantifiable metrics (PRs, commits, reviews)

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
- Automatic metric calculations
- Integrated with Jira data

## Troubleshooting

### No GitHub Data Appearing

**Check:**
1. Is `GITHUB_TOKEN` environment variable set?
   ```powershell
   echo $env:GITHUB_TOKEN  # Windows
   echo $GITHUB_TOKEN      # Mac/Linux
   ```
2. Is GitHub MCP in your **global** `~/.cursor/mcp.json`?
3. Did you restart Cursor after setup?
4. Do you have PRs/commits in the date range?

### Authentication Errors

**Solutions:**
- Verify token is valid: https://github.com/settings/tokens
- Check token has required scopes: `repo`, `read:org`, `read:user`
- Regenerate token if expired
- Ensure environment variable has correct value

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

- ✅ Use environment variables for tokens
- ✅ Store tokens in global config (not project files)
- ✅ Use minimal required scopes
- ✅ Rotate tokens periodically
- ✅ Revoke tokens immediately if compromised
- ❌ Never commit tokens to Git
- ❌ Never share tokens in screenshots

## Optional: Disable GitHub Integration

If you want to generate a report **without** GitHub data:

1. **Temporary**: Just mention in your request:
   ```
   Generate my Q2 report. I'm L2 IC.
   Use only Jira data, skip GitHub.
   ```

2. **Permanent**: Remove GitHub MCP from your global `~/.cursor/mcp.json`

## Next Steps

- **[Setup Guide](SETUP.md)** - Detailed GitHub MCP configuration
- **[Metrics Guide](../METRICS_GUIDE.md)** - Understanding GitHub metrics
- **[Usage Guide](USAGE_GUIDE.md)** - Advanced usage patterns
- **[Examples](../examples/example-request.md)** - Sample requests with GitHub

---

**Questions?** See the main [README](../README.md) or [SETUP.md](SETUP.md) for more information.

