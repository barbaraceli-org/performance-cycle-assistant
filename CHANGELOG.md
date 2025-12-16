# Changelog

All notable changes to the Performance Cycle Report Assistant will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.0] - 2025-01-16

### Added
- Initial release of Performance Cycle Report Assistant
- Atlassian Rovo MCP integration for automatic Jira data fetching
- Two-report structure: Work Summary and Performance Analysis
- Support for L1, L2, L3 Technical Writer levels
- Automatic quarterly work grouping and clustering by work areas
- 6 competency area analysis:
  - Abstraction & Modeling
  - Responsibility & Scope
  - Autonomy & Execution
  - Communication
  - Editorial, Writing & Content Management
  - Technical Writing
- Comprehensive documentation:
  - Setup Guide with Atlassian Rovo MCP configuration
  - Usage Guide with tips and troubleshooting
  - Quick Start guide for rapid onboarding
  - Example requests for common scenarios
- Customizable competency framework via CSV file
- Automatic report saving to `reports/` folder
- `.gitignore` for protecting personal data

### Features
- Automatic Jira issue retrieval based on date range
- Intelligent work area clustering using components, labels, and themes
- Neutral, evidence-based tone in generated reports
- Accomplishments and unfinished tasks tracking
- Alignment analysis with LX Technical Writer expectations
- Strengths and development areas for each competency
- Summary of alignment (complete, partial gaps, major gaps)

### Documentation
- Comprehensive setup instructions (~2 minutes)
- Troubleshooting guides for common issues
- Security best practices
- Project structure documentation
- Customization guidelines

## [Unreleased]

### Planned
- Support for additional competency frameworks
- Multi-language report generation
- Report comparison across time periods
- Export to PDF/DOCX formats
- Team-level aggregated reports
- Custom JQL query templates

