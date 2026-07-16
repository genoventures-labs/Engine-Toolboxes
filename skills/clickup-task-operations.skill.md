---
id: clickup-task-operations
name: ClickUp Task Operations
description: Read, find, create, update, assign, and discuss ClickUp tasks through the guarded ClickUp Ops engine.
engine: clickup
languages: [any]
agents: [project-operator, backend, reviewer]
tags: [clickup, tasks, project-management, comments, assignments]
triggers: [clickup task, find task, create task, update task, task status, assign task, task comment, task activity]
---

# ClickUp Task Operations

## Method

1. Run `clickup_auth` when the connection or active workspace is unknown.
2. Find the target with `clickup_search`, then confirm identity with `clickup_task` or `clickup_activity` before changing it.
3. Use the narrowest mutation tool: create, status, priority, assignment, or comment.
4. Present the exact target and intended change at the approval gate.
5. Re-read the task after mutation and report the resulting status or returned task ID.

## Guardrails

- Never infer a task ID from a similar title when multiple results exist.
- Treat task URLs and custom IDs as valid identifiers.
- Do not use `clickup_delete_task` unless the user explicitly asks to delete that exact task.
- Never expose ClickUp tokens or configuration secrets.
