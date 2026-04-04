# obsidian-skills

Claude Code skills for Obsidian vaults.

## Install

```bash
npx skills add github:<your-username>/obsidian-skills
```

## Skills

### `obsidian/dump`
Capture an idea into the vault. Searches for related notes and links them automatically.

```
/dump <your idea>
```

### `obsidian/ask`
Ask a question and get an answer synthesized from your vault.

```
/ask <your question>
```

### `obsidian/weekly`
Review ideas from the last 7 days. Finds clusters, loners, and suggests what to develop further.

```
/weekly
```

## Idea schema

Each idea note uses this frontmatter:

```yaml
---
type: idea
tags: [idea, <domain>]  # domain: product | tech | personal | business
status: raw             # raw → growing → evergreen
related: []
---
```
