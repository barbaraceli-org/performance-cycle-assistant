# Setup Guide

## Atlassian Rovo MCP Configuration

This project requires the Atlassian Rovo MCP (Model Context Protocol) server to automatically fetch Jira and Confluence data.

### Prerequisites

- Cursor IDE (latest version)
- Jira Cloud account with access to your workspace
- Atlassian account with appropriate permissions

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

### Step 4: Verify Configuration

Test the connection in Cursor Chat:

```
Show me my recent Jira issues
```

If configured correctly, you should see a list of your Jira issues.

### Alternative: Manual Configuration File

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

## Next Steps

Once Atlassian MCP is configured, proceed to:
- **[Quick Start](../QUICK_START.md)** - Generate your first report
- **[Usage Guide](USAGE_GUIDE.md)** - Learn advanced features
- **[Examples](../examples/example-request.md)** - See sample requests

