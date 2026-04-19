# Attribution header

Every skill file in this repository carries the following header inside the SKILL.md body (immediately after the frontmatter), as an HTML comment so it doesn't render in previews but remains in the source:

```markdown
<!--
Based on the JARVIS framework by Taoufik — @TaoufikAI — https://wakejarvis.ai
Licensed under MIT with attribution requirement. See LICENSE at the repo root.
-->
```

## When adding a new skill

1. Create `skills/<skill-name>/SKILL.md`
2. Paste the attribution block (above) as the first non-frontmatter content
3. Write the skill body below it

## When forking or remixing

Keep the attribution block in every published SKILL.md. If you change the skill significantly, add your own line below ours — don't replace ours.

Example for a modified skill:

```markdown
<!--
Based on the JARVIS framework by Taoufik — @TaoufikAI — https://wakejarvis.ai
Modified by [Your Name] — [your handle or URL] — [date]
Licensed under MIT with attribution requirement.
-->
```
