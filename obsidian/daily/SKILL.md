---
name: daily
description: Open or create today's journal entry with correct week frontmatter
---

Open today's daily note. If it doesn't exist, create it with the correct `week` frontmatter.

## Steps

1. **Compute today's values** using bash:
```bash
today=$(date +"%Y-%m-%d")
week=$(date +"%Y-W%V")
echo "$today $week"
```

2. **Check if today's note exists**:
```bash
obsidian print file="$today" 2>&1
```

3. **If it doesn't exist, create it**:
```bash
obsidian create path="journal/$today.md" content="---\ntype: daily\nweek: $week\n---\n\n## Цели\n\n## Заметки\n" silent
```

4. **Open the note**:
```bash
obsidian open file="$today"
```

5. **Report back** — confirm the note was opened or created, show today's date and week.

## Notes

- Prefer `## Цели` over `## Фокус` in daily notes by default.
- Do not add a separate `## Фокус` block unless the user explicitly asks for it.
