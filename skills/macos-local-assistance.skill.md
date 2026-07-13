---
id: macos-local-assistance
name: macOS Local Assistance
description: Use the macOS helper engine for bounded local Mac tasks such as opening targets, notifications, clipboard, screenshots, and scripted checks.
languages: [any]
agents: [devops, debugger, docs, reviewer]
tags: [macos, local, automation, switchbay-engine]
triggers: [macos, mac, open app, clipboard, notification, screenshot, run script, local helper]
---

# macOS Local Assistance

## Use When

- The user asks for a local macOS action through the MacOS helper engine.
- The task involves opening an app, file, folder, or URL.
- The workflow needs clipboard read/write, a local notification, or a screenshot.
- A bounded shell or AppleScript snippet would answer a macOS-specific question.

## Inputs

- User goal and target app, file, folder, URL, or text.
- Whether the action reads context or changes local state.
- Destination path for artifacts such as screenshots.
- Any approval requirement surfaced by the engine manifest.

## Method

1. Identify the exact helper tool needed: `status`, `query`, `open_app`, `run_script`, `notify`, `clipboard_get`, `clipboard_set`, or `screenshot`.
2. Normalize targets before calling tools: expand user-facing paths, keep URLs explicit, and preserve quoted text exactly.
3. Prefer read-only checks first when the user is diagnosing or asking what is possible.
4. Request approval before gated actions: `run_script` and `screenshot`.
5. For scripts, keep the snippet short, bounded, and specific to the requested task.
6. Parse JSON output and report only the result the user needs.

## Output

- State the action completed or the reason it failed.
- Include target paths, return codes, or stderr only when useful.
- For screenshots or generated artifacts, provide the saved path.
- For clipboard actions, avoid echoing sensitive clipboard contents unless the user asked to inspect them.

## Guardrails

- Do not use `run_script` for broad filesystem scans, destructive commands, privileged commands, or long-running automation without explicit approval and a clear reason.
- Do not capture screenshots unless the user requested it and approval is granted.
- Do not expose clipboard contents unnecessarily.
- Do not imply the helper can bypass macOS permissions, TCC prompts, or app sandbox limits.
- Keep actions local to the user's Mac and current workspace unless the user explicitly asks otherwise.
