---
id: clickup-work-delivery
name: ClickUp Work Delivery
description: Carry a ClickUp task through repository work, verification, status updates, and an evidence-based completion comment.
engine: clickup
languages: [any]
agents: [project-operator, backend, reviewer]
tags: [clickup, delivery, implementation, commit, verification]
triggers: [work this clickup task, implement clickup task, finish task, move to review, comment commit hash]
---

# ClickUp Work Delivery

## Method

1. Read `clickup_activity` before touching the workspace; extract acceptance criteria, constraints, and prior discussion.
2. Confirm the active repository matches the task. Keep the ClickUp task ID visible in the work plan.
3. Implement and verify the requested work using normal workspace tools.
4. Summarize changed files, tests, unresolved risks, and the commit or branch reference.
5. Ask approval before posting `clickup_comment` or calling `clickup_update_status`.
6. Re-read the task after approved updates and report the final ClickUp state.

## Guardrails

- Never mark work complete when verification failed or required work remains.
- Never invent a commit hash, test result, task field, or acceptance criterion.
- Keep ClickUp updates concise and useful to collaborators; do not paste internal chain-of-thought.
- Status names vary by workspace. Read the task first and use the user's requested status wording.
