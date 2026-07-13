---
id: macos-helper-routing
name: macOS Helper Routing
description: Choose the safest MacOS helper tool for local app, clipboard, notification, screenshot, preference, and Finder tasks.
languages: [python, shell, applescript, any]
agents: [devops, debugger, docs, any]
tags: [macos, local-tools, switchbay-engine, workflow]
triggers: [macos, mac, finder, clipboard, screenshot, notification, defaults, open app, local helper]
---

# macOS Helper Routing

## Use When

- The user asks for help with local macOS actions through the MacOS Agent engine.
- A task involves opening apps, files, folders, or URLs.
- A task involves clipboard text, notifications, screenshots, Finder reveal/open/info, or macOS `defaults`.
- You need to decide whether a direct shell command or a safer MacOS helper tool is the right boundary.

## Inputs

- User goal and target path, app name, URL, defaults domain/key, clipboard text, or screenshot destination.
- Current workspace and whether the target is inside user-space.
- Any approval requirement from the engine manifest.

## Method

1. Identify the intended macOS action and the least-powerful tool that satisfies it.
2. Prefer read-only helper tools before broader script execution.
3. Route common actions:
   - Health check: `status`.
   - General local context question: `query`.
   - Open app, file, folder, or URL: `open_app`.
   - Finder reveal/open/info/parent reveal: `finder_show`, `finder_open`, `finder_info`, `finder_reveal_parent`.
   - Read a preference: `defaults_get`.
   - Write a preference: `defaults_set` with approval.
   - Read or write clipboard text: `clipboard_get` / `clipboard_set`.
   - Show a notification: `notify`.
   - Capture screen: `screenshot` with approval.
   - Run shell or AppleScript only when no narrower helper exists: `run_script` with approval.
4. Normalize paths before passing them to Finder tools; keep them in `~/`, `/Users/Shared`, or repo CWD when possible.
5. Inspect JSON output for `ok`, `action`, `stderr`, and `returncode` before reporting success.
6. If a helper returns an error, report the exact boundary that failed and suggest the next safest check.

## Output

- Tool selected and why, when useful.
- Result summary using the helper's JSON fields.
- Any approval required before state-changing or sensitive actions.
- Next safest fallback if the requested action cannot be completed.

## Guardrails

- Do not use `run_script` for actions already covered by narrower helper tools.
- Do not write `defaults`, take screenshots, or run scripts without explicit approval when required.
- Do not claim a GUI action succeeded unless the helper returned `ok: true`.
- Do not operate on system paths or private app containers unless the user explicitly asks and the tool policy allows it.
