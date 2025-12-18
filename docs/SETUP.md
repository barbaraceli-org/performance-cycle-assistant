# Setup Guide

## Overview

This project integrates with two MCP servers:
- **Atlassian MCP** (required): Fetches Jira data
- **GitHub MCP** (optional): Fetches PR, commit, and review data

> **✅ Automatic Setup:** This project includes `mcp.json` which automatically configures both the Atlassian Rovo MCP and GitHub MCP. For GitHub, you just need to set the `GITHUB_TOKEN` environment variable (see below).

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

> **✅ Already Configured:** The project `mcp.json` already includes GitHub MCP configuration. You just need to:
> 1. Generate a GitHub Personal Access Token (Step 1)
> 2. Set the `GITHUB_TOKEN` environment variable (Step 2)
> 3. Verify it's set (Step 4)
> 4. Restart Cursor (Step 5)
>
> **That's it!** No need to edit any configuration files.

### Prerequisites

- GitHub account with access to relevant repositories
- GitHub Personal Access Token (PAT)

### Security: Token Management

**⚠️ NEVER commit GitHub tokens to Git!**

The GitHub MCP server requires authentication. The project uses environment variables to keep your token secure. The `mcp.json` file references `${GITHUB_TOKEN}`, which Cursor will automatically read from your environment.

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

### Step 2: Set Environment Variable (Secure Method)

**On Windows (PowerShell - Permanent):**

```powershell
# Set user environment variable (persists across sessions)
[System.Environment]::SetEnvironmentVariable('GITHUB_TOKEN', 'your_token_here', 'User')
```

**On Windows (System Settings GUI):**

1. Search for "Environment Variables" in Windows
2. Click "Edit the system environment variables"
3. Click "Environment Variables..." button
4. Under "User variables", click "New..."
5. Variable name: `GITHUB_TOKEN`
6. Variable value: `your_token_here` (paste your token)
7. Click OK
8. **Restart Cursor**

**On Mac/Linux (Permanent):**

Add to your shell profile (`~/.bashrc`, `~/.zshrc`, etc.):

```bash
export GITHUB_TOKEN="your_token_here"
```

Then reload:
```bash
source ~/.bashrc  # or ~/.zshrc
```

### Step 3: Verify Project Configuration

The project `mcp.json` already includes GitHub MCP configuration:

```json
{
  "mcpServers": {
    "github": {
      "url": "https://api.githubcopilot.com/mcp/",
      "headers": {
        "Authorization": "Bearer ${GITHUB_TOKEN}"
      }
    }
  }
}
```

**No additional configuration needed!** Cursor will automatically use this project-level configuration when you open the project.

> **Note:** If you prefer to use a global configuration instead, you can add the same configuration to `~/.cursor/mcp.json` (Windows: `C:\Users\YourName\.cursor\mcp.json`). However, using the project-level `mcp.json` is recommended as it's already set up and works automatically.

**For GitHub Enterprise:**

If using GitHub Enterprise Cloud, edit the project `mcp.json` and change the URL:

```json
{
  "mcpServers": {
    "github": {
      "url": "https://copilot-api.your-enterprise.ghe.com/mcp",
      "headers": {
        "Authorization": "Bearer ${GITHUB_TOKEN}"
      }
    }
  }
}
```

### Step 4: Verify Environment Variable

Before restarting, verify that your `GITHUB_TOKEN` is set:

**On Windows (PowerShell):**
```powershell
echo $env:GITHUB_TOKEN
```

**On Mac/Linux:**
```bash
echo $GITHUB_TOKEN
```

If you see your token (starts with `ghp_`), you're all set! If not, the environment variable wasn't set correctly. Go back to Step 2 and try again.

### Step 5: Restart Cursor

After setting the environment variable:

1. **Close Cursor completely** (not just the window - fully quit the application)
2. **Restart Cursor**
3. **Open this project** in Cursor
4. The GitHub MCP will now be available and automatically configured via the project `mcp.json`

### Verify GitHub Configuration

Test the connection in Cursor Chat:

```
Show me my recent GitHub pull requests
```

If configured correctly, you should see your recent PRs.

### Security Best Practices

**DO:**
- ✅ Use environment variables for tokens
- ✅ Store tokens in your global `~/.cursor/mcp.json` (not in project files)
- ✅ Add `.env`, `*.token`, `secrets/` to `.gitignore`
- ✅ Rotate tokens periodically
- ✅ Use fine-grained tokens with minimal scopes when possible
- ✅ Revoke tokens immediately if compromised

**DON'T:**
- ❌ Hardcode tokens in `mcp.json` files
- ❌ Commit tokens to Git
- ❌ Share tokens in screenshots or documentation
- ❌ Use tokens with more permissions than needed
- ❌ Store tokens in plain text files in your project

### Troubleshooting GitHub MCP

#### "GitHub MCP not found"

**Solution:**
- Verify `GITHUB_TOKEN` environment variable is set: `echo $env:GITHUB_TOKEN` (Windows) or `echo $GITHUB_TOKEN` (Mac/Linux)
- Ensure you've **fully restarted Cursor** after setting the variable (quit completely, not just close the window)
- Verify the project `mcp.json` includes the GitHub MCP configuration (it should already be there)
- Make sure you opened the project folder in Cursor (the project-level `mcp.json` is only active when the project is open)

#### "Authentication failed"

**Solution:**
- Verify your token is valid: https://github.com/settings/tokens
- Check token has required scopes (`repo`, `read:org`, `read:user`)
- Regenerate token if expired
- Ensure environment variable has correct token value

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
4. Update the `GITHUB_TOKEN` environment variable
5. Restart Cursor

### Alternative: GitHub CLI Authentication

If you have GitHub CLI installed, you can use it for authentication:

```powershell
# Install GitHub CLI (Windows)
winget install --id GitHub.cli

# Authenticate
gh auth login

# Verify
gh auth status
```

Then use this MCP configuration:

```json
{
  "mcpServers": {
    "github": {
      "command": "gh",
      "args": ["api", "--paginate"],
      "env": {
        "GH_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

---

## Next Steps

Once MCP servers are configured, proceed to:
- **[Quick Start](../QUICK_START.md)** - Generate your first report
- **[Usage Guide](USAGE_GUIDE.md)** - Learn advanced features
- **[Examples](../examples/example-request.md)** - See sample requests
- **[Metrics Guide](../METRICS_GUIDE.md)** - Understand GitHub and Jira metrics

