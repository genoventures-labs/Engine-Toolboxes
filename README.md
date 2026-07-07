<div style="display: flex; align-items: center; gap: 15px;">
  <img src="https://github.com/genoventures-labs/Switchbay-Engines/raw/main/assets/SMark.png" width="60" alt="Switchbay Logo" />
  <h1 style="margin: 50;">Switchbay: Engine Toolboxes</h1>
</div>


[![Switchbay - Engine Bay](https://img.shields.io/badge/Switchbay_-Engine_Toolboxes-royalblue)](#)
[![Switchbay - Templates](https://img.shields.io/badge/Switchbay_-Agent_Skills-green)](#)

**Switchbay's engine toolboxes** are Reusable skill cards for Switchbay agents. They allow you to build your own custom skills for your Switchbay agents. You can use these skills to build your own custom applications and workflows. 

---

### What's Switchbay?:
> Switchbay is a terminal-first AI coding workbench for developers who want cloud intelligence, local control, and provider independence without rebuilding their workflow every time the model stack changes.

### What are Engine Toolboxes?:
> Engine Toolboxes are reusable skill cards for Switchbay agents. They allow you to build your own custom skills for your Switchbay agents. You can use these skills to build your own custom applications and workflows. 

### What are Engine Toolboxes used for?:
> Engine Toolboxes are used to build your own custom skills for your Switchbay agents. You can use these skills to build your own custom applications and workflows. For example, you can use an Engine Toolbox to build a skill that allows you to search the web for information, or a skill that allows you to generate code. 

### What's Inside Engine Toolboxes?:
> Inside this repository you'll find a variety of agent skills, built using the example provided below. Ranging from simple skills to complex skills, and from small to large. 

### Tool Syntax for Engine Toolboxes:
> This is the syntax for the Engine Toolbox. It is a markdown file that contains the skill's name, description, languages, agents, tags, and triggers. 

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
**Example:** [Default Template](./templates/default.skill.md)


### Syncing with Switchbay Engine Toolboxes:
> To sync your skills with Switchbay, you can use the following command:

```bash
switchbay toolbox sync
```

---

*Switchbay: Terminal-first. Developer-first. Provider-agnostic.*



