# tukang
Local services MCP connector. Prompt a handyman via Claude and it pings them on whatsapp!
# Tukang MCP

> AI-native handyman and local services dispatch for Singapore.  
> Built on the [Model Context Protocol](https://modelcontextprotocol.io).

## What is Tukang?

Tukang is an MCP server that lets AI agents (Claude, ChatGPT, or any 
MCP-compatible client) find, match, and dispatch skilled tradespeople 
in Singapore via WhatsApp — without any app download or manual booking flow.

When a user asks their AI "find me a plumber near Toa Payoh," Tukang handles 
the entire workflow: search → match → dispatch → accept/reject → confirmation.

## MCP Server Endpoint
https://tukang.created.app/api/mcp


Protocol: MCP JSON-RPC 2024-11-05  
Transport: HTTP + SSE

## Quick Start (Claude Desktop)

Add to your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "tukang": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-remote", 
               "https://tukang.created.app/api/mcp"]
    }
  }
}
```

Restart Claude Desktop. Then try:

> "Find me an electrician near Bishan and dispatch a job for 
> a faulty power socket. Urgent."

## Tools

| Tool | Description |
|---|---|
| `search_handymen` | Search available tradespeople by location + skill |
| `dispatch_job` | Create job + send WhatsApp to matched handymen |
| `get_job_status` | Poll job status + dispatch attempt results |
| `list_jobs` | List all jobs with optional status filter |

## Architecture

- AI client calls Tukang MCP via JSON-RPC over HTTP
- PostgreSQL (Neon) stores handymen, jobs, dispatch attempts
- Meta WhatsApp Cloud API sends Accept/Reject buttons to handymen
- Webhook receives replies and locks job atomically (SELECT FOR UPDATE)
- First handyman to accept wins; all others auto-rejected in same transaction

## Stack

- Next.js (App Router) on created.app
- PostgreSQL via Neon serverless
- Meta WhatsApp Cloud API
- MCP JSON-RPC 2024-11-05

## Coverage

Currently seeded with handymen across Singapore covering:
- Plumbing
- Electrical
- Aircon servicing
- Carpentry
- Painting
- General repairs

## Planned

- Photo upload + vision-based job diagnosis
- Stripe payment collection
- Telegram + SMS channels
- Mobile app
- Verified handyman profiles

## License

MIT
