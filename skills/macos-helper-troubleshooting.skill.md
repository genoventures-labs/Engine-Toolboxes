---
id: macos-helper-troubleshooting
name: macOS Helper Troubleshooting
description: Diagnose failures in the macOS helper engine with small checks across installation, command availability, permissions, paths, and JSON output.
languages: [python, shell, json]
agents: [debugger, devops, reviewer]
tags: [macos, troubleshooting, engine, diagnostics]
triggers: [macos helper failing, finder helper failing, open failed, defaults failed, screenshot failed, clipboard failed]
---

# macOS Helper Troubleshooting

## Use When

- A macOS helper tool returns `ok: false`, non-zero return code, empty output, or unexpected JSON.
- A local macOS command such as `open`, `defaults`, `osascript`, `pbcopy`, `pbpaste`, or `screencapture` fails.
- Finder helper path checks reject a path unexpectedly.
- The engine appears unavailable or misconfigured.

## Inputs

- Tool name and arguments used.
- JSON output, stderr, return code, and current working directory.
- Target path, defaults domain/key, or macOS command involved.
- Recent changes to manifest, helper scripts, or macOS permissions.

## Method

1. Restate the failing helper action and expected outcome.
2. Run `status` or inspect the script path first to confirm the helper is reachable.
3. Reproduce with the smallest safe command or read-only tool.
4. For Finder failures, check path existence, expansion, and allowed roots before changing code.
5. For permission-sensitive tools, consider macOS privacy/TCC prompts and app permissions.
6. For argparse errors, compare manifest command templates with Python subcommand options.
7. For JSON issues, confirm the helper emits one JSON object and no extra text.
8. Patch only the smallest confirmed mismatch, then rerun the focused helper command.

## Output

- Most likely cause with supporting evidence.
- Minimal fix or user action needed.
- Focused verification command and result.
- Any unresolved permission or environment caveat.

## Guardrails

- Do not jump to broad system changes before a small reproduction.
- Do not request destructive cleanup, resets, or permission changes unless clearly necessary.
- Do not run screenshots or scripts during diagnosis without approval.
- Keep troubleshooting within the local helper and current workspace unless the user asks to inspect broader system state.
