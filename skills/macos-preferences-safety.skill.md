---
id: macos-preferences-safety
name: macOS Preferences Safety
description: Read and change macOS defaults preferences with verification, approvals, and rollback awareness.
languages: [shell, applescript, python, any]
agents: [devops, debugger, reviewer, any]
tags: [macos, defaults, preferences, safety]
triggers: [defaults, mac preferences, finder settings, write preference, read preference, plist]
---

# macOS Preferences Safety

## Use When

- The user asks to inspect a macOS preference value.
- The user asks to change a setting backed by `defaults`.
- A task references a defaults domain/key such as `com.apple.finder`.
- You need to verify a settings change without guessing from UI state.

## Inputs

- Defaults domain.
- Defaults key.
- Desired value and expected type, when writing.
- Whether the target app needs restart/reload after a change.

## Method

1. Separate read-only inspection from writes.
2. For read-only checks, use `defaults_get` with the exact domain and key.
3. Before a write, capture the current value with `defaults_get` when possible.
4. Ask for or confirm approval before using `defaults_set`.
5. Write the smallest requested value only; avoid unrelated preference changes.
6. Verify the new value with `defaults_get` after writing.
7. If the changed app needs a reload, explain that separately and request approval for any restart/kill action.
8. Keep a rollback note with the previous value or state if it existed.

## Output

- Domain/key inspected or changed.
- Previous value when available.
- New value and verification result.
- Any follow-up needed, such as relaunching Finder or the app.

## Guardrails

- Do not write defaults without explicit approval.
- Do not batch unrelated settings changes.
- Do not run `killall`, logout, restart, or similar commands unless the user explicitly approves.
- Do not invent preference keys; verify domain/key names from project docs, user input, or a read command.
- Treat type ambiguity as a reason to ask or use a safer read-first check.
