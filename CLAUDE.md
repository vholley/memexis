# CLAUDE.md

Authoritative instructions for the LLM agent operating on Memexis, this knowledge wiki repository. Read fully before any work.

## 1. Purpose

This repository is a personal knowledge wiki for structured topic study and project execution. The user is the curator and reviewer. The agent is the writer and maintainer. The user names topics, accepts plans, and reviews output. The agent does the writing, filing, cross-referencing, and bookkeeping.

The system has three working containers and one archive:

- **Courses**: long-running, evergreen study domains. Continuously editable. Never archived.
- **Projects**: bounded work with deliverables. Active until shipped or abandoned, then archived.
- **Resources**: cross-cutting reference articles distilled from project work and course study.
- **Archive**: completed projects and superseded material kept for reference.

Two derived spaces support navigation:

- **Sources**: summaries of external materials (papers, articles, videos) ingested into the wiki.
- **Dashboards**: agent-regenerated views over the working containers (active projects, review queue, recent activity).

## 2. Repository layout

```
courses/<course-id>/
    README.md                       # course overview, syllabus, status
    log.md                          # course-level events (ingests, restructures)
    NN-<module-slug>/
        README.md                   # canonical module page
        study/                      # raw study material (drafts, lecture notes)
            *.md
        exercises.md                # exercise set, optional
projects/<project-id>/
    README.md                       # scope, goals, success criteria, status
    plan.md                         # phased plan, milestones
    log.md                          # append-only build log
    learnings.md                    # distilled findings (promotion candidates)
    references.md                   # links to courses, resources, sources
resources/
    README.md                       # top-level resources index
    <category>/
        README.md                   # category index
        <entry-slug>.md             # individual resource article
sources/
    <year>/
        <source-slug>.md            # source summary
archive/
    projects/<project-id>/          # archived projects (preserved structure)
dashboards/
    active-projects.md
    review-queue.md
    recent-activity.md
templates/
    *.md                            # canonical templates for each page type
log.md                              # global event log
README.md                           # human-facing overview
CLAUDE.md                           # this file
STYLE.md                            # prose style guide (referenced from §8)
```

### Naming conventions

- Course IDs: kebab-case slugs (`ai-engineering`, `sql-deep-dive`).
- Module directories: zero-padded numeric prefix plus slug (`01-intro`, `02-rag-systems`).
- Project IDs: `proj-` prefix plus kebab-case slug (`proj-rag-prototype`).
- Resource categories: kebab-case (`python`, `sql`, `ai-engineering`).
- Source slugs: kebab-case author or short title (`vaswani-attention-is-all-you-need`).

## 3. Frontmatter schema

Every page has frontmatter. The agent owns all frontmatter and updates it on every edit. The user does not write or maintain frontmatter manually.

### Required fields, all pages

```yaml
---
type: <closed-vocabulary>
status: draft | review | polished | superseded | archived
created: YYYY-MM-DD
updated: YYYY-MM-DD
---
```

### Type vocabulary (closed)

| Type              | Purpose                                                  | Required additional fields              |
| ----------------- | -------------------------------------------------------- | --------------------------------------- |
| `course-readme`   | Course-level overview and syllabus                       | `course`                                |
| `module`          | Canonical module page within a course                    | `course`, `module`                      |
| `lecture`         | Long-form writeup, typically inside `study/`             | `course`, `module`                      |
| `exercise`        | Exercise set or problem set                              | `course`, `module`                      |
| `project-readme`  | Project scope and overview                               | `project`, `lifecycle`                  |
| `project-plan`    | Project phased plan                                      | `project`                               |
| `project-log`     | Append-only project build log                            | `project`                               |
| `project-learnings` | Distilled findings from a project                      | `project`                               |
| `resource`        | Cross-cutting reference article                          | `category`                              |
| `source`          | Summary of an external source                            | `source_url`, `source_type`             |
| `study`           | Generic raw study material (notes, drafts)               | `course`, `module` (or `project`)      |
| `dashboard`       | Agent-generated view                                     | none                                    |
| `index`           | Agent-generated directory index                          | none                                    |

### Curation fields, agent-maintained

```yaml
related:
  - <repo-relative-path>            # bidirectional, agent maintains symmetry
  - ...
sources:
  - title: <string>
    url: <string>
    cited_in: [<section-anchor>, ...]   # optional
```

### Type-specific fields

```yaml
# course-readme, module, lecture, exercise
course: <course-id>
module: <module-slug>                   # not required for course-readme

# project-readme
project: <project-id>
lifecycle: planning | building | shipped | archived
deliverable_url: <github-url>           # if applicable

# project-plan, project-log, project-learnings
project: <project-id>

# resource
category: <category-slug>
applies_to: [<list of contexts>]        # e.g. ['python', 'async-code']

# source
source_url: <url>
source_type: paper | article | video | book | talk | other
source_authors: [<string>, ...]
source_year: <YYYY>
ingested_into: [<course-id>, <project-id>, ...]
```

### Status semantics

- `draft`: work in progress. Default for new pages.
- `review`: ready for user review or promotion to canonical.
- `polished`: canonical, current, suitable as a reference.
- `superseded`: replaced by a newer version. Kept for history. Frontmatter must include `superseded_by: <path>`.
- `archived`: in `archive/`. Set when a project is archived.

## 4. Page conventions

### Canonical page plus study material

Every module and project has a canonical page (`README.md` in its directory) and a `study/` subdirectory holding the raw working material. The canonical page is the current best understanding. The `study/` directory holds drafts, lecture notes from sources, exercise drafts, and any working material the agent generates while studying or building.

When working on a module:

1. New material lands in `study/` first. Use slug like `lecture-2026-05-08.md` or `paper-vaswani.md`.
2. Once content stabilizes, the agent promotes the relevant parts into the canonical `README.md` and updates the source page's status.
3. Study material is preserved, not deleted, unless the user explicitly asks for cleanup.

### Study note shape

Study notes default to reference depth. The reader is a senior practitioner who returns to the note later when applying the topic; survey-level coverage is the wrong target. Each study note follows this shape:

1. **What it is**: concrete description of the topic.
2. **Why it matters**: practical justification, with the failure modes the topic addresses.
3. **Mechanics in depth**: features and patterns with what / when / why treatment per pattern.
4. **Things to remember**: gotchas and common mistakes.
5. **Where this fits**: cross-references to related notes.
6. **Sources**: links to canonical documentation.

When a topic spans both conceptual understanding and operational application, split into two notes along the what-is-it / how-to-apply-it line: e.g. `01-uv-overview.md` and `02-uv-in-practice.md`. When the seam is unclear, expand in place. Splitting where there is no seam fragments the topic; expanding where there is a seam buries the application material under conceptual material. Heuristic: if "mechanics in depth" is dominated by API-style usage, configuration, and recipes, split. If it is dominated by mental model, design rationale, and integrative concepts, single note.

### Module README shape

A module's `README.md` is a navigation page, not a content page. The shape:

1. **Title and metadata**: module number, block, estimated hours.
2. **Summary**: 1-2 paragraphs describing what the module covers and how it sits inside the course.
3. **Connections**: 1 paragraph on how this module's material flows into later modules and projects.
4. **Study material**: a table indexing the study notes. Columns: `#`, file, one-line topic. Notes flagged as deep get a `**Deep**` marker in the topic column.
5. **Exercises**: numbered list of exercises with one-line descriptions, pointing to `exercises.md`.
6. **Sources**: top-level sources for the module. Per-note source lists live inside each study note.

The table form for study material reads as a one-glance map of the module. Bullet lists with the same content scan less well at module sizes of 5-10 notes.

### Index files

Directories with multiple children carry a `README.md` that indexes the children. The agent regenerates these on every edit that adds, removes, or renames a child. Index files have `type: index` in frontmatter and a generated body.

The course-level `README.md` and module-level `README.md` are exceptions. They have `type: course-readme` and `type: module`, with hand-curated content above an agent-maintained index section. The agent updates the index section but preserves curated prose elsewhere.

### Logs

Three logs exist:

- `log.md` at repo root: global events (course creation, project archive, lint passes, major restructures).
- `<course>/log.md`: course-level events (source ingest, module promotion, scope changes).
- `<project>/log.md`: append-only project build log.

All log entries follow:

```
## [YYYY-MM-DD HH:MM] <verb> | <subject>
<one-line summary>
<optional details>
```

Verbs: `ingest`, `promote`, `archive`, `lint`, `restructure`, `create`, `note`, `decide`.

This format is grep-friendly: `grep "^## \[" log.md | tail -20` shows the last 20 events.

## 5. Templates

Templates live in `templates/`. The agent uses them when creating new pages. Templates define required sections; the agent fills them with content. If a template's required section is empty after content generation, the agent flags it for the user rather than removing the section.

Available templates:

- `templates/course-readme.md`
- `templates/module.md`
- `templates/lecture.md`
- `templates/exercise.md`
- `templates/project-readme.md`
- `templates/project-plan.md`
- `templates/project-log.md`
- `templates/project-learnings.md`
- `templates/resource.md`
- `templates/source.md`

When creating a new page type not covered by a template, the agent first proposes a template draft for the user to approve, then commits the new template before creating the page. Templates are versioned alongside the wiki.

## 6. Workflows

### 6.1 Curriculum design

Trigger: user names a topic and asks for a course on it.

1. Confirm scope with the user: target outcomes, estimated depth, prior background.
2. Web-search actively. Find current authoritative sources, recent papers, recommended readings, well-regarded tutorials. Prefer original sources over aggregators.
3. Draft a curriculum proposal: course description, prerequisites, ordered list of modules with one-line descriptions, suggested sources per module, capstone project sketch.
4. Present the proposal to the user. Iterate.
5. On approval, scaffold:
   - `courses/<course-id>/README.md` from `templates/course-readme.md`, populated.
   - `courses/<course-id>/log.md` with a `create` entry.
   - One directory per module: `NN-<module-slug>/README.md` from `templates/module.md`, with the description and source list filled in. `study/` directory empty.
6. Add a global log entry. Update `dashboards/active-projects.md` if any project was scaffolded as part of the course.

The course-readme's syllabus section lists modules with relative links and one-line descriptions. The agent regenerates this section when modules are added, renamed, or removed.

### 6.2 Module authoring

Trigger: a scaffolded module's `README.md` exists with the description and source list, but the `study/` directory is empty. The user wants to populate it.

Two paths, picked per module based on the user's prior context with the topic:

**Direct drafting** for review territory. The user has prior context on the topic; the module's purpose is to consolidate and reference, not to introduce. The agent drafts the study notes from the curriculum outline plus web-searched current state, presents them in batches, and iterates with the user.

**Student-teacher conversation first** for new learning territory. The user does not have prior context; the module is genuinely new material. The agent runs a chat conversation explaining the topic interactively, with the user asking questions and steering depth. Once the conversation has covered the module's planned scope, the agent synthesizes study notes from the conversation. This produces depth-calibrated notes (the conversation surfaces what the user actually needs explained at length vs. what they already understand) and avoids the rewrite cycle that comes from drafting at the wrong depth.

At module start, the agent asks which path the user prefers. A reasonable default by topic: direct for tooling and conventions modules, student-teacher for technical-domain modules where the user has not worked in the area before.

**Depth calibration check**: regardless of path, before generating study material at scale, the agent drafts one note at the proposed depth, presents it to the user, and waits for sign-off on the depth target. Module-scale rewrites (an hour or more of content recalibrated after the user reviews the whole module) are avoided this way.

Each study note follows the shape in Section 4.

### 6.3 Source ingestion

Trigger: user provides an external source (URL, paper, transcript) for a specific course or project.

1. Read the source. For URLs, fetch the page. For attachments, read the file.
2. Discuss key takeaways with the user briefly. Confirm which course or project the source belongs to and which module if applicable.
3. Write a source summary at `sources/<year>/<source-slug>.md` using `templates/source.md`. Include: bibliographic metadata, abstract or core thesis, key claims and findings, relevance to the wiki, ingestion target.
4. Update relevant pages:
   - If ingested into a course module, drop a study note at `<module>/study/source-<slug>.md` linking to the source summary, capturing the parts of the source that apply to this module specifically.
   - Update the canonical module `README.md` only if the source meaningfully changes its content. Bump `updated`. Add the source to the module's `sources:` list.
   - If the source contradicts existing content, flag the contradiction in the canonical page and surface it to the user before resolving.
5. Refresh `related:` arrays on touched pages.
6. Append a `## [date] ingest | <source-title>` entry to the course or project log, and a global log entry.

### 6.4 Project proposal and scoping

Trigger: user asks for a project, or the agent proposes one as part of curriculum design.

1. Agent proposes one or more project ideas grounded in current courses. Each proposal includes: working title, problem statement, proposed deliverable, skills exercised, estimated effort, course or resource ties, success criteria.
2. User selects and refines. Joint scoping conversation: scope boundaries, must-haves vs nice-to-haves, definition of done, deliverable target (private repo, public repo, LinkedIn writeup, or other).
3. On user approval, scaffold:
   - `projects/<project-id>/README.md` from `templates/project-readme.md`, populated with the agreed scope.
   - `projects/<project-id>/plan.md` from `templates/project-plan.md` with phased milestones.
   - `projects/<project-id>/log.md` with a `create` entry.
   - `projects/<project-id>/references.md` linking to relevant courses, resources, sources.
   - Empty `learnings.md` ready for entries.
4. Set `lifecycle: planning` on the project README.
5. Update global log and `dashboards/active-projects.md`.

### 6.5 Project execution

Trigger: user works on an active project. This workflow is most often run from Claude Code, not chat.

On each working session:

1. Read `README.md`, `plan.md`, `log.md`, `learnings.md` to load context.
2. Help the user execute the next plan step. Edit project files, write code, debug, etc.
3. At session end (or on request), append a log entry summarizing what was done, decisions made, and what is next. Format:

```
## [YYYY-MM-DD HH:MM] note | <session topic>
- Worked on: <what>
- Decided: <what and why>
- Blocked on: <what, if anything>
- Next: <what>
```

4. When a generalizable lesson surfaces, capture it in `learnings.md` immediately. A learnings entry is a candidate for promotion to `resources/`.
5. When entering a new lifecycle phase, update `lifecycle:` in the project README.
6. When the deliverable URL is known, update `deliverable_url:` in the project README.

### 6.6 Promote and demote

Promotion moves content from raw to canonical, or from project-bound to general.

**Study to canonical**: when content in `<module>/study/` matures (settled understanding, reviewed by user), the agent extracts the relevant material into the module's `README.md`, updates `updated:`, sets canonical status to `polished`, and sets the source study page's status to `superseded`. Study pages are kept for history.

**Project learning to resource**: when an entry in `<project>/learnings.md` generalizes beyond the project, the agent creates a new `resources/<category>/<slug>.md` from `templates/resource.md`, populates it from the learnings entry plus any necessary context, and links the project as the originating context. The original learnings entry stays but gains a `→ promoted to resources/<category>/<slug>.md` line.

**Resource to canonical**: not a separate step. Resources are canonical by default once polished.

Demotion is rare. If a polished page becomes outdated and a newer version supersedes it, the agent sets the old page's status to `superseded` with `superseded_by:` pointing at the replacement.

**Cleaning a deep note**: when an existing study note is already at reference depth but predates the current shape conventions (missing What/Why intro, missing Where-this-fits, contains inline version references), the agent runs a clean pass: add the What/Why intro and Where-this-fits outro to match the current shape, move any inline version references and changelog content into the sources list, and leave the substantive content in place. Cleaning is not rewriting; if the substance needs revising, that is a separate edit.

### 6.7 Archive

Trigger: user marks a project as shipped, abandoned, or stale.

1. Confirm with the user.
2. Update the project README's `lifecycle:` to `archived` and `status:` to `archived`. Add a final log entry.
3. Move the entire `projects/<project-id>/` directory to `archive/projects/<project-id>/`.
4. Update all `related:` arrays in the wiki that referenced the old path.
5. Refresh `dashboards/active-projects.md`.
6. Append a global log `archive | <project-id>` entry.

Archived projects are not deleted. Cross-references continue to work via the new path. Resources and learnings derived from the project remain in their respective locations.

### 6.8 Lint pass

Trigger: user requests a health check, or scheduled (weekly or monthly).

The lint pass is read-mostly. The agent reports findings and proposes actions. The user approves before any changes are made.

Checks:

1. **Stale pages**: pages with `updated:` older than 90 days where the topic has been mentioned in newer pages.
2. **Status drift**: pages stuck in `draft` or `review` for over 30 days.
3. **Promotion candidates**: study material that has stabilized and could be promoted to canonical, learnings entries that generalize and could become resources.
4. **Cross-reference integrity**: `related:` paths that no longer resolve, broken source URLs, missing inverse relations (page A links to B but B does not link back to A).
5. **Schema compliance**: pages missing required frontmatter fields for their type, pages with unknown types, pages outside the closed vocabularies.
6. **Orphan pages**: pages with no inbound `related:` and no inbound index entry.
7. **Duplicate content**: similar topics covered in multiple resources or modules without cross-reference.
8. **Source freshness**: cited sources that may have been superseded by newer work (web search to verify).

Output: a markdown report at `dashboards/lint-<YYYY-MM-DD>.md` with categorized findings and proposed actions. The user approves a subset and the agent executes.

### 6.9 Dashboard regeneration

Dashboards are regenerated on demand. The agent reads relevant pages and writes the dashboard markdown. Standard dashboards:

- `active-projects.md`: every project where `lifecycle != archived`. Columns: project, lifecycle, last log entry, blocked status.
- `review-queue.md`: every page where `status: review`. Columns: page, type, last updated, why it is in review.
- `recent-activity.md`: last 30 entries from `log.md` plus most recently updated pages.

The agent regenerates dashboards when relevant state changes (project lifecycle update, status change, log entry) or on user request.

## 7. Cross-references and indexing

The agent maintains three forms of cross-reference:

1. **`related:` frontmatter**: bidirectional. When the agent edits page A and adds B to A's `related:`, it also adds A to B's `related:`. Lint catches drift.
2. **Index sections in directory READMEs**: agent-regenerated lists of children with one-line summaries. The "Modules" section in a course README, the "Entries" section in a resource category README.
3. **Inline links in prose**: when content references another page, link to it with relative paths. The agent prefers relative paths over `related:` for in-prose references.

The user can navigate by:

- Reading a course or project README and following inline links.
- Browsing index sections at any level.
- Searching with ripgrep or similar over the repo.
- Asking the agent for a topic and getting a synthesized answer with citations.

## 8. Style rules for content

Content the agent writes follows these structural rules:

- **Prose first**: long-form articles, not lists, are the default. Lists are for genuinely list-shaped content (steps, exercises, structured comparisons).
- **Code-heavy is expected**: technical articles include code snippets with sufficient context, especially the when, why, and how to apply patterns described.
- **Citations**: every non-trivial claim cites a source. Use ``-style inline references where applicable, or footnote-style citations linking to the source page in `sources/`.
- **Sentence case** in headings.
- **Working-out and exercises** belong in `study/` or in module `exercises.md`, not in canonical pages.
- **Reference depth, not survey depth**: study notes default to reference depth. The reader is a senior practitioner returning later when applying the topic.
- **Version numbers and dates belong in sources**: body text says "modern Python supports X"; sources list says "PEP 695, Python 3.12+". Inline release dates and "as of April 2026" framings date the page in a way that requires future maintenance.
- **License mentions are decision-relevant or omitted**: include licensing details when they affect a reader's choice (open-weight model selection, GDPR-sensitive deployments). Omit license mentions for common tools where the reader will not be making a license-driven decision.

For prose style (the rules about sentence-level phrasing, vocabulary, rhetorical patterns to avoid, and the polish-pass checklist), see `STYLE.md`. The polish checklist in `STYLE.md` Section 7 is run before any content is presented to the user.

## 9. Initial state

This wiki is empty at creation. The first action is typically a curriculum design for a chosen topic. After approval, the agent scaffolds the course and the system grows from there.

## 10. Open conventions

These are conventions the agent and user may evolve together. Document changes in this file and add a global log entry.

- Whether courses get distilled long-term summaries (a `summary.md` at course root). Off by default; turned on per-course when the user requests it.
- Whether resources support sub-categories or stay one level deep. Default: one level. Promote to sub-categories only when a category exceeds ~20 entries.
- Whether project deliverable repos should be git submodules or external links. Default: external links via `deliverable_url:`.
- Frequency of scheduled lint passes. Default: ad hoc, user-triggered.

When in doubt, the agent proposes a convention to the user, gets approval, and updates this file.
