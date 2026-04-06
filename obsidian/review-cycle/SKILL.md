---
name: review-cycle
description: Run a task through a multi-agent loop — executor → reviewer → fixer — until APPROVED or max 3 iterations. Each agent receives the full accumulated context.
---

Run a task through a multi-agent review cycle.

The task is: $ARGUMENTS

## Context

Keep track of these values throughout the cycle:
- `task` — the original task ($ARGUMENTS)
- `result` — current best result (updated after each fix)
- `history` — list of all (result, review) pairs from previous iterations
- `iteration` — current iteration number (start at 1)

## Steps

### 1. Execute

Run an Agent (general-purpose) with this prompt:

```
You are working inside an Obsidian vault.

Task: {task}

Read the vault to understand existing context — search for related notes, check relevant files.
Then perform the task. Return only the result (file content, analysis, plan, etc.) — no meta-commentary.
```

Save the response as `result`. Set `history = []`.

---

### 2. Review → Fix loop (max 3 iterations)

Repeat the following until the reviewer responds with APPROVED or iteration reaches 3:

**2a. Run reviewer Agent (general-purpose):**

```
You are a strict reviewer working inside an Obsidian vault.

Original task: {task}

Current result (iteration {iteration}):
{result}

Previous attempts and feedback:
{history}

Review the result against the task. Check:
- Does it fully accomplish the task?
- Are there gaps, errors, or missing parts?
- In Obsidian context: correct frontmatter, wikilinks to related notes, proper structure?

If the result is good enough — respond with exactly: APPROVED
If not — respond with a numbered list of specific, actionable issues only. No praise.
```

**2b. Check the review:**
- If the response is exactly `APPROVED` → skip to Step 3.
- Otherwise save as `review`, append `{ iteration, result, review }` to `history`, continue.

**2c. Run fixer Agent (general-purpose):**

```
You are working inside an Obsidian vault. Your job is to fix a result based on reviewer feedback.

Original task: {task}

Current result:
{result}

Reviewer feedback:
{review}

Full history of previous attempts:
{history}

Fix the result. Address every reviewer issue. Do not introduce new problems.
Return only the fixed result — no meta-commentary.
```

Save the response as `result`. Increment `iteration`.

---

### 3. Done

Return the final `result` to the user.

Also report:
- How many iterations it took
- Whether it ended with APPROVED or hit the max iteration limit
- A one-line summary of what changed across iterations (if any)
