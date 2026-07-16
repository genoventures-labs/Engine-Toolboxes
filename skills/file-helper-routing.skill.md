---
id: file-helper-routing
name: File Helper Routing
description: Choose the right FileHelper engine tool for safe, sandboxed file reads, directory listings, in-file search, and guarded find-and-replace.
engine: file-helper
languages: [python]
agents: [backend, debugger, docs, devops]
tags: [files, filesystem, search, read, replace, sandbox, switchbay-engine]
triggers: [file helper, read file, list directory, search file, replace in file, find in file, file utility, sandbox file]
---

# File Helper Routing

## Use When

- You need to list or inspect directory contents in a sandboxed way.
- You need to read a file with a size guard to avoid loading huge files.
- You need to search for a string or pattern inside a file with context lines.
- You need to do a guarded in-place find-and-replace in a file.

## Tools

| Tool | Purpose | Mutates? |
|---|---|---|
| `list_directory` | List entries in a directory with size and type metadata | No |
| `read_file` | Read a file's full text up to a byte limit | No |
| `search_in_file` | Search a file for a string or regex with surrounding context | No |
| `replace_in_file` | Find and replace text in a file in-place | **Yes — requires approval** |

## Method

1. Always set `root` to the active workspace root when sandboxing is important. This prevents path traversal outside the project.
2. Route by goal:
   - **Browse a folder** → `list_directory`. Use `recursive: true` only for small or known-size trees.
   - **Read a config, script, or doc** → `read_file`. Use the default `max_bytes` (200 KB) unless the file is known to be large.
   - **Find a pattern** → `search_in_file`. Use `regex: true` for pattern matching; increase `context_lines` when surrounding code matters.
   - **Patch a file** → `replace_in_file`. This is destructive — confirm the search string is exact before calling. Requires explicit approval.
3. For `search_in_file`, check response fields:

| Field | Meaning |
|---|---|
| `matches` | List of matches with `line`, `text`, and `context` |
| `total` | Total match count |
| `truncated` | True if results exceeded `max_results` |

4. For `replace_in_file`, always read the file first with `read_file` or `search_in_file` to confirm the target text before replacing.
5. If `list_directory` returns an empty array, confirm the path is correct before concluding the directory is empty.

## Output

- Readable summary of directory entries, file contents, matches, or replacement result.
- Line numbers for search matches.
- Explicit confirmation before replace is executed.
- Error message and suggested fix if a tool call fails.

## Guardrails

- Do not call `replace_in_file` without reading the target content first.
- Do not set `recursive: true` on unknown root directories — it can return thousands of entries.
- Do not raise `max_bytes` above 1 MB without a clear reason.
- Do not operate outside the declared `root` — sandboxing is the primary safety boundary of this engine.
