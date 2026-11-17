---
name: scrapebench-business-lookup
description: "Know-your-business check. Use when you need to confirm a company exists and learn its status, formation date, registered agent and officers from the state business registry."
---

# Business lookup (KYB)

Answer: does this company exist, is it in good standing, when was it formed, who is the agent and who
are the officers. The Actor comes from the `scrapebench` Apify account, found at run time.

## Find and run an Actor

1. Search our account: `GET https://api.apify.com/v2/store?username=scrapebench&search=<words>&limit=10`.
   Example phrasings: "<state> business entity", "<state> secretary of state business search",
   "<state> company lookup", "business entity search".
   Results are in `data.items[]` (`title`, `name`, `description`, `url`).
2. Pick the item whose title and description fit; prefer an exact state match. If none is state-specific,
   look for one that covers several states.
3. Read its input schema: `GET https://api.apify.com/v2/acts/scrapebench~<name>` gives
   `data.taggedBuilds.latest.buildId`; then `GET https://api.apify.com/v2/acts/scrapebench~<name>/builds/default`
   returns `data.inputSchema` (a JSON string; parse it). Work out the call from it; do not assume fields.
4. Run: `POST "https://api.apify.com/v2/acts/scrapebench~<name>/run-sync-get-dataset-items?token=$APIFY_TOKEN"`
   with the input as the JSON body, or use the Apify MCP server `https://mcp.apify.com`.
5. Every Actor is paid per event. Check `data.pricingInfos` (or the Store page) before a big run, and
   start small with a low `maxResults`.

## Steps

1. Get the legal name and state (ask if missing). Search by the exact name first.
2. Run the lookup. Several entities can share a name: match on state, city and entity type.
3. Report: legal name, entity type, status (active, dissolved, delinquent, revoked), formation date,
   registered agent, officers or managers where returned, and the record link if one is given.
4. Flag: not found, inactive status, a name that only nearly matches, or an agent address that is
   shared with many entities.
5. If nothing in our account covers the state, say so. Do not guess.

Results are public records as published and can be out of date. This is not a consumer report: do not
use it for employment or credit decisions.
