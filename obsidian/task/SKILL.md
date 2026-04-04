---
name: task
description: Create a new task in the vault tasks/ folder
---

Create a new task note in the `tasks/` folder.

## Arguments

Parse from the user's input (all optional except name):
- **name** — task title (required)
- **project** — project name, e.g. `AI компаньон` (formatted as wikilink)
- **priority** — `high`, `medium` (default), or `low`
- **scheduled** — date in `YYYY-MM-DD` format
- **due** — date in `YYYY-MM-DD` format
- **effort** — estimated duration in minutes (e.g. `30`, `90`, `120`)

## Steps

1. **Parse the input** — extract name and any optional fields.

2. **Create the file** using obsidian-cli:
```bash
obsidian create path="tasks/<task name>.md" content="---\ntype: task\nstatus: todo\npriority: <priority>\nscheduled: <scheduled>\ndue: <due>\neffort: <effort>\nproject: \"[[<project>]]\"\n---\n" silent
```

If obsidian-cli fails, use the Write tool to create `tasks/<task name>.md` directly.

3. **Report back** — confirm the task was created with its key fields.
