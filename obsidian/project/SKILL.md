---
name: project
description: Create a new project note in the vault root
---

Create a new project note in the vault root (`/`).

## Arguments

Parse from the user's input (all optional except name):
- **name** — project title (required)
- **goal** — annual goal this project belongs to, e.g. `2026` (formatted as wikilink `[[2026]]`)
- **status** — `active` (default), `pause`, or `archive`
- **priority** — number `1`–`5` (default `3`)
- **tags** — domain tags, e.g. `career`, `personal`, `health`, `product`
- **target_date** — deadline in `YYYY-MM-DD` format

## Steps

1. **Parse the input** — extract name and any optional fields.

2. **Build the filename** — `Проект — <name>.md` (keep the user's language for the name).

3. **Create the file** using obsidian-cli:
```bash
obsidian create path="Проект — <name>.md" content="..." silent
```

If obsidian-cli fails, use the Write tool to create the file directly in the vault root.

4. **File template:**

```markdown
---
type: project
status: <status>
goal: "[[<goal>]]"
priority: <priority>
start_date: <today YYYY-MM-DD>
target_date: <target_date>
review_last: <today YYYY-MM-DD>
review_next: <today + 7 days YYYY-MM-DD>
tags:
  - project
  - <domain tag>
---

## Outcome


## Definition of Done

- [ ]

## Wiki


## Children

![[tasks.base#children]]
```

5. **Report back** — confirm the project was created with its key fields.
