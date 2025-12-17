# Performance Cycle Report Assistant

Automatically generates comprehensive performance cycle reports for Technical Writers using Jira data and LX competency frameworks.

## 🎯 New in v2.0: Quantitative Metrics!

Work summary reports now include automatic metrics: completion rates, resolution times, issue type distributions, blocker analysis, and more!

---

## 🧭 Navigation

**Lost?** → See **[NAVIGATION.md](NAVIGATION.md)** for a complete guide to all documentation.

---

## 🚀 Quick Start

**First time?** → See **[QUICK_START.md](QUICK_START.md)** for a one-page guide.

Open Cursor Chat (`Ctrl+L` or `Cmd+L`) and say:

```
Generate my performance cycle report for 2025-01-01 to 2025-06-30.
I'm a Level 2 Technical Writer.
```

That's it! The assistant will:
- ✅ Fetch your Jira issues automatically
- ✅ Calculate quantitative metrics
- ✅ Group work by quarter and area
- ✅ Generate accomplishments and identify unfinished tasks
- ✅ Analyze alignment with L2 expectations
- ✅ Create competency-based assessment
- ✅ Save two comprehensive reports

## Prerequisites

- **Cursor IDE** (latest version)
- **Jira Cloud** access to your workspace
- **Atlassian account** with appropriate permissions

> **Note:** The Atlassian Rovo MCP is automatically configured via `mcp.json` in this project. No manual setup required!

## Available Levels

**Technical Writers**
- **L1** - Technical Writer I
- **L2** - Technical Writer II  
- **L3** - Senior Technical Writer

**Technical Writing Managers**
- **L3** - Technical Writing Manager I (manager track starts at L3)
- **L4** - Technical Writing Manager II
- **L5** - Senior Manager, Technical Writing
- **L6** - Head of Education & Documentation

## What You Get

Two separate reports automatically saved to the `reports/` folder:

### 1. Work Summary (`work-summary-[date-range].md`)
- **Quantitative metrics** (NEW!):
  - Overall: completion rate, avg resolution time, issue types, priorities
  - Per-quarter: issues completed, in progress, completion rate
  - Per-work-area: completed count, avg resolution time
  - Unfinished work: blocker analysis, average age
- Accomplishments by quarter and work area
- Unfinished tasks with current status

### 2. Performance Analysis (`performance-analysis-[date-range].md`)
- 6 competency areas with strengths and development areas:
  - Abstraction & Modeling
  - Responsibility & Scope
  - Autonomy & Execution
  - Communication
  - Editorial, Writing & Content Management
  - Technical Writing
- Summary of alignment (complete alignment, partial gaps, major gaps)

## Example Requests

**Basic:**
```
Generate my performance report for 2025-01-01 to 2025-06-30.
I'm Level 2.
```

**With additional context:**
```
Generate my Q2 2025 report (Apr-Jun).
I'm Level 3.

Also include:
- Led documentation strategy workshop
- Mentored 2 junior writers
```

**Manager example:**
```
Generate my 2025-01-01 to 2025-06-30 report.
I'm a Level 3 Technical Writing Manager.
Focus on team leadership outcomes.
```

## Troubleshooting

**Jira issues not found?**
- Ensure you're logged into Atlassian in your browser
- Test with: "Show me my recent Jira issues"
- Check date format: YYYY-MM-DD
- Restart Cursor if MCP connection fails

**Report seems incomplete?**
- Add non-Jira activities in your request
- Mention specific projects or initiatives

## 📚 Documentation

**Getting Started:**
- **[Quick Start](QUICK_START.md)** - One-page reference (⭐ start here)
- **[Setup Guide](docs/SETUP.md)** - MCP configuration (usually automatic)

**Using the Assistant:**
- **[Usage Guide](docs/USAGE_GUIDE.md)** - Detailed instructions, tips, and best practices
- **[Examples](examples/example-request.md)** - Sample requests and use cases
- **[Example Report](examples/example-report-with-metrics.md)** - Complete report with metrics

**Understanding Results:**
- **[Metrics Guide](METRICS_GUIDE.md)** - Quantitative metrics explained
- **[CHANGELOG](CHANGELOG.md)** - Release history and updates

## Customization

**For Your Organization:**

1. **Competency Frameworks:** Replace `context/Technical writer career path.csv` (writers) and/or `context/Tech Writer Career Path - Technical Writing Manager.csv` (managers) with your organization's frameworks
2. **Report Structure:** Edit `.cursorrules` to customize report format, sections, or bullet point counts
3. **Metrics:** Modify `.cursorrules` section 4.2 to adjust which metrics are calculated and displayed
4. **Jira Queries:** Modify the JQL in `.cursorrules` (section 1) to adjust filters or fields
5. **Levels:** Update the CSVs and `.cursorrules` to add or modify levels for writers or managers

## 📁 Project Structure

```
performance-cycle/
├── .cursorrules                              # Assistant configuration
├── .gitignore                                # Git ignore rules
├── mcp.json                                  # Atlassian MCP auto-configuration
│
├── 📖 README.md                              # This file - complete overview
├── 🧭 NAVIGATION.md                          # Documentation guide
├── ⭐ QUICK_START.md                         # One-page quick reference
├── 📊 METRICS_GUIDE.md                       # Quantitative metrics explained
│
├── context/
│   ├── Technical writer career path.csv                      # LX expectations (Writers L1-L3)
│   └── Tech Writer Career Path - Technical Writing Manager.csv  # Manager expectations (Managers start at L3)
├── CHANGELOG.md                               # Release history
│
├── docs/
│   ├── SETUP.md                              # MCP configuration
│   └── USAGE_GUIDE.md                        # Detailed usage guide
│
├── examples/
│   ├── example-request.md                    # Sample requests
│   └── example-report-with-metrics.md        # Complete example report
│
└── reports/                                  # Generated reports (auto-created)
```

