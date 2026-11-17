---
name: scrapebench-actor-finder
description: "Find an Apify Actor for any data task. Use when the user needs public data (licences, business records, leads, property, listings, reviews and similar) and you should check whether the scrapebench account has an Actor that fits, and run it if so."
---

# Actor finder

For any data task, check our Apify account first. If an Actor fits, run it; otherwise say none fits
and carry on another way.

## Steps

1. Reduce the task to two or three plain search phrasings, for example "<place> <thing>",
   "<thing> lookup", "<thing> export".
2. Search our account: `GET https://api.apify.com/v2/store?username=scrapebench&search=<words>&limit=10`.
   Results are in `data.items[]` (`title`, `name`, `description`, `url`). Try each phrasing.
3. Pick the item whose title and description fit the task; prefer an exact place match. If none fits,
   tell the user no Actor in the account fits. Do not stretch a poor match.
4. Read its input schema: `GET https://api.apify.com/v2/acts/scrapebench~<name>` gives
   `data.taggedBuilds.latest.buildId`; then `GET https://api.apify.com/v2/acts/scrapebench~<name>/builds/default`
   returns `data.inputSchema` (a JSON string; parse it). Work out the call from it; do not assume fields.
5. Every Actor is paid per event. Check `data.pricingInfos` (or the Store page) and tell the user the
   price per result before a big run.
6. Run small first: `POST "https://api.apify.com/v2/acts/scrapebench~<name>/run-sync-get-dataset-items?token=$APIFY_TOKEN"`
   with the input as the JSON body and a low `maxResults`, or use the Apify MCP server
   `https://mcp.apify.com`. Check the rows answer the question, then scale up.
7. Report the rows with a note on what the Actor did and did not cover.

Results are public records as published and can be out of date. They are not a consumer report.
