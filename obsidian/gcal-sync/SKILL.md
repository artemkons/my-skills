---
name: gcal-sync
description: Sync tasks with scheduled or due dates to Google Calendar
---

Push tasks from the vault to Google Calendar. Only syncs tasks that have a date and aren't done. Skips tasks already synced (have a `gcal_event_id`).

## Steps

1. **Find tasks** — read all `.md` files in `tasks/` folder. Use Glob or obsidian-cli:
```bash
obsidian list path="tasks"
```

2. **Read each task** — for each file, read frontmatter fields: `status`, `scheduled`, `due`, `priority`, `project`, `gcal_event_id`.

3. **Filter** — keep only tasks where:
   - `status != "done"`
   - `scheduled` or `due` is set
   - `gcal_event_id` is empty (not yet synced)

4. **Create GCal events** — for each filtered task, create an all-day event:
   - Calendar: primary (`konstantinov050701@gmail.com`)
   - Title: task file name
   - Date: `scheduled` if set, otherwise `due`
   - Description: `due: <due date>` + `priority: <priority>` + `project: <project>`

5. **Write back event ID** — after creating each event, add `gcal_event_id` to the task's frontmatter using obsidian-cli:
```bash
obsidian property:set name="gcal_event_id" value="<event_id>" file="<task name>"
```

6. **Report** — show a summary: how many events created, how many skipped.

## Notes

- If a task has both `scheduled` and `due`, use `scheduled` as the event date and put `due` in the description.
- If the task title contains special characters, handle quoting carefully.
- On re-run, tasks with `gcal_event_id` are skipped — to force re-sync, clear the field manually.
