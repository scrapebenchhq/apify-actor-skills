---
name: scrapebench-b2b-leads
description: "Build a B2B lead list. Use when you need local businesses or licensed trades in a category and area (for example roofers in Phoenix or electricians in one county), with phone numbers where available."
---

# B2B leads

Turn "I need <trade or category> in <area>" into a list of businesses with contact details. The Actor
comes from the `scrapebench` Apify account, found at run time.

## Find and run an Actor

1. Search our account: `GET https://api.apify.com/v2/store?username=scrapebench&search=<words>&limit=10`.
   Example phrasings: "<state> contractor directory leads", "<category> leads <city>",
   "business directory <country>", "<trade> contact list".
   Results are in `data.items[]` (`title`, `name`, `description`, `url`).
2. Pick the item whose title and description fit; prefer an exact state or country match.
3. Read its input schema: `GET https://api.apify.com/v2/acts/scrapebench~<name>` gives
   `data.taggedBuilds.latest.buildId`; then `GET https://api.apify.com/v2/acts/scrapebench~<name>/builds/default`
   returns `data.inputSchema` (a JSON string; parse it). Work out the call from it; do not assume fields.
4. Run: `POST "https://api.apify.com/v2/acts/scrapebench~<name>/run-sync-get-dataset-items?token=$APIFY_TOKEN"`
   with the input as the JSON body, or use the Apify MCP server `https://mcp.apify.com`.
5. Every Actor is paid per event. Check `data.pricingInfos` (or the Store page) before a big run, and
   start small with a low `maxResults`.

## Steps

1. Pin down the category (or trade), the area (state, county, city or ZIP) and how many leads are wanted.
2. Search, pick, and read the schema as above. Map category and area onto the input fields it offers.
3. Run a small sample first (a few rows). Check the fields: business name, address, phone, licence or
   category. Phone is present only where the source publishes it; say how many rows lack one.
4. If the sample fits, raise `maxResults` to what the user wants and run again.
5. Hand back a table or file. Remove duplicates by name and address.

Results are public records as published and can be out of date. Check calling and email rules for the
area before outreach.
