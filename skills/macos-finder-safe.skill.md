---
id: macos-finder-safe
name: macOS Finder Safe Operations
description: Safely reveal, open, and inspect user-space files or folders through the MacOS Finder helper.
languages: [python, shell, any]
agents: [devops, debugger, docs, any]
tags: [macos, finder, files, safety]
triggers: [finder, reveal file, open folder, file info, show in finder, reveal parent]
---

# macOS Finder Safe Operations

## Use When

- The user asks to reveal a file or folder in Finder.
- The user asks to open a user-space file or folder from Finder.
- The user asks for basic file or folder metadata before opening it.
- The task can be completed without renaming, moving, deleting, or writing files.

## Inputs

- Target path, preferably absolute or `~`-relative.
- Desired Finder action: show, open, info, or reveal parent.
- Workspace context if the target path is repo-relative.

## Method

1. Resolve the target path from the user's wording and current workspace.
2. Confirm the intended operation is read-only or Finder-open only.
3. Choose the specific helper:
   - `finder_info` to verify existence and inspect metadata.
   - `finder_show` to reveal a file or open a folder in Finder.
   - `finder_open` to open a user-space file or folder.
   - `finder_reveal_parent` to open the containing folder.
4. Keep targets under allowed user-space roots: home directory, `/Users/Shared`, or repo CWD.
5. Read the JSON response and check `ok`, `error`, `path`, `parent`, and `returncode`.
6. If a path is outside allowed roots or missing, stop and ask for a safer target or clarification.

## Output

- Whether the Finder operation succeeded.
- The path or parent path acted on.
- Any helper error, such as `path not found` or `path outside allowed roots`.
- A concise next step if the operation could not be completed.

## Guardrails

- Do not delete, move, rename, chmod, or edit files as part of a Finder request.
- Do not bypass the Finder helper with raw shell for equivalent reveal/open/info operations.
- Do not reveal protected system or private app-container locations unless tool policy explicitly allows it.
- Do not infer that Finder displayed correctly if the helper reports `ok: false`.
