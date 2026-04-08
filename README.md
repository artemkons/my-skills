# obsidian-skills

Claude Code skills for Obsidian vaults.

## Install

```bash
npx skills add git@github.com:artemkons/my-skills.git
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

### `obsidian/skill-maintainer`
Updates skill instructions and keeps the source repo plus local installed copies in sync.

```
/skill-maintainer <what to change in the skills>
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
