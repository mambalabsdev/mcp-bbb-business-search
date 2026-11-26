# BBB Business Search MCP Server

[![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-bbb-business-search/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-bbb-business-search) [![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-bbb-business-search%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-bbb-business-search&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-bbb-business-search)](https://www.npmjs.com/package/@mambalabsdev/mcp-bbb-business-search) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-bbb-business-search)](https://www.npmjs.com/package/@mambalabsdev/mcp-bbb-business-search) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-bbb-business-search)](https://github.com/mambalabsdev/mcp-bbb-business-search/blob/main/LICENSE)

An MCP server for the Mamba Labs BBB Business Search actor on Apify. BBB business listings as rows.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Takes business types or names and places and returns one row per business from the BBB directory with its BBB rating, accreditation, category, phone, address, and profile link. Filter by accreditation or minimum rating.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-bbb-business-search": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-bbb-business-search"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `search_bbb_businesses` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "Find 20 BBB accredited plumbers in Austin, TX with an A+ rating and list their phone numbers."
- "List roofing companies in Denver, CO from BBB with their BBB rating."

## Tool

`search_bbb_businesses`. One tool. Example arguments:

```json
{
  "search_terms": [
    "plumber"
  ],
  "locations": [
    "Austin, TX"
  ],
  "max_results_per_search": 10
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `search_terms` | array | no | What to search, for example plumber, roofing, or a business name. A term can hold commas. In Clay, put the column inside a one item list. |
| `locations` | array | no | City and state or province code (Austin, TX or Toronto, ON), or a ZIP or postal code. Each term is searched in each place. Leave empty to search the whole BBB directory. A place BBB does not recognize returns a free row. |
| `max_results_per_search` | number | no | Most businesses to return for each term and place, 1 to 225. BBB serves at most 225 per search. Default 30. |
| `accredited_only` | boolean | no | true returns only BBB Accredited Businesses. Others are skipped and not charged. Default false. |
| `min_rating` | string | no | Lowest BBB letter grade to return, for example A or B+. Businesses below it or not rated are skipped and not charged. Leave empty for any rating. |
| `sort` | string | no | relevance (default, BBB's best match order), rating, distance, or a_to_z. |
| `dedupe` | boolean | no | true (default) returns a business once per run even when two searches find it. The repeat is not charged. |
| `max_charge_usd` | number | no | Optional. The run stops before a charge that would pass this amount. Summary rows are always returned. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `business-row`, one business returned | $0.003 | $0.00285 | $0.0027 | $0.00255 |
| `apify-actor-start`, per run start | $0.006 | $0.006 | $0.006 | $0.006 |

Businesses held back by a filter, repeats, search summaries, and refused input are free. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/bbb-business-search](https://apify.com/mambalabs/bbb-business-search)

## License

MIT. See [LICENSE](LICENSE).
