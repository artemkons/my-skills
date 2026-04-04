Capture an idea into the vault.

The user's idea is: $ARGUMENTS

## Steps

1. **Understand the idea** — read the input, extract the core concept. If the idea is in Russian, keep everything in Russian. Match the user's language.

2. **Choose a filename** — concise noun phrase that names the idea. No dates, no prefixes. E.g. `Автоматическая линковка идей.md`

3. **Determine domain tag** — pick one: `product`, `tech`, `personal`, `business`. Pick what fits best from context.

4. **Find related notes** — search the vault root for notes that share keywords or concepts with this idea. Look at filenames and content. Be thorough.

5. **Create the note** with this exact frontmatter:

```yaml
---
type: idea
tags: [idea, <domain>]
status: raw
related: []
---
```

After the frontmatter, write 1–3 sentences expanding on the idea — what it is, why it matters. Then add a `## Related` section with wikilinks to related notes (if any found).

6. **Update related notes** — for each related note found, open it and add a wikilink back to the new idea. Add it to their `## Related` section (create the section if it doesn't exist), or append to their `related:` frontmatter list.

7. **Report back** — tell the user:
   - The note that was created (filename)
   - Related notes that were linked (if any)
   - A one-line observation: what this idea connects to in the vault
