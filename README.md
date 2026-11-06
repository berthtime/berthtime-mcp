# BerthTime MCP server

Ask your AI assistant where your ocean containers are.

[BerthTime](https://berthtime.com) tracks ocean containers across the major carriers. Its remote **MCP server** lets ChatGPT, Claude, Cursor, VS Code and any other MCP client read your shipments in plain language: where a container is, what arrives this week, what is running late, and the full carrier event history. You can also start tracking new containers.

This repository contains the public connection details, registry metadata and client setup examples. The server is hosted by BerthTime; there is nothing to install or run yourself.

| | |
|---|---|
| Endpoint | `https://berthtime.com/api/v1/mcp` |
| Transport | Streamable HTTP |
| Authentication | OAuth 2.1 (sign in with your BerthTime account), or a BerthTime API key |
| MCP Registry | [`com.berthtime/berthtime`](https://registry.modelcontextprotocol.io/v0.1/servers/com.berthtime%2Fberthtime/versions/latest) |
| Setup guide | https://berthtime.com/ai-assistant |
| Requirements | A BerthTime account ([sign up](https://berthtime.com)) |

## What you can ask

- "Where is container MSCU1234566 right now?"
- "Which of my containers arrive this week?"
- "Is anything delayed by more than two days?"
- "Show me everything that happened to my Felixstowe shipment."
- "Start tracking booking 123456789."

## Tools

**Read:** `list_companies`, `list_shipments`, `list_shared_shipments`, `get_shipment`, `get_shipment_events`, `list_arriving_shipments`, `list_delayed_shipments`, `detect_reference`, `list_carriers`, `get_usage`, `list_alert_rules`, `list_support_requests`.

**Write** (needs read-and-write access, chosen at sign-in, and an owner or admin role in the company; or a read + write API key): `track_shipment`, `update_shipment_identifiers` (set the booking, bill of lading or container numbers on a shipment), `refresh_shipment`, `get_share_link` (creates a link if none exists; paid plans), `create_share_link` (paid plans), `create_support_request`, `archive_shipment`.

Several tools were renamed on 6 November 2026; the old names (`arriving_soon`, `delayed_shipments`, `list_notification_rules`, `list_tickets`, `create_ticket`) still work.

Every tool declares MCP annotations (`readOnlyHint`, `destructiveHint`, `idempotentHint`, `openWorldHint`). If you belong to several companies, pass the optional `company` argument (id, @handle or exact name).

## Connect

### ChatGPT and Claude
Add a custom connector with the URL `https://berthtime.com/api/v1/mcp` and choose OAuth. Step-by-step instructions with screenshots: https://berthtime.com/ai-assistant

### Claude Code
```sh
claude mcp add --transport http berthtime https://berthtime.com/api/v1/mcp
```

### Cursor
`~/.cursor/mcp.json`: see [examples/cursor-mcp.json](examples/cursor-mcp.json).

### VS Code
`.vscode/mcp.json`: see [examples/vscode-mcp.json](examples/vscode-mcp.json).

### With an API key instead of OAuth
Create an API key in BerthTime (Settings → API keys) and send it as a bearer token: see [examples/api-key-header.json](examples/api-key-header.json). An API key acts for a single company.

## Security and privacy

- When you sign in you pick one company, or all the companies you belong to, and whether the assistant may only read or may also make changes. Tokens are valid only on the MCP endpoint; access tokens last one hour and refresh tokens rotate.
- Disconnect a signed-in app in Settings → AI assistants, or revoke an API key in Settings → API keys. Both take effect immediately.
- The server only reads and changes your data in BerthTime. It does not store your conversations; each tool call (tool name and arguments) is recorded in your company's audit log.
- Rate limits: 120 requests per minute and 600 per hour per API key or token, shared with the REST API.
- [Privacy policy](https://berthtime.com/privacy) · [Terms of service](https://berthtime.com/terms)

## Support

team@berthtime.com, or the support page inside the BerthTime app.

---

BerthTime is operated by Majorbird Vietnam Company Limited. The examples in this repository are MIT-licensed; the BerthTime service itself is proprietary.
