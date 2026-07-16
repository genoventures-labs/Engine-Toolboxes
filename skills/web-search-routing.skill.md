---
id: web-search-routing
name: Web Search Routing
description: Choose the right WebSearch engine tool for DuckDuckGo searches, single-URL scrapes, and bounded domain crawls.
engine: web-search
languages: [python]
agents: [backend, debugger, docs, devops, architect]
tags: [web, search, scrape, crawl, duckduckgo, research, switchbay-engine]
triggers: [web search, search the web, scrape url, crawl site, duckduckgo, find online, look up, fetch page, research online]
---

# Web Search Routing

## Use When

- The user wants to search the web without an API key.
- You need to fetch and read the content of a specific public URL.
- You need to explore a domain by following links from a seed page.
- You want current information not available in workspace knowledge or memory.

## Tools

| Tool | Purpose | Approval? |
|---|---|---|
| `web_search` | DuckDuckGo search — returns ranked results with titles, URLs, descriptions | No |
| `web_scrape` | Fetch one explicit URL and extract text, links, or both | No |
| `web_crawl` | Follow links from a seed URL up to 3 levels deep, same domain only | **Yes** |

## Method

1. Route by goal:
   - **Find pages on a topic** → `web_search`. Keep `count` at 10 unless more breadth is needed (max 20).
   - **Read a known URL** → `web_scrape`. Use `extract: "text"` for content, `extract: "links"` to map a page, `extract: "all"` for both.
   - **Map a site or doc structure** → `web_crawl`. Requires approval. Start with `depth: 1` and `max_pages: 5` — increase only if needed.

2. For `web_search`, read result fields:

| Field | Meaning |
|---|---|
| `results[].title` | Page title |
| `results[].url` | Page URL |
| `results[].description` | Snippet from DuckDuckGo |
| `results[].rank` | Position in result set |

3. After `web_search`, use `web_scrape` on the most relevant result URL to get full content — don't guess at content from snippets alone.
4. For `web_crawl`, set `delay` to at least `0.5` to avoid rate-limiting. Never crawl beyond `depth: 3` or `max_pages: 20`.
5. Always cite the source URL when presenting scraped or crawled content.

## Output

- Search: ranked list of titles, URLs, and snippets.
- Scrape: readable page text or link list with source URL cited.
- Crawl: page-by-page summary with URLs and depth reached.
- Error or empty result with a suggested fallback query or URL if a call fails.

## Guardrails

- Do not crawl private, internal, or authenticated URLs.
- Do not present scraped content as verified fact — always cite the source and note the retrieval date.
- Do not increase `max_pages` beyond 20 or `depth` beyond 3 — the engine enforces these limits, but don't try to work around them.
- Always get approval before calling `web_crawl`.
- If DuckDuckGo returns no results, try a rephrased query before concluding the topic has no coverage.
