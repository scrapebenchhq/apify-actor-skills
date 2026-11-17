# Apify actor skills

Agent Skills (`SKILL.md`) that find and run the right Apify Actor from the
[`scrapebench`](https://apify.com/scrapebench) account, searching the Store at run time. Every Actor is paid per event.

## Install

```bash
git clone https://github.com/scraper111/apify-actor-skills.git
cp -r apify-actor-skills/scrapebench-actor-finder ~/.claude/skills/
```

## Use

Set `APIFY_TOKEN` (`export APIFY_TOKEN=...`) or connect the Apify MCP server (`https://mcp.apify.com`),
then ask in plain words.

- `scrapebench-contractor-vetting`: licence, business standing and complaints, then a verdict. "Vet ABC Plumbing LLC in Phoenix, Arizona before I hire them."
- `scrapebench-business-lookup`: does a company exist, status, formation, agent and officers. "Is Acme Holdings LLC active in Delaware?"
- `scrapebench-b2b-leads`: leads by trade or category and area. "Find 50 roofers in Harris County with phone numbers."
- `scrapebench-property-records`: owner, sale, deed, assessor and flood checks. "Who owns 12 Main St, Austin, Texas?"
- `scrapebench-actor-finder`: any data task; uses an Actor if one fits, otherwise says none does. "Is there an Actor for Ohio liquor licences?"
