# Usage Guide

> **First time?** See the [Setup Guide](SETUP.md) to configure Atlassian Rovo MCP.

## How to Use

1. **Open Cursor Chat:** `Ctrl+L` or `Cmd+L`

2. **Make your request:**
   ```
   Generate my performance cycle report for 2025-01-01 to 2025-06-30.
   I'm Level 3.
   ```

3. **Review and refine:**
   - Ask for more detail: "Expand the Q2 accomplishments"
   - Add context: "Include these activities: [list]"
   - Focus areas: "Add more detail to Communication competency"

## What Happens Automatically

The assistant will:
1. Fetch your Jira issues (created, updated, or resolved in date range)
2. Group by calendar quarters (Q1-Q4)
3. Cluster into work areas (based on components, labels, themes)
4. Generate accomplishment bullets per area
5. Identify unfinished tasks
6. Analyze 6 competency areas with strengths and development areas
7. Save two separate reports to `reports/`:
   - `work-summary-[date-range].md`
   - `performance-analysis-[date-range].md`

## Tips for Best Results

1. **Keep Jira updated** - Add meaningful descriptions, labels, and components
2. **Provide context** - Mention non-Jira activities, special projects, challenges
3. **Be specific** - Use exact level (L1, L2, or L3)
4. **Review and iterate** - Ask for refinements or additional detail

## Troubleshooting

**Jira issues not found:**
- Verify Atlassian Rovo MCP is configured ([Setup Guide](SETUP.md))
- Test: "Show me my recent Jira issues"
- Check date format: YYYY-MM-DD
- Ensure you're logged into Atlassian in your browser

**Report incomplete:**
```
Also include these activities not in Jira:
- [Your activities]
```

**Wrong level analysis:**
```
I'm Level 3. Compare against L3 expectations specifically.
```

## Advanced Usage

**Compare periods:**
```
Generate reports for Q1 and Q2 2025. Highlight differences.
```

**Team member report:**
```
Generate report for [name], Level 2, 2025-01-01 to 2025-06-30
```

