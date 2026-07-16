---
id: domain-tools-routing
name: Domain Tools Routing
description: Validate domain names and TLDs using the DomainTools engine. Choose the right tool for domain format checks vs. TLD recognition.
engine: domaintools
languages: [python]
agents: [backend, architect, debugger, docs]
tags: [domain, tld, dns, validation, switchbay-engine]
triggers: [domain, tld, check domain, valid domain, domain name, check tld, domain available, .com, .io, .dev]
---

# Domain Tools Routing

## Use When

- The user wants to validate a domain name format.
- The user wants to know if a TLD (e.g. `.io`, `.dev`, `.xyz`) is recognized.
- A workflow requires confirmed domain format before using it as an input elsewhere.
- You need to pre-check a domain before passing it to DNS, web, or email tools.

## Tools

| Tool | Purpose |
|---|---|
| `check_domain` | Validate domain format and confirm the TLD is recognized |
| `check_tld` | Isolate just the TLD check for a given domain |

## Method

1. If the user provides a bare TLD (e.g. `.io`) — wrap it into a dummy domain (`example.io`) and use `check_tld`.
2. If the user provides a full domain (e.g. `mybrand.dev`) — use `check_domain` first.
3. Read the JSON response fields:

| Field | Meaning |
|---|---|
| `domain` | The normalized domain that was checked |
| `valid_format` | Whether the domain passes format validation |
| `tld` | The extracted TLD |
| `tld_recognized` | Whether the TLD is in the known list |
| `available` | Availability status — `null` means lookup not implemented |
| `error` | Present if the input was rejected before checks ran |

4. If `valid_format: false` — report the format issue and ask the user to correct the input.
5. If `tld_recognized: false` — report the TLD as unrecognized. Do not claim it is invalid globally; the list may not cover every ccTLD.
6. If `available: null` — note that live availability lookup is not implemented; suggest WHOIS or a registrar check.
7. Do not invent availability status. Never say a domain is "available" or "taken" based on format checks alone.

## Output

- Format validity verdict.
- TLD recognition result.
- Honest note about availability if asked.
- Suggested next step (WHOIS, registrar, or DNS lookup) when relevant.

## Guardrails

- Do not treat `tld_recognized: false` as proof the domain is invalid — TLD lists are never exhaustive.
- Do not claim availability from this engine; it does not perform live WHOIS or DNS lookups.
- Always pass the full domain string (e.g. `example.com`), not a bare TLD or path.
