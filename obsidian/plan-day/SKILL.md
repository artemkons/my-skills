---
name: plan-day
description: Plan the day by fitting tasks into 90-min work blocks and pushing to Google Calendar
---

Build a day plan from today's tasks, fit them into work blocks, and create Google Calendar events.

When writing the plan into a daily note, prefer a `## Цели` block as the main high-signal summary of the day. Do not add a separate `## Фокус` block unless the user explicitly asks for it. Daily goals should be checked against the current week's goals before finalizing the plan.

## Work schedule

Work starts at 10:00, ends at 19:00. The rhythm is: **90 min work → 20 min break**, repeating until end of day. Breaks are NOT added to calendar — they stay as free windows.

Blocks are computed dynamically — do not hardcode them. The cursor starts at 10:00 and advances through work/break cycles. If an existing event occupies some time, the cursor jumps past it and the 90/20 rhythm continues from there.

Total capacity: ~450 min / day (5 full cycles).

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

4. **Check existing GCal events** — before scheduling, fetch all events for the target day using gcal_list_events (timeMin=day 10:00, timeMax=day 19:00, timezone=Europe/Moscow). Build a list of occupied time ranges to avoid overlaps.

5. **Pack tasks into blocks** using this algorithm:
   - Available slots: `[(10:00, 11:30), (11:50, 13:20), (13:40, 15:10), (15:30, 17:00), (17:20, 18:50)]`
   - Subtract already occupied time ranges (from existing GCal events) from available slots
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

8. **If writing into the daily journal**:
   - Read the current weekly note first and align the day's goals with the week's goals
   - Prefer sections like `## Разбор вчера`, `## Сверка с неделей`, `## Цели`, `## План дня`, `## Итог дня`
   - Treat `## Цели` as the main motivating and prioritizing block
   - Do NOT add `## Фокус` unless the user explicitly requests that format

## Notes

- Do NOT create events for break times — breaks are free windows between blocks
- If a task spans multiple blocks, create separate GCal events per segment with the same task name
- Overdue tasks (due < today) get scheduled first within their priority group
- If the user specifies a date argument (e.g. `/plan-day 2026-04-07`), plan that day instead of today
