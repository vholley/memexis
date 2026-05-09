---
type: project-log
status: polished
created: 2026-05-07
updated: 2026-05-07
---

# Global log

Append-only record of major events across the wiki. Format: `## [YYYY-MM-DD HH:MM] <verb> | <subject>`. Verbs: `create`, `archive`, `restructure`, `lint`, `note`, `decide`.

Grep recipe: `grep "^## \[" log.md | tail -20`

---

## [2026-05-07 00:00] create | Memexis initialized
Initial scaffold created. Three working containers (`courses/`, `projects/`, `resources/`), one archive (`archive/`), supporting directories (`sources/`, `dashboards/`, `templates/`). Schema and workflows defined in `CLAUDE.md`.
