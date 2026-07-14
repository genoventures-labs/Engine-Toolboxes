---
id: gumroad-health-check
name: Gumroad API Health Check
description: Diagnose Gumroad API connectivity, token presence, and endpoint reachability using the GumOps health check tool.
engine: gumops
languages: [python]
agents: [debugger, backend, devops]
tags: [gumroad, health, connectivity, token, api, diagnostics]
triggers: [gumroad connected, api health, check api, missing token, token not set, connection error, api down, ping gumroad, gumroad unreachable]
---

# Gumroad API Health Check

## Use When

- The user asks if the Gumroad API is connected or working.
- A GumOps tool returns an auth error or connection failure.
- `GUMROAD_ACCESS_TOKEN` may be missing, empty, or invalid.
- You want to confirm API reachability before running analytics or refund tools.

## Tool

`gum_health_check` — no parameters required.

## Method

1. Run `gum_health_check` immediately. Do not call any other GumOps tool first.
2. Read the response fields:

| Field | Meaning |
|---|---|
| `token_present` | Whether a token was found in the environment |
| `token_prefix` | First 6 chars of the token (for confirmation, never log the full token) |
| `endpoints.user.ok` | Whether `/user` responded successfully |
| `endpoints.products.ok` | Whether `/products` responded successfully |
| `endpoints.*.latency_ms` | Round-trip time per endpoint |
| `healthy` | `true` only if all endpoints returned 2xx |
| `errors` | List of human-readable failure messages |

3. Diagnose based on the result:

| Symptom | Likely Cause | Action |
|---|---|---|
| `token_present: false` | `GUMROAD_ACCESS_TOKEN` not set | Ask the user to set it in their environment |
| `token_present: true`, `user.ok: false`, status 401 | Token invalid or expired | Ask the user to regenerate the token in Gumroad settings |
| `token_present: true`, all endpoints fail | Network or API outage | Check internet connection; retry in 60s |
| `token_present: true`, `user.ok: true`, `products.ok: false` | Permissions or account issue | Report partial connectivity; check Gumroad account status |
| `healthy: true` | All clear | Proceed to any other GumOps tool |

4. Report findings concisely — one sentence per check. Do not dump raw JSON unless asked.
5. If `healthy: false`, stop and surface the fix before using any other GumOps tool.

## Output

- Connection status: healthy / degraded / no token.
- Seller name and email (from `/user`) when available.
- Product count (from `/products`) when available.
- Latency per endpoint.
- Clear next step if unhealthy.

## Guardrails

- Never log or echo the full access token. Only report `token_prefix`.
- Do not run sales, refund, or analytics tools if `healthy: false`.
- If the token is missing, direct the user to their environment config — never hardcode or suggest a token value.
