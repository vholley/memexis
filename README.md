# Memexis

 A personal study and project system. The name is built from Memex (Vannevar Bush's personal knowledge device, 1945) and the -xis ending of praxis (action, practice). Memory and practice, indexed.

## What this is

Three top-level containers, plus an archive:

- `courses/` — long-running, evergreen topic studies. Continuously editable.
- `projects/` — bounded work with defined deliverables. Active until shipped, then archived.
- `resources/` — cross-cutting reference articles (patterns, techniques, lessons that generalize).
- `archive/` — completed projects and superseded material.

## Setup

This repo is a template. To start your own Memexis:

1. Fork it or use it as a GitHub template to create your own copy.
2. Configure Claude to read and write your copy: follow [`docs/setup-claude-mcp-github.md`](docs/setup-claude-mcp-github.md) to set up the GitHub MCP server in Claude Desktop or Claude Code.

More setup docs (Claude Code install, optional integrations) will live in `docs/` as they're added.

## How to use

Most work happens through an LLM agent. Two interaction modes:

- **Chat (primary)**: open a Claude conversation, discuss what you want to do, and let Claude operate on the repo via tool calls or by handing tasks to Claude Code.
- **Claude Code (secondary)**: open the repo in Claude Code for hands-on, multi-file editing during project work or larger restructures.

The agent reads `CLAUDE.md` for schema, conventions, and per-workflow instructions. That file is the source of truth for how the system behaves.

## Common operations

- **Start a new course**: ask Claude to design a curriculum on a topic. It will web-search, propose modules and sources, and scaffold the course directory.
- **Ingest a source**: drop a paper, article, or transcript into a course's `study/` folder and ask Claude to process it. The summary lands in `sources/`, and relevant module pages get updated.
- **Start a project**: ask Claude to propose a project that builds on a course (or describe one yourself). Joint scoping, then Claude scaffolds `projects/<name>/`.
- **Capture a learning**: tell Claude about a pattern or technique worth keeping. It writes a `resources/` entry and links it back to the project or course it came from.
- **Lint pass**: ask Claude to health-check the wiki periodically. It flags stale claims, missing cross-references, study material ready for promotion, and contradictions.

## Reading the wiki

Any markdown viewer works. Suggestions:

- **VS Code** with markdown preview for everyday reading and ripgrep-based search.
- **Obsidian** pointed at the repo as a read-only viewer if you want backlink and graph features (optional).
- **MkDocs Material** or similar if you ever want a rendered, browsable site.

## Repository layout

See `CLAUDE.md` for the full spec. At a glance:

```
courses/<course-id>/                # course-level README and modules
    README.md                       # course overview, syllabus
    log.md                          # course-level events log
    NN-<module-slug>/               # one directory per module
        README.md                   # canonical module page
        study/                      # raw study material (LLM working drafts)
        exercises.md                # optional exercises
projects/<project-id>/
    README.md                       # scope, goals, status
    plan.md                         # phased plan
    log.md                          # build log
    learnings.md                    # distilled findings
    references.md                   # links to courses, resources, sources
resources/
    README.md                       # top-level resources index
    <category>/                     # python, sql, ai-engineering, etc.
        README.md                   # category index
        <entry>.md                  # individual resource articles
sources/
    <year>/<source-slug>.md         # ingested source summaries
archive/
    projects/<project-id>/          # completed or abandoned projects
dashboards/                         # LLM-generated views (active projects, review queue, etc.)
templates/                          # page templates for each type
log.md                              # global event log
CLAUDE.md                           # agent instructions and schema
```
