# Engine Toolboxes

Reusable skill cards for Switchbay agents.

Switchbay syncs this repo with:

```bash
switchbay toolbox sync
```

Skills are markdown files with frontmatter:

```markdown
---
id: my-skill
name: My Skill
description: A reusable working method.
languages: [any]
agents: [any]
tags: [workflow]
triggers: [when this should be used]
---

# My Skill

## Use When

## Method

## Output

## Guardrails
```

Put reusable skills in `skills/` and templates in `templates/`.
