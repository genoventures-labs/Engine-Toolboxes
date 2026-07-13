---
id: macos-sensitive-actions
name: macOS Sensitive Actions
description: Handle screenshots, scripts, clipboard, notifications, and other local Mac actions with explicit safety boundaries.
languages: [shell, applescript, python, any]
agents: [devops, security, debugger, any]
tags: [macos, safety, approvals, clipboard, screenshots, scripts]
triggers: [screenshot, run script, applescript, clipboard, notification, local mac action, approval]
---

# macOS Sensitive Actions

## Use When

- The user asks to take a screenshot.
- The user asks to run shell or AppleScript through the MacOS helper.
- The user asks to read or write clipboard contents.
- The user asks to show a local notification.
- A local macOS action may expose private data or change user state.

## Inputs

- Requested action and exact text/script/output path.
- User approval status for sensitive actions.
- Expected result and any safe verification command.

## Method

1. Classify the action:
   - Low impact: `notify`, `clipboard_set` when the text is supplied by the user.
   - Privacy-sensitive: `clipboard_get`, `screenshot`.
   - State-changing/high impact: `run_script`, `defaults_set`, app restarts, file changes.
2. Prefer narrow helper tools over `run_script`.
3. For screenshots, require explicit approval and use a user-space output path.
4. For clipboard reads, avoid exposing sensitive text unless it is necessary for the task; summarize or redact when appropriate.
5. For clipboard writes, confirm the exact text or source content.
6. For `run_script`, explain what the script does, what it may read/change, and why no narrower helper fits.
7. After execution, inspect JSON output fields: `ok`, `stderr`, `stdout`, `returncode`, and action-specific fields.
8. Report failures without retrying with broader permissions unless the user approves the escalation.

## Output

- Safety classification and whether approval is required.
- Exact action performed or awaiting approval.
- Result summary and any relevant stderr/returncode.
- Redacted handling of sensitive clipboard or screenshot content when needed.

## Guardrails

- Never run scripts as a shortcut for available MacOS helper tools.
- Never take screenshots or run scripts without required approval.
- Never paste secrets, tokens, or private clipboard content into the conversation unless explicitly requested and necessary.
- Never use broad destructive commands for a local Mac helper task.
- Keep generated scripts short, bounded, and readable.
