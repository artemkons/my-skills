---
name: research
description: Deep research on any topic — searches the web, reads sources, synthesizes a report with citations. Supports depth modes: quick, normal (default), deep.
---

Conduct deep research on a topic using web search and source analysis.

The input is: $ARGUMENTS

## Step 0. Parse depth mode

Check if $ARGUMENTS starts with a depth keyword:
- `quick <topic>` — fast scan, fewer sources
- `deep <topic>` — thorough analysis, more sources
- anything else — normal mode (default)

Set parameters based on mode:

| Mode | Sub-queries | Sources to read | Report sections |
|------|-------------|-----------------|-----------------|
| quick | 3 | 3–4 | TL;DR + Key takeaways only |
| normal | 4–6 | 5–7 | Full report |
| deep | 7–9 | 10–14 | Full report + extra sections for contradictions and open questions |

## Steps

### 1. Understand the scope
Parse the topic. What are the key dimensions to explore? What would make this research complete and useful?

### 2. Plan sub-queries
Break the topic into sub-queries (count per mode) that cover the topic from different angles:
- Core concept / definition
- How it works / mechanics
- Real-world examples / case studies
- Tradeoffs, criticisms, limitations
- Current state / recent developments
- Practical applications
- (deep only) Edge cases, controversies, minority views
- (deep only) Academic / primary sources

### 3. Search each sub-query
Use WebSearch for each sub-query. Run them in parallel where possible. Collect the most relevant results (title + URL + snippet).

### 4. Read key sources
From the search results, pick the top URLs (count per mode). Use WebFetch (or the `defuddle` skill for articles) to read each one. Extract key facts, arguments, and insights.

### 5. Synthesize the report
Write a structured report in the user's language:

**quick mode:**
```
# [Topic]
## TL;DR
## Key takeaways
## Sources
```

**normal / deep mode:**
```
# [Topic]

## TL;DR
2–3 sentence summary of the most important finding.

## [Section per key dimension]
Substantive analysis, not just summaries. Connect ideas across sources. Surface tensions and tradeoffs.

## Key takeaways
Bullet list of the most actionable or insightful points.

## Sources
- [Title](URL) — one-line description of what this source contributed
```

**deep mode adds:**
```
## Where sources disagree
## Open questions worth exploring further
```

### 6. Save to vault (optional)
If the research is substantial and worth keeping — ask the user if they want it saved as a note in the vault. If yes, save it as a markdown file in the vault root with:

```yaml
---
type: research
tags: [research, <domain>]
date: <YYYY-MM-DD>
---
```

## Quality bar
- Cite specific claims, not just general topics
- Surface what's surprising or non-obvious
- Flag where sources disagree
- Don't pad — if 3 sections cover it well, don't add 5 more
