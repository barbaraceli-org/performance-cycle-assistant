# Usage Guide

> **Note:** The Atlassian Rovo MCP is automatically configured via `mcp.json` in this project. No setup required!
> **Callout:** Always state whether you are an individual contributor or a manager. IC (Technical Writer) levels end at **L3**; the manager track starts at **L3**. Use “technical writer”, “tech writer”, “ic”, or “individual collaborator” for IC roles, and “technical writing manager” or “manager” for manager roles.

## How to Use

1. **Open Cursor Chat:** `Ctrl+L` or `Cmd+L`

2. **Make your request:**
   ```
Generate my performance cycle report for 2025-01-01 to 2025-06-30.
I'm a Level 3 Technical Writer.
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
3. **Be specific** - State your role and exact level (Technical Writer L1/L2/L3 or Technical Writing Manager L3/L4/L5/L6)
4. **Review and iterate** - Ask for refinements or additional detail

## Troubleshooting

**Jira issues not found:**
- Ensure you're logged into Atlassian in your browser
- Test: "Show me my recent Jira issues"
- Check date format: YYYY-MM-DD
- Restart Cursor if MCP connection fails
- See [Setup Guide](SETUP.md) for manual configuration if needed

**Report incomplete:**
```
Also include these activities not in Jira:
- [Your activities]
```

**Wrong role/level analysis:**
```
I'm a Level 3 Technical Writing Manager. Compare against manager expectations.
```

## Advanced Usage

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
- **6 Competency Areas** - Strengths and development areas for each
- **Summary of Alignment** - Overall assessment against level expectations

**Use for:**
- Career development planning
- Identifying skill gaps
- Setting growth goals

## Metrics Explained

See **[METRICS_GUIDE.md](../METRICS_GUIDE.md)** for complete details on:
- What each metric means
- How metrics are calculated
- Interpreting your results
- Good ranges and benchmarks

## Customization

### For Your Organization

1. **Replace or extend the competency frameworks:**
   - Edit `context/Technical writer career path.csv` (writers)
   - Edit `context/Tech Writer Career Path - Technical Writing Manager.csv` (managers L3–L6)
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

## Best Practices

### Keep Jira Updated

- ✅ Use meaningful issue titles
- ✅ Add descriptions with context
- ✅ Apply relevant labels and components
- ✅ Update status promptly
- ✅ Link related issues

### Provide Context

Include non-Jira activities:
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
A: Yes! Replace `context/Technical writer career path.csv` with your organization's framework.

**Q: What if my level isn't L1, L2, or L3?**  
A: Update the CSV file with your organization's levels and expectations.

## Next Steps

- **[Metrics Guide](../METRICS_GUIDE.md)** - Understand quantitative metrics
- **[Examples](../examples/example-report-with-metrics.md)** - See a complete example report
- **[Quick Start](../QUICK_START.md)** - Quick reference guide
- **[README](../README.md)** - Full project documentation
- **[CHANGELOG](../CHANGELOG.md)** - Release history and updates
