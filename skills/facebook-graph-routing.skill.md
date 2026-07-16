---
id: facebook-graph-routing
name: Facebook Graph Routing
description: Choose the right Facebook Graph API tool for object lookups, edge traversal, and Page insights.
engine: facebook
languages: [python]
agents: [backend, architect, debugger, docs]
tags: [facebook, graph-api, social, insights, pages, switchbay-engine]
triggers: [facebook, graph api, page insights, fb page, facebook object, facebook edge, impressions, engagement, facebook data]
---

# Facebook Graph Routing

## Use When

- The user wants to fetch a Facebook object (user, page, post, etc.) by ID.
- The user wants to traverse a Graph API edge (feed, photos, posts, etc.) from a node.
- The user wants Page-level insights: impressions, engagement, reach.
- You need to confirm a Facebook Graph API object exists before passing its ID elsewhere.

## Tools

| Tool | Purpose |
|---|---|
| `get_object` | Fetch a single Graph API node by ID with selected fields |
| `get_edge` | Fetch a named edge from a parent node (e.g. a Page's posts or photos) |
| `get_page_insights` | Fetch aggregated Page metrics for a specific period |

## Prerequisites

- `FACEBOOK_ACCESS_TOKEN` must be set in the environment before any tool call.
- Page insights require a **Page access token**, not a user token.
- Edge access depends on the token's granted permissions.

## Method

1. Confirm `FACEBOOK_ACCESS_TOKEN` is present in the environment. Do not proceed if it is missing — surface the missing token error immediately.
2. Route by goal:
   - **Fetch a specific object** → `get_object` with `object_id` and the needed `fields` (comma-separated).
   - **Fetch related content** → `get_edge` with `node_id` (the parent) and `edge` name.
   - **Fetch Page metrics** → `get_page_insights` with `page_id`, `metrics`, and `period`.
3. Default field sets by object type:

| Object Type | Suggested Fields |
|---|---|
| User / Me | `id,name,email` |
| Page | `id,name,fan_count,about` |
| Post | `id,message,created_time,story` |

4. Default insight metrics: `page_impressions,page_engaged_users`. Default period: `day`.
5. Read the JSON response. If it contains an `error` key, report the `message` and `code` fields — do not re-try silently.
6. For paginated edges, note the `paging.next` cursor if present and offer to fetch the next page.

## Output

- Object data or edge items as a readable summary (not raw JSON unless asked).
- Metric values with period label for insights.
- Token or permission error with clear next step if the call fails.

## Guardrails

- Never hardcode or log the access token value.
- Do not pass user tokens to Page insight calls — they will fail with a permissions error.
- Do not assume an object ID is valid without a successful `get_object` response.
- Do not retry a failed call more than once without user confirmation.
