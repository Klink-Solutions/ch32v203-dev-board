# Workflow

## Tracking work

All work is tracked in **GitHub Issues**, organized on a **Projects board**.

- One issue per piece of work. Use the issue templates (Task, Requirement, Decision).
- Labels show the phase (`phase: define`, `phase: design`, `phase: layout`, `phase: build`,
  `phase: validate`) and type (`task`, `requirement`, `decision`, `question`).
- Milestones group issues into stages (e.g. "1. Define", "2. Design").
- Board columns: **Backlog -> In progress -> Blocked -> Done**.

## Commits

- Reference the issue in the commit message: `Draft electrical requirements (#2)`.
  Use `Closes #N` in the last commit or PR that finishes it.
- Commit Fusion exports (schematic/layout PDFs and archives) at milestones, not every edit.

## Records

- Dated notes go in [`docs/journal/`](docs/journal/).
- Significant design choices get a record in [`docs/decisions/`](docs/decisions/).
- Requirements live in [`docs/requirements/`](docs/requirements/).
