---
name: scrapebench-contractor-vetting
description: "Vet a contractor before hiring, paying a deposit or insuring them. Use when you have a contractor name or licence number and a US state, and need a sourced verdict on licence status, business entity standing and complaints."
---

# Contractor vetting

Check a contractor in three steps, then give a verdict that cites each record. Each step uses an
Apify Actor from the `scrapebench` account, found at run time with the steps below.

## Find and run an Actor (used for every step)

1. Search our account: `GET https://api.apify.com/v2/store?username=scrapebench&search=<words>&limit=10`.
   Results are in `data.items[]` (`title`, `name`, `description`, `url`).
2. Pick the item whose title and description fit; prefer an exact state match.
3. Read its input schema: `GET https://api.apify.com/v2/acts/scrapebench~<name>` gives
   `data.taggedBuilds.latest.buildId`; then `GET https://api.apify.com/v2/acts/scrapebench~<name>/builds/default`
   returns `data.inputSchema` (a JSON string; parse it). Work out the call from it; do not assume fields.
4. Run: `POST "https://api.apify.com/v2/acts/scrapebench~<name>/run-sync-get-dataset-items?token=$APIFY_TOKEN"`
   with the input as the JSON body, or use the Apify MCP server `https://mcp.apify.com`.
5. Every Actor is paid per event. Check `data.pricingInfos` (or the Store page) before a big run, and
   start small with a low `maxResults`.

## 1. Licence

Search phrasings: "<state> contractor license", "<state> contractor licence lookup", "<state> construction license".
Search by licence number when you have one, otherwise by business or person name. Record the number,
status (active, expired, suspended, revoked), classifications, expiry. A name search can return
several licences: match on business name AND city before trusting one.

## 2. Business entity

Search phrasings: "<state> business entity", "<state> secretary of state business search".
Compare entity name, status (active, dissolved, delinquent), formation date and registered agent with the
licence. A licence under a name the entity record does not show is a flag.

## 3. Complaints

Search phrasings: "<state> contractor complaints", "<state> contractor discipline". If nothing fits, say
no complaint data was checked for that state. Never infer a clean record from missing data.

## 4. Verdict

Write: **Verdict** (CLEAR, CAUTION, DO NOT HIRE or INCOMPLETE if you could not match the contractor);
**Evidence** (one line per step with the record found); **Gaps** (what was not checked);
**Next step** (confirm with the issuing state agency before signing anything).

- CLEAR only if the licence is active in the right classification, the entity is in good standing, and
  no open or disciplined cases were found where complaint data exists.
- CAUTION for an expired or soon-expiring licence, a name mismatch, a delinquent entity or open cases.
- DO NOT HIRE for a suspended or revoked licence, discipline, or no licence where one is required.
- Results are public records as published and can be out of date. This is not a consumer report: do not
  use it for employment or credit decisions.
