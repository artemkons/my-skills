---
name: skill-maintainer
description: Update skill instructions and keep the source repo and local installed skills in sync
---

Update one or more Codex skills without forgetting the source repository.

## Steps

1. **Identify the affected skills** — determine which skill folders or files need changes from the user's request.

2. **Treat the source repo as canonical** — always inspect and update the corresponding files in `/Users/artemkonstantinov/Projects/my-skills` first.

3. **Mirror local installed copies** — after updating the source repo, make the same change in the installed skills under `/Users/artemkonstantinov/.agents/skills` when the matching skill exists there.

4. **Keep behavior aligned** — if the change affects templates, defaults, or planning rules, update all directly related skills too, not just the one explicitly named.

5. **Check for documentation drift** — if a new skill is added or behavior changes materially, update the repo `README.md` so the skill set stays discoverable.

6. **Report back clearly** — list:
   - which source repo files changed
   - which local installed skill files changed
   - whether anything could not be mirrored

## Notes

- Prefer small, explicit instruction changes over vague reminders.
- Default assumption: `/Users/artemkonstantinov/Projects/my-skills` is the main repository and local installed skills are derived copies.
- When there is a conflict, keep the repo and local installed version semantically identical unless the user explicitly asks for divergence.
