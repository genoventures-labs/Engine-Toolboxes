---
id: macos-helper-contract-check
name: macOS Helper Contract Check
description: Review or update the macOS helper engine contract across manifest tools, Python subcommands, JSON output, and approval policy.
languages: [python, json, markdown]
agents: [reviewer, security, devops, architect]
tags: [macos, engine, contract, safety, review]
triggers: [macos helper, engine manifest, tool contract, approval policy, finder helper, macos agent]
---

# macOS Helper Contract Check

## Use When

- Adding, changing, or reviewing a tool in the macOS helper engine.
- The manifest, README, Python argparse command, or tool output may be out of sync.
- Debugging a mismatch between Switchbay tool calls and `macos_agent.py` or `finder_safe.py` behavior.
- Reviewing safety boundaries for local macOS actions.

## Inputs

- Engine manifest entry, especially tool name, command template, required parameters, and approval metadata.
- Python helper subcommands and argparse options.
- README/toolbox documentation.
- Expected JSON output fields and failure behavior.

## Method

1. Identify the boundary: Switchbay engine manifest -> command template -> Python argparse -> macOS CLI call -> JSON response.
2. Check every tool name maps to an implemented subcommand or helper script.
3. Confirm required parameters match between manifest and argparse names.
4. Verify success and failure responses include `ok`, `action`, and enough context to debug.
5. Confirm approval-gated actions remain gated: scripts, screenshots, preference writes, and other state-changing operations.
6. Check Finder path restrictions and ensure read-only Finder tools stay read-only.
7. Update README and matching toolbox skills when behavior changes.
8. Add or run focused tests/manual commands for both a happy path and a rejected/error path.

## Output

- Contract mismatches found, grouped by tool.
- Safety or approval gaps, if any.
- Exact files or docs that need updates.
- Verification commands or manual checks performed.

## Guardrails

- Do not loosen path restrictions or approval policy without a specific user-approved reason.
- Do not add shell execution paths outside the explicit `run_script` tool.
- Preserve JSON output compatibility for existing agents.
- Treat local user data, screenshots, clipboard, and preferences as sensitive.
