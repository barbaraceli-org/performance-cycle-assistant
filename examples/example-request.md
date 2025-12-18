# 📝 Example Requests

This guide shows different ways to request performance cycle reports.

---

## 🎯 Basic Request

The simplest way to generate a report:

```
Generate my performance cycle report for 2025-01-01 to 2025-06-30.
I'm a Level 3 Technical Writer.
```

**What happens:**
- Fetches all your Jira issues from the date range
- Calculates quantitative metrics automatically
- Groups work by quarter and area
- Analyzes against L3 Technical Writer expectations
- Saves two reports:
  - `reports/work-summary-2025-H1.md`
  - `reports/performance-analysis-2025-H1.md`

---

## 📅 Common Date Ranges

**Half-year (H1):**
```
Generate my H1 2025 report (2025-01-01 to 2025-06-30).
I'm a Level 2 Technical Writer.
```

**Half-year (H2):**
```
Generate my H2 2025 report (2025-07-01 to 2025-12-31).
I'm a Level 2 Technical Writer.
```

**Full year:**
```
Generate my 2025 annual report (2025-01-01 to 2025-12-31).
I'm a Level 3 Technical Writer.
```

**Single quarter:**
```
Generate my Q2 2025 report (April-June).
I'm a Level 1 Technical Writer.
```

---

## 🎨 With Additional Context

Include non-Jira/GitHub activities and achievements:

```
Generate my Q1 2025 report (Jan-Mar).
I'm a Level 2 Technical Writer.

Also include:
- Presented "Documentation Best Practices" at team all-hands
- Completed technical writing certification
- Mentored new hire during onboarding
- Led documentation strategy workshop
```

**Why add context:**
- Captures work not tracked in Jira or GitHub
- Highlights presentations and training
- Shows mentoring and leadership
- Demonstrates professional development

**Note:** If GitHub MCP is configured, your PRs, commits, and reviews are automatically included!

---

## 🎯 Focused Analysis

Emphasize specific areas or competencies:

```
Generate my H1 2025 report (Jan-Jun).
I'm a Level 3 Technical Writer.

Focus on:
- API documentation contributions
- Cross-team collaboration
- Mentoring and technical leadership activities
- Documentation strategy and planning
```

**Use when:**
- Preparing for promotion discussions
- Highlighting specific competencies
- Demonstrating leadership impact
- Showing strategic contributions

---

## 🔄 Iterative Refinement

Start basic, then refine:

**Step 1: Generate initial report**
```
Generate my 2025 report. I'm a Level 2 Technical Writer.
```

**Step 2: Add detail to specific sections**
```
Expand the Communication competency section with more specific examples.
```

**Step 3: Add missing context**
```
Also include these activities:
- Organized documentation review sessions
- Created style guide for the team
```

---

## 📊 With Custom Work Areas

Organize your work into specific categories:

```
Generate my H1 2025 report. I'm a Level 3 Technical Writer.

Group my work into these areas:
- API Documentation
- Developer Guides
- Release Notes
- Internal Documentation
- Documentation Process Improvements
```

---

## 🆚 Comparing Periods

Generate multiple reports for comparison:

```
Generate reports for Q1 and Q2 2025. 
I'm a Level 2 Technical Writer.
Highlight differences in volume and focus areas.
```

---

## ✅ Do's and Don'ts

### ✅ DO Include:

- **Date range** (YYYY-MM-DD or quarter/year)
- **Your role and level** (e.g., "Level 2 Technical Writer" or "Level 3 Technical Writing Manager")
- **Activities not in Jira/GitHub** (presentations, training, mentoring, team outcomes, process improvements)
- **Special projects** not tracked in systems
- **Context** about challenges or achievements

> **Note:** Jira issues and GitHub PRs/commits/reviews are automatically fetched. Focus on adding activities not tracked in systems: mentoring, presentations, workshops, process improvements, team outcomes (for managers), and links to key artifacts.

### ❌ DON'T Need To:

- List Jira issues manually (automatic)
- List GitHub PRs manually (automatic if configured)
- Export data (automatic)
- Mention the expectations file (automatic)
- Calculate metrics (automatic)
- Format the reports (automatic)

---

## 💡 Pro Tips

**Tip 1: Be specific about your level and role**
```
I'm a Level 2 Technical Writer (IC). Compare against the IC framework.
I'm a Level 3 Technical Writing Manager. Use the manager framework.
```

**Tip 2: Mention blockers and challenges**
```
Note: Q2 had significant delays due to product roadmap changes.
```

**Tip 3: Highlight cross-functional work**
```
Include collaboration with Engineering, Product, and Design teams.
```

**Tip 4: Add learning and growth**
```
Also include:
- Completed advanced technical writing course
- Attended API documentation workshop
```

---

## 🐙 GitHub Integration Examples

If you have GitHub MCP configured, you can explicitly mention GitHub work:

```
Generate my H1 2025 report.
I'm a Level 3 Technical Writer.

Focus on:
- API documentation PRs in the developer-docs repo
- README improvements across multiple repositories
- Documentation reviews I provided to the engineering team
```

**Or let it auto-detect:**

```
Generate my Q2 2025 report. I'm L2 IC.
```

The assistant will automatically include your GitHub activity if configured!

---

## 🔗 Next Steps

- **See a complete example:** [example-report-with-metrics.md](example-report-with-metrics.md)
- **Understand metrics:** [../METRICS_GUIDE.md](../METRICS_GUIDE.md) (includes GitHub metrics)
- **Setup GitHub:** [../docs/SETUP.md](../docs/SETUP.md) (optional but recommended)
- **Learn more:** [../docs/USAGE_GUIDE.md](../docs/USAGE_GUIDE.md)
- **Quick reference:** [../QUICK_START.md](../QUICK_START.md)

