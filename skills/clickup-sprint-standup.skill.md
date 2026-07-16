---
id: clickup-sprint-standup
name: ClickUp Sprint And Standup
description: Build grounded sprint, standup, assigned-work, and overdue summaries from live ClickUp data.
engine: clickup
languages: [any]
agents: [project-operator, architect]
tags: [clickup, sprint, standup, overdue, planning]
triggers: [daily standup, clickup summary, current sprint, sprint status, overdue tasks, what am i working on]
---

# ClickUp Sprint And Standup

## Method

1. Use `clickup_summary` for the default daily completed, in-progress, and overdue view.
2. Use `clickup_sprint` when the user asks about the active sprint.
3. Use `clickup_assigned` for the broader personal workload and `clickup_overdue` for deadline risk.
4. Open important or ambiguous tasks with `clickup_task` before drawing conclusions.
5. Separate facts from recommendations; include task identifiers beside proposed actions.

## Output

- Completed work
- Work in progress
- Blocked or overdue work
- Next actions, each tied to a real task

## Guardrails

- Do not call an empty result “all clear” when authentication or sprint detection failed.
- Do not change task state during a reporting request.
- State the time window and active ClickUp workspace when available.
