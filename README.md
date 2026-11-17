# Lawn Service IQ MCP server through Cotlet.dev

Connect Lawn Service IQ to ChatGPT, Claude, Codex and Claude Code through Cotlet.dev. Review crew progress, plan routes, prepare estimates and share service proof from your own signed-in Lawn Service IQ Cloud office account.

This is an unofficial integration, not affiliated with Lawn Service IQ. Cotlet.dev hosts the connection through your Cloud Browser. This repository provides an optional Python stdio MCP bridge.

## Connect your account

1. [Request access to Lawn Service IQ](https://cotlet.dev/signup?app=lawn-service-iq).
2. Open https://lawnserviceiq.com/login in your Cotlet.dev Cloud Browser and sign in with your Lawn Service IQ Cloud office login. Enable Lawn Service IQ for agents.
3. Connect your agent. Refresh the connector if you enabled Lawn Service IQ after connecting.

### ChatGPT

Open Settings > Apps & Connectors > Create. Enter `https://cotlet.dev/mcp/v1/connect` as the MCP server URL and choose OAuth.

### Claude

Open Settings > Connectors > Add custom connector and enter `https://cotlet.dev/mcp/v1/connect`.

### Codex

Create a Cotlet.dev API key in your console and set `COTLET_API_KEY` in your environment. Add:

```toml
[mcp_servers.cotlet]
url = "https://cotlet.dev/mcp/v1"
bearer_token_env_var = "COTLET_API_KEY"
```

### Claude Code

```bash
claude mcp add --transport http cotlet https://cotlet.dev/mcp/v1 --header "Authorization: Bearer $COTLET_API_KEY"
```

## Dispatch and estimate workflows

The [live Lawn Service IQ reference](https://cotlet.dev/integrations/lawn-service-iq-api-mcp-server) lists six hosted capabilities: one read and five write, grouped under `lawn_service_iq_read` and `lawn_service_iq_write`.

| Capability | What your agent can do |
|---|---|
| `view_crew_work` | Read stops and completion progress per crew. |
| `build_crew_routes` | Read route metrics, move stops between crews or sort drive order. |
| `move_dispatch_day` | Read the forecast or move unworked stops to another day. |
| `measure_lot` | Read saved lawn area or measure a lot on the satellite map. |
| `create_estimate` | List estimates or create one using your rate tiers. |
| `share_service_proof` | Read proof, copy its customer link or email it to the customer. |

Write tools ask for confirmation by default, configurable per tool. Reading actions run at once. Review dates, crews, affected customers, measured areas and estimate totals before approving changes. Moving a day can shift future visits and email customers. Saved estimates appear in the customer portal; sent emails already reach their recipients.

Examples to ask your agent:

- "Show today's open stops for each crew."
- "Compare the routes by dollars per mile before proposing changes."
- "Check the forecast and prepare a rain-day move for my approval."
- "Read this client's lawn area and prepare an estimate from my service rates."
- "Find the proof link for the latest completed visit."

These workflows use the Cloud office account. Payments, saved cards, QuickBooks, payroll, employee logins and other Settings are outside these hosted capabilities. The source page's official-connection check, dated 15 November 2026, found no public Lawn Service IQ API or official MCP server.

## Optional local stdio bridge

Python 3.10 or newer is required, with no runtime dependencies. Clone this repository and configure a client that supports stdio:

```json
{"mcpServers":{"lawn-service-iq":{"command":"python3","args":["/absolute/path/to/lawn-service-iq-mcp/lawn_service_iq_agent.py"],"env":{"COTLET_API_KEY":"your-cotlet-key"}}}}
```

The local bridge lists seven browser helpers: `lawn_service_iq_guide`, `lawn_service_iq_status`, `lawn_service_iq_open`, `lawn_service_iq_read`, `lawn_service_iq_click`, `lawn_service_iq_type` and `lawn_service_iq_close`. They forward to Cotlet.dev's shared browser tools. This local browser-helper interface is separate from the six hosted workflow capabilities above. Call the guide first and close the browser when finished.

Discovery works without a key. Executing a tool requires your Cotlet.dev API key and connected account.

```bash
python3 lawn_service_iq_agent.py connect
python3 lawn_service_iq_agent.py guide
python3 lawn_service_iq_agent.py status
python3 lawn_service_iq_agent.py serve
```

You can also build the included Dockerfile and pass `COTLET_API_KEY` as an environment variable.

## Reference and access

- [Lawn Service IQ connection guide and operation reference](https://cotlet.dev/integrations/lawn-service-iq-api-mcp-server?utm_source=github&utm_medium=readme&utm_campaign=lawn-service-iq)
- [Request access](https://cotlet.dev/signup?app=lawn-service-iq)
- [Cotlet.dev console](https://cotlet.dev/app)

Licensed under Elastic License 2.0. See LICENSE.
