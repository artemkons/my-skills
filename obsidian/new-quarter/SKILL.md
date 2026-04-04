---
name: new-quarter
description: Create a new quarterly planning note in planning/ with correct year frontmatter
---

Create a quarterly planning note for the given quarter (or current quarter if not specified).

## Arguments

- **quarter** — in format `YYYY-QN` (e.g. `2026-Q2`). Defaults to current quarter.

## Steps

1. **Determine the quarter** — if no argument given, compute current quarter:
```bash
month=$(date +"%m")
year=$(date +"%Y")
quarter=$(( (10#$month - 1) / 3 + 1 ))
quarter_str="${year}-Q${quarter}"
echo $quarter_str
```

2. **Extract year** from the quarter string:
```bash
year=$(echo $quarter_str | cut -d'-' -f1)
echo $year
```

3. **Check if the note already exists**:
```bash
ls planning/$quarter_str.md 2>/dev/null && echo "exists" || echo "not found"
```

4. **Create the note**:
```bash
obsidian create path="planning/$quarter_str.md" content="---\ntype: quarterly-plan\nyear: \"$year\"\n---\n\n← [[$year]]\n\n## Фокус квартала\n\n## Приоритетные проекты\n\n## Итоги\n\n---\n\n![[planning/weeks.base]]\n" silent
```

5. **Open the note**:
```bash
obsidian open file="$quarter_str"
```

6. **Report back** — confirm quarter note created, show quarter and year.
