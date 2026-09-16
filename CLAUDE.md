# Performance Cycle Report Assistant — Claude Code

@AGENTS.md

## Claude Code notes

- Prefer invoking the Skills directly instead of reading everything up front: use `generate-work-summary` when the user asks for accomplishments/unfinished-work reporting, and `generate-performance-analysis` when they ask for a competency evaluation. Each skill pulls in the shared retrieval/writing docs it needs.
- Project MCP servers (Atlassian) are configured in `.mcp.json` and load automatically for this project in Claude Code. GitHub is also required, but it's connected as a plugin rather than through `.mcp.json` — see `docs/SETUP.md`.
- Cursor reads the exact same files: `AGENTS.md` as an always-applied rule, and `.claude/skills/` as Agent Skills (Cursor loads Claude's skills directory for compatibility). There is no Cursor-specific copy to maintain — the legacy `.cursorrules` file has been removed, and changes made here or in a `SKILL.md` apply to both tools immediately.
