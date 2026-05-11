---
type: project-log
status: polished
created: 2026-05-07
updated: 2026-05-11
---

# Global log

Append-only record of major events across the wiki. Format: `## [YYYY-MM-DD HH:MM] <verb> | <subject>`. Verbs: `create`, `archive`, `restructure`, `lint`, `note`, `decide`.

Grep recipe: `grep "^## \[" log.md | tail -20`

---

## [2026-05-07 00:00] create | Memexis initialized
Initial scaffold created. Three working containers (`courses/`, `projects/`, `resources/`), one archive (`archive/`), supporting directories (`sources/`, `dashboards/`, `templates/`). Schema and workflows defined in `CLAUDE.md`.

## [2026-05-11 12:00] create | resources/mcp/github-mcp-with-docker-desktop
First resource entry. Scaffolded `resources/` and `resources/mcp/` indexes and added the GitHub MCP with Docker Desktop setup note covering Claude Desktop and Claude Code paths, plus variations (read-only, dynamic toolsets, remote HTTP for Claude Code, Docker MCP Toolkit, GHES) and common pitfalls. Sets `mcp/` as the category precedent for future MCP server setup notes. Submitted as PR for review.
