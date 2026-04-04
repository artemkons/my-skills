---
name: task
description: Create a new task in the vault tasks/ folder
---

Create a new task note in the `tasks/` folder.

## Arguments

Parse from the user's input (all optional except name):
- **name** — task title (required, can be the whole input if no other args)
- **project** — project name, e.g. `AI компаньон` (will be formatted as wikilink)
- **priority** — `high`, `medium` (default), or `low`
- **scheduled** — date in `YYYY-MM-DD` format
- **due** — date in `YYYY-MM-DD` format

## Steps

1. **Parse the input** — extract name and any optional fields from the user's message.

2. **Build frontmatter**:
```yaml
---
type: task
status: todo
priority: <priority or "medium">
scheduled: <scheduled or empty>
due: <due or empty>
project: "<[[Project Name]]> or empty"
---
```

3. **Create the file** using obsidian-cli:
```bash
obsidian create name="<task name>" path="tasks/<task name>.md" content="---\ntype: task\nstatus: todo\npriority: <priority>\nscheduled: <scheduled>\ndue: <due>\nproject: \"[[<project>]]\"\n---\n" silent
```

If obsidian-cli fails, use the Write tool to create `tasks/<task name>.md` directly.

4. **Report back** — confirm the task was created with its key fields.
