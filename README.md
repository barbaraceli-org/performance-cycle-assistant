# Performance Cycle Report Assistant

Automatically generates comprehensive performance cycle reports for Technical Writers using Jira data and LX competency frameworks.

## 🚀 Quick Start

Open Cursor Chat (`Ctrl+L` or `Cmd+L`) and say:

```
Generate my performance cycle report for 2025-01-01 to 2025-06-30.
I'm a Level 2 Technical Writer.
```

That's it! The assistant will:
- Fetch your Jira issues automatically
- Group work by quarter and area
- Generate accomplishments and identify unfinished tasks
- Analyze alignment with L2 expectations
- Create competency-based assessment

## Prerequisites

- **Cursor IDE** with Atlassian Rovo MCP configured ([Setup Guide](docs/SETUP.md))
- **Jira Cloud** access to your workspace
- **Atlassian account** with appropriate permissions

> **First time?** Follow the [Setup Guide](docs/SETUP.md) to configure Atlassian Rovo MCP (takes ~2 minutes).

## Available Levels

- **L1** - Technical Writer I
- **L2** - Technical Writer II  
- **L3** - Senior Technical Writer

## What You Get

Two separate reports automatically saved to the `reports/` folder:

### 1. Work Summary (`work-summary-[date-range].md`)
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

## Troubleshooting

**Jira issues not found?**
- Verify Atlassian Rovo MCP is configured ([Setup Guide](docs/SETUP.md))
- Test with: "Show me my recent Jira issues"
- Check date format: YYYY-MM-DD
- Ensure you're logged into Atlassian in your browser

**Report seems incomplete?**
- Add non-Jira activities in your request
- Mention specific projects or initiatives

## Documentation

- **[Setup Guide](docs/SETUP.md)** - Configure Atlassian Rovo MCP (~2 min setup)
- **[Quick Start](QUICK_START.md)** - One-page reference guide
- **[Usage Guide](docs/USAGE_GUIDE.md)** - Detailed instructions and tips
- **[Examples](examples/example-request.md)** - More sample requests
- **[Changelog](CHANGELOG.md)** - Version history and updates

## Customization

**For Your Organization:**

1. **Competency Framework:** Replace `context/Technical writer career path.csv` with your organization's framework
2. **Report Structure:** Edit `.cursorrules` to customize report format, sections, or bullet point counts
3. **Jira Queries:** Modify the JQL in `.cursorrules` (lines 30-32) to adjust filters or fields
4. **Levels:** Update the CSV and `.cursorrules` to add or modify writer levels

## Project Structure

```
performance-cycle/
├── .cursorrules                              # Assistant configuration
├── .gitignore                                # Git ignore rules
├── CHANGELOG.md                              # Version history
├── README.md                                 # This file
├── QUICK_START.md                            # One-page reference
├── context/
│   └── Technical writer career path.csv      # LX expectations (L1-L3)
├── docs/
│   ├── SETUP.md                              # Atlassian Rovo MCP setup
│   └── USAGE_GUIDE.md                        # Detailed usage guide
├── examples/
│   └── example-request.md                    # Sample requests
└── reports/                                  # Generated reports (auto-created)
```

