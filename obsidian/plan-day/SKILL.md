---
name: plan-day
description: Plan the day by fitting tasks into 90-min work blocks and pushing to Google Calendar
---

Build a day plan from today's tasks, fit them into work blocks, and create Google Calendar events.

## Work schedule

Blocks run 10:00–19:00 with 20-min breaks between blocks. Breaks are NOT added to calendar — they stay as free windows.

```
Block 1:  10:00 – 11:30
Block 2:  11:50 – 13:20
Block 3:  13:40 – 15:10
Block 4:  15:30 – 17:00
Block 5:  17:20 – 18:50
```

Total capacity: 450 min / day.

## Steps

1. **Get today's date**:
```bash
today=$(date +"%Y-%m-%d")
echo $today
```

2. **Find today's tasks** — read all files in `tasks/`. For each, read frontmatter and collect tasks where:
   - `status != "done"`
   - `scheduled == today` OR `due <= today`

   Use Glob to find `tasks/*.md`, then Read each file for frontmatter fields: `status`, `priority`, `scheduled`, `due`, `effort`, `project`, `gcal_event_id`.

3. **Sort tasks** by priority: high → medium → low. Within same priority, due date ascending.

4. **Pack tasks into blocks** using this algorithm:
   - Available slots: `[(10:00, 11:30), (11:50, 13:20), (13:40, 15:10), (15:30, 17:00), (17:20, 18:50)]`
   - Track current position (slot index + minutes used in current slot)
   - For each task with `effort`:
     - If task fits in remaining time of current slot → schedule it there
     - If task doesn't fit → move to start of next slot, schedule there
     - If task effort > 90 min → fill current slot, then continue in next slot after the break (task spans multiple slots, but GCal events are created per-slot segment)
   - Tasks without `effort` → assign 90 min (one full block) by default
   - Track overflow tasks (don't fit in any block)

5. **Create GCal events** for each scheduled task segment:
   - Calendar: `konstantinov050701@gmail.com`
   - Title: task file name
   - Start/end: exact times from packing algorithm (RFC3339 with Europe/Moscow timezone)
   - Description: `priority: X | effort: Xmin | project: X`
   - Skip tasks that already have `gcal_event_id` set (already scheduled)

6. **Write back event IDs** — for each created event, set `gcal_event_id` on the task:
```bash
obsidian property:set name="gcal_event_id" value="<event_id>" file="<task name>"
```

7. **Report**:
   - Show the day plan as a table (time | task | effort)
   - Total capacity used vs 450 min
   - List overflow tasks that didn't fit
   - Warn if total effort exceeds capacity

## Notes

- Do NOT create events for break times — breaks are free windows between blocks
- If a task spans multiple blocks, create separate GCal events per segment with the same task name
- Overdue tasks (due < today) get scheduled first within their priority group
- If the user specifies a date argument (e.g. `/plan-day 2026-04-07`), plan that day instead of today
