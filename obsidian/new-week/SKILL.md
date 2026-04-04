---
name: new-week
description: Create a new weekly planning note in planning/ with correct quarter frontmatter
---

Create a weekly planning note for the given week (or current week if not specified).

## Arguments

- **week** — ISO week in format `YYYY-WWW` (e.g. `2026-W15`). Defaults to current week.

## Steps

1. **Determine the week** — if no argument given, compute current week:
```bash
week=$(date +"%Y-W%V")
echo $week
```

2. **Compute the quarter** from the week's Monday date:
```bash
# Get Monday of the given week
monday=$(date -j -f "%Y-W%V-%u" "${week}-1" +"%Y-%m-%d" 2>/dev/null || date -d "${week}-1" +"%Y-%m-%d")
month=$(date -j -f "%Y-%m-%d" "$monday" +"%m" 2>/dev/null || date -d "$monday" +"%m")
year=$(date -j -f "%Y-%m-%d" "$monday" +"%Y" 2>/dev/null || date -d "$monday" +"%Y")
quarter=$(( (10#$month - 1) / 3 + 1 ))
quarter_str="${year}-Q${quarter}"
echo "$monday $quarter_str"
```

3. **Check if the note already exists**:
```bash
obsidian print file="$week" 2>&1
```

4. **Create the note**:
```bash
obsidian create path="planning/$week.md" content="---\ntype: weekly-plan\nquarter: $quarter_str\n---\n\n## Фокус недели\n\n## Проекты в работе\n\n![[planning/days.base]]\n\n## Итоги\n" silent
```

5. **Open the note**:
```bash
obsidian open file="$week"
```

6. **Report back** — confirm week note created, show week and quarter.
