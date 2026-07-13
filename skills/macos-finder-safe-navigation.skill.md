---
id: macos-finder-safe-navigation
name: macOS Finder Safe Navigation
description: Use the safe Finder helper to reveal, open, or inspect user-space files and folders without renaming, moving, or deleting anything.
languages: [any]
agents: [devops, debugger, docs, reviewer]
tags: [macos, finder, filesystem, safe-navigation]
triggers: [finder, reveal file, open folder, file info, show in finder, reveal parent]
---

# macOS Finder Safe Navigation

## Use When

- The user asks to show, reveal, open, or inspect a file or folder in Finder.
- The task needs Finder context but should not modify files.
- The user wants metadata for a local path before opening or revealing it.

## Inputs

- A user-space path under `~/`, `/Users/Shared`, or the repository working directory.
- Desired Finder action: `finder_show`, `finder_open`, `finder_info`, or `finder_reveal_parent`.
- Any relevant expectation such as revealing the item itself versus opening its containing folder.

## Method

1. Confirm the path is specific enough to act on.
2. Choose the least invasive helper tool:
   - `finder_info` for existence and metadata checks.
   - `finder_show` to reveal a file or open a directory in Finder.
   - `finder_open` to open a file or folder with the default macOS handler.
   - `finder_reveal_parent` to open the containing folder.
3. Keep operations read-only; do not rename, move, delete, duplicate, or edit files through this workflow.
4. If the helper rejects a path as outside allowed roots, stop and explain the allowed roots.
5. Report the Finder action and any path-related error clearly.

## Output

- Action taken and normalized path.
- For metadata, include existence, type, size, and modified time when relevant.
- For failures, include whether the path was missing, outside allowed roots, or rejected by macOS.

## Guardrails

- Never use Finder navigation as a substitute for destructive file operations.
- Do not attempt to bypass the helper's allowed-root policy.
- Do not reveal sensitive locations unless the user explicitly names them.
- Prefer `finder_info` before opening unknown or ambiguous paths.
