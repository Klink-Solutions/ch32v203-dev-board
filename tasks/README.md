# Task Tracking

Tasks live in the repo as markdown files so every change is versioned alongside the design.

## How it works

- One file per task: `NNN-short-title.md` (copy [`templates/task-template.md`](templates/task-template.md)).
- Open tasks live in [`active/`](active/). When finished, set `Status: Done`, fill in the
  completion date, and move the file to [`done/`](done/) (`git mv`).
- [`INDEX.md`](INDEX.md) is the at-a-glance list. Update it when you add or finish a task.
- Task numbers are never reused.
- Reference tasks in commit messages (e.g. `task 002: draft electrical requirements`).

## Statuses

`Todo` -> `In progress` -> `Blocked` (note why) -> `Done`

## Suggested phases

1. **Define:** goals, requirements, tool and form-factor decisions
2. **Design:** architecture, component selection, schematic
3. **Layout:** PCB layout, review, outputs
4. **Build:** order, assemble, bring-up
5. **Validate:** test against requirements, document results
