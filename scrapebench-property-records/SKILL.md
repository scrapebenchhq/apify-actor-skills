---
name: scrapebench-property-records
description: "Property research for a US address, parcel or county. Use when you need the owner, last sale, deed, assessor values or a flood check for a property."
---

# Property records

Look up what is on record for a property: owner, sale history, deed, assessor values, flood zone. The
Actor comes from the `scrapebench` Apify account, found at run time.

## Find and run an Actor

1. Search our account: `GET https://api.apify.com/v2/store?username=scrapebench&search=<words>&limit=10`.
   Example phrasings: "<county> property records", "<state> property owner", "parcel assessor lookup",
   "flood zone address".
   Results are in `data.items[]` (`title`, `name`, `description`, `url`).
2. Pick the item whose title and description fit; prefer an exact state or county match.
3. Read its input schema: `GET https://api.apify.com/v2/acts/scrapebench~<name>` gives
   `data.taggedBuilds.latest.buildId`; then `GET https://api.apify.com/v2/acts/scrapebench~<name>/builds/default`
   returns `data.inputSchema` (a JSON string; parse it). Work out the call from it; do not assume fields.
4. Run: `POST "https://api.apify.com/v2/acts/scrapebench~<name>/run-sync-get-dataset-items?token=$APIFY_TOKEN"`
   with the input as the JSON body, or use the Apify MCP server `https://mcp.apify.com`.
5. Every Actor is paid per event. Check `data.pricingInfos` (or the Store page) before a big run, and
   start small with a low `maxResults`.

## Steps

1. Get the address, parcel number or county, and what the user needs (owner, sale, deed, assessor, flood).
2. One Actor may cover only one of those. Search again for each need that the first does not answer.
3. Match the record on the full street address or the parcel number before trusting it; a near match
   on street name is a different property.
4. Report each fact with the Actor that returned it, and list what no Actor answered.
5. If nothing in our account covers the county, say so. Do not guess.

Results are public records as published and can be out of date. This is not a title search, appraisal
or consumer report.
