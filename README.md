# Open Tender and RFP Finder MCP Server

[![LightNow capabilities](https://lightnow.ai/badge/com.mambabuilt/mcp-open-tender-rfp-finder)](https://lightnow.ai/servers/com.mambabuilt/mcp-open-tender-rfp-finder) [![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-open-tender-rfp-finder/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-open-tender-rfp-finder) [![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-open-tender-rfp-finder%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-open-tender-rfp-finder&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-open-tender-rfp-finder)](https://www.npmjs.com/package/@mambalabsdev/mcp-open-tender-rfp-finder) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-open-tender-rfp-finder)](https://www.npmjs.com/package/@mambalabsdev/mcp-open-tender-rfp-finder) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-open-tender-rfp-finder)](https://github.com/mambalabsdev/mcp-open-tender-rfp-finder/blob/main/LICENSE) [![mcpservers.org](https://img.shields.io/badge/mcpservers.org-listed-blue)](https://mcpservers.org/servers/mambalabsdev/mcp-open-tender-rfp-finder)

An MCP server for the Mamba Labs Open Tender and RFP Finder actor on Apify. Give it keywords, CPV codes, or buyer countries. Each run reads four official public notice portals and returns one row per open tender or RFP notice.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Your keys](#your-keys)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

The actor reads UK Contracts Finder, UK Find a Tender, EU TED (Tenders Electronic Daily), and the CanadaBuys open tender file, all official open data with no key. It keeps the notices that match your keywords and filters and returns one flat row per notice: buyer, buyer country, publication day, response deadline, days to deadline, estimated value and currency, CPV or UNSPSC codes, contract type, procedure, the buyer contact email the notice publishes, and a link. Filtered notices are never billed, and when nothing matches one free row says why.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-open-tender-rfp-finder": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-open-tender-rfp-finder"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `find_open_tenders` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "Find open UK and Irish public tenders for software published this week."
- "Which German and Dutch public bodies are buying IT services worth more than 100,000 EUR right now?"
- "List open CanadaBuys RFPs for marketing services with their deadlines."

## Tool

`find_open_tenders`. One tool. Example arguments:

```json
{
  "keywords": [
    "software"
  ],
  "countries": [
    "GB"
  ],
  "published_within_days": "7"
}
```

## Inputs

Every buyer facing input of the actor, unchanged. Omitted inputs take the actor's own defaults. Numbers and booleans are sent as strings.

| Input | Type | Description |
|---|---|---|
| `keywords` | array | Words or phrases to find in the notice title or description, any case. A notice matches any of them. EU TED matches over the full notice text in every language. Empty: every open notice in the window. Up to 25. |
| `exclude_keywords` | array | Drop a notice whose title or description contains any of these. |
| `cpv_prefixes` | array | Keep notices with a CPV code that starts with one of these, for example 72 or 48000000. CanadaBuys uses UNSPSC codes, so a CPV filter leaves it out. |
| `countries` | array | Two letter buyer country codes, for example GB, DE, CA. A portal that covers none of them is skipped. |
| `portals` | array | Portals to read. Default all four. |
| `published_within_days` | string | Publication window in days, 1 to 90, today included. Default 14. |
| `only_open` | string | Drop notices whose deadline has passed. Default true. |
| `min_value` | string | Drop notices valued below this number, in the notice currency. Notices with no value are kept. |
| `max_notices` | string | Notice rows per run, newest first, up to 5000. Each notice row is billed. Default 100. |
| `only_new` | string | Skip notices an earlier run with the same state_key returned. Default false. |
| `state_key` | string | Names the seen list for only_new. |
| `max_charge_usd` | string | Optional. The run stops before any row that would take its charges over this amount. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `notice-row`, per notice row | $0.003 | $0.00285 | $0.0027 | $0.00255 |
| `apify-actor-start`, per run start | $0.002 | $0.002 | $0.002 | $0.002 |

The no results row and notices skipped by `only_new` are free.

## Your keys

The actor needs no key. Every portal is an official open data API or file, read directly, and the actor holds no key of any kind. Mamba Labs pays the platform and proxy costs of the run.

## Full actor documentation

[apify.com/mambalabs/open-tender-rfp-finder](https://apify.com/mambalabs/open-tender-rfp-finder)

## License

MIT. See [LICENSE](LICENSE).
