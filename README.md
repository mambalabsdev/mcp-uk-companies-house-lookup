# UK Companies House Lookup MCP Server

[![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-uk-companies-house-lookup%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-uk-companies-house-lookup&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-uk-companies-house-lookup)](https://www.npmjs.com/package/@mambalabsdev/mcp-uk-companies-house-lookup) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-uk-companies-house-lookup)](https://www.npmjs.com/package/@mambalabsdev/mcp-uk-companies-house-lookup) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-uk-companies-house-lookup)](https://github.com/mambalabsdev/mcp-uk-companies-house-lookup/blob/main/LICENSE)

An MCP server for the Mamba Labs UK Companies House Lookup actor on Apify. UK company register data as rows.

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

Takes Companies House company numbers or company names and returns one row per company with status, type, dates, registered office, SIC codes, and accounts and confirmation statement dates. Officers and filing history add one row per appointment and per filing.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-uk-companies-house-lookup": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-uk-companies-house-lookup"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `lookup_uk_companies` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "Look up Companies House number 00445790 and list its current directors."
- "Find the UK company called Monzo Bank on Companies House and show its SIC codes and last accounts date."

## Tool

`lookup_uk_companies`. One tool. Example arguments:

```json
{
  "company_numbers": [
    "00445790"
  ],
  "include_officers": true,
  "current_officers_only": true,
  "max_officers_per_company": 10
}
```

## Inputs

Every buyer facing input of the actor. Omitted inputs take the actor's own defaults.

| Input | Type | Required | Description |
|---|---|---|---|
| `company_numbers` | array | no | Companies House company numbers (for example 00445790 or SC123456) or company page URLs. A number shorter than 8 digits is padded with zeros. In Clay, put the column inside a one item list. |
| `search_terms` | array | no | Company names to search on the register. Each search returns up to the number of companies set below, in Companies House's own order. A name can hold commas. |
| `max_companies_per_search` | number | no | Companies to read for each company name, 1 to 100. Default 5. |
| `name_match` | string | no | words (default): read only companies whose current or previous name holds every word you searched, closest first (legal form words such as LTD are ignored). any: read the register's results in its own order, including companies that share only some words. |
| `include_officers` | boolean | no | true adds one row per officer appointment (directors, secretaries, LLP members), charged per officer. Default false. |
| `current_officers_only` | boolean | no | true returns only officers still in post. Default false, which also returns resigned officers. |
| `max_officers_per_company` | number | no | Most officer rows per company, 1 to 1000. Default 50. |
| `include_filings` | boolean | no | true adds one row per filing history entry, newest first, charged per filing. Default false. |
| `max_filings_per_company` | number | no | Most filing rows per company, 1 to 1000. Default 25. |
| `dedupe` | boolean | no | true (default) reads a company once per run even when two inputs lead to it. The repeat is not charged. |
| `max_charge_usd` | number | no | Optional. The run stops before a charge that would pass this amount. Summary rows are always returned. |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `company-row`, one company read from the register | $0.004 | $0.0038 | $0.0036 | $0.0034 |
| `officer-row`, one officer appointment | $0.002 | $0.0019 | $0.0018 | $0.0017 |
| `filing-row`, one filing history entry | $0.001 | $0.00095 | $0.0009 | $0.00085 |
| `apify-actor-start`, per run start | $0.003 | $0.003 | $0.003 | $0.003 |

Repeats, numbers not found, search summaries, and refused input are free. Mamba Labs pays the platform costs of the run, Apify proxy traffic included.

## Full actor documentation

[apify.com/mambalabs/uk-companies-house-lookup](https://apify.com/mambalabs/uk-companies-house-lookup)

## License

MIT. See [LICENSE](LICENSE).
