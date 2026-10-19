# Hire Solidmade (remote MCP server)

Let your AI assistant see what Solidmade does, what it costs, and ask for a fixed price on a job.

Solidmade (https://solidmade.co) is a small studio that fixes and builds Shopify stores and websites, and takes on the design, writing, automation and data work around them. Every job gets one fixed price before anything starts.

**Endpoint:** `https://solidmade-mcp.solidmade.workers.dev/mcp`
**Transport:** Streamable HTTP (stateless JSON-RPC). No API key, no sign-up.

## Tools

| Tool | What it does |
|---|---|
| `list_services` | Lists current offers with USD prices: monthly Shopify plans, fixed-price fixes, 48-hour jobs (automation fix, CSV cleanup, landing page) and quoted custom work. Optional `category` filter. Read-only. |
| `get_service` | One offer by `id` (from `list_services`): what it includes, price, how to start, its page on solidmade.co. Read-only. |
| `request_quote` | Sends Solidmade a job request on the user's behalf: `contact_email`, `site_url`, `what_they_need`, optional `budget_optional` and `service_id_optional`. Solidmade replies to that email from hello@solidmade.co within one business day with one fixed price and a date. Nothing starts and nothing is charged until the user agrees. Call it only after the user OKs sharing their email and link. |

## Connect

Claude Code:

```bash
claude mcp add --transport http hire-solidmade https://solidmade-mcp.solidmade.workers.dev/mcp
```

Claude Desktop / Claude.ai: Settings, Connectors, Add custom connector, paste the endpoint URL.

Cursor (`~/.cursor/mcp.json`) or VS Code (`.vscode/mcp.json`, key `servers`):

```json
{
  "mcpServers": {
    "hire-solidmade": { "url": "https://solidmade-mcp.solidmade.workers.dev/mcp" }
  }
}
```

Clients that only speak stdio:

```bash
npx -y mcp-remote https://solidmade-mcp.solidmade.workers.dev/mcp
```

## Try it with curl

```bash
curl -s https://solidmade-mcp.solidmade.workers.dev/mcp \
  -H 'content-type: application/json' -H 'accept: application/json, text/event-stream' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/list"}'
```

## Example prompts

- "My Shopify product page shows the wrong variant price on mobile. Can Solidmade fix it, and for how much?"
- "What does Solidmade charge to look after a Shopify store every month?"
- "Get me a fixed price from Solidmade to clean up a 1,500-row product CSV for import."

## Privacy

`request_quote` stores the email, link and job text you send so Solidmade can reply with a price. It is used only to answer that request. The read-only tools store nothing about you. Contact: hello@solidmade.co.

## License

MIT for this README and `server.json`.
