# Setup Guide

## Overview

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

## Next Steps

Once MCP servers are configured, proceed to:
- **[Quick Start](../QUICK_START.md)** - Generate your first report
- **[Usage Guide](USAGE_GUIDE.md)** - Learn advanced features
- **[Examples](../examples/example-request.md)** - See sample requests
- **[Metrics Guide](../METRICS_GUIDE.md)** - Understand GitHub and Jira metrics

