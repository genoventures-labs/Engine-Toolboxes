---
id: macos-preferences-check
name: macOS Preferences Check
description: Safely inspect and intentionally update macOS user defaults with the MacOS helper engine.
languages: [any]
agents: [devops, debugger, reviewer]
tags: [macos, defaults, preferences, configuration]
triggers: [defaults, macos preferences, finder setting, system preference, read preference, write preference]
---

# macOS Preferences Check

## Use When

- The user asks to inspect a macOS preference value.
- A macOS issue may be caused by a user defaults setting.
- The user explicitly asks to update a macOS defaults domain/key.

## Inputs

- Defaults domain, such as `com.apple.finder`.
- Defaults key to read or write.
- Desired value and type expectations when writing.
- Whether an app restart, relaunch, or `killall` would be needed after a change.

## Method

1. Prefer `defaults_get` first to record the current value.
2. Verify the domain and key are specific; do not guess when the setting is ambiguous.
3. Explain the impact before writing any preference.
4. Request approval before `defaults_set`, since it changes user configuration.
5. Write only the requested key/value; avoid bundled preference changes.
6. After writing, verify with `defaults_get` when practical.
7. Mention if the affected app may need to be relaunched, but do not kill or restart apps unless the user explicitly asks and approves.

## Output

- Domain, key, and current value for reads.
- For writes: previous value, requested value, and verification result.
- Any follow-up needed for the setting to take effect.

## Guardrails

- Do not change system-wide or privileged preferences from this workflow.
- Do not run `killall`, restart, logout, or reboot without explicit user approval.
- Do not infer hidden preference keys as facts; cite uncertainty when relying on convention.
- Keep changes reversible by reading the old value first whenever possible.
