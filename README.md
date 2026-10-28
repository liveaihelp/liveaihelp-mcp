# LIVEAIHELP — Hire a Live Human Expert (MCP Server)

Hire a vetted human expert live via chat or video when your AI agent gets stuck. $1/min.

LIVEAIHELP is a hosted remote [MCP](https://modelcontextprotocol.io) server. When an agent hits something it can't do — a human judgment call, a real-world step, a stuck workflow — it hires a vetted human expert and chats with them through a real-time session, billed per minute from a prepaid balance. There is nothing to install locally: connect by URL with an API key.

- **Endpoint:** `https://www.helpai.live/api/mcp` (streamable HTTP)
- **Auth:** API key — `Authorization: Bearer <key>` (keys look like `lh_…`)
- **Registry:** published on the Official MCP Registry as `live.helpai/liveaihelp` (see `server.json` in this repo)
- **Website:** https://www.helpai.live

## Pricing

- Sessions bill at **$1/min** from your LIVEAIHELP minute balance.
- New accounts get **5 free minutes** at signup — no card required.
- Top up in the portal: bundles of $10 / $25 / $60, or subscriptions of $39/mo and $390/yr.

## Tools

| Tool | Description |
|---|---|
| `hire_human_expert` | Hire a human expert — start a live session (chat or video) with a vetted human expert for a task. Billed at $1/min. If no expert is online, you are placed in a FIFO holding queue. |
| `send_message` | Send a message to the expert — send a chat message to the human expert in an active session. |
| `read_messages` | Read expert messages — read new chat messages from the human expert and the session status. Poll this after sending a message. |
| `check_queue` | Check queue position — check your position in the expert holding queue until you are matched to a session. |
| `end_session` | End session — end the session when the task is done. Stops billing immediately. |
| `check_balance` | Check minute balance — check your LIVEAIHELP minute balance. Sessions bill at $1/min. |

All six tools carry MCP annotations (`title`, `readOnlyHint`, `destructiveHint`, `idempotentHint`, `openWorldHint`). Chat mode is recommended for AI agents.

Typical flow: `hire_human_expert` → `send_message` → poll `read_messages` → `end_session`. If hiring returns a queue position instead of a session, poll `check_queue` until matched.

## Connect

1. **Create a free account** at https://www.helpai.live/signup (you get 5 free minutes).
2. **Create an API key:** open your portal and create a key under **"API keys for bots."** Keys look like `lh_…` (48 hex chars).
3. **Add the server to your MCP client** with your key in the Authorization header:

```json
{
  "mcpServers": {
    "liveaihelp": {
      "url": "https://www.helpai.live/api/mcp",
      "headers": { "Authorization": "Bearer <YOUR_LIVEAIHELP_API_KEY>" }
    }
  }
}
```

**Cline:** add `"transportType": "streamableHttp"` to the entry above —
Cline defaults URL-based servers to the legacy SSE transport, which this
server does not use; without it the connection fails and no tools load.

Works with any MCP-compatible client (Claude, Cursor, Cline, and others that support remote streamable-HTTP servers with header auth). A REST bot API is also available — see the developer docs.

## Agent developers: earn 30%

When your agent can't finish a task, its fallback message can hand the user to a human expert — and you earn on it. Send users to your referral signup link:

```
https://www.helpai.live/signup?ref=YOURCODE
```

`YOURCODE` is your affiliate code — by default, your account username. When a user who signed up through it makes their first purchase, you automatically earn **30% commission plus 5 bonus minutes** — no clicks or impressions, only converted sales. Full terms: https://www.helpai.live/affiliates

## Links

- Website: https://www.helpai.live
- Developer docs: https://www.helpai.live/developers
- Affiliate program: https://www.helpai.live/affiliates
- Support: Support@helpai.live

## About this repo

This is a public descriptor repo for the hosted LIVEAIHELP MCP server (README, `server.json`, `llms-install.md`, logo). The server itself is hosted and company-maintained by PINEAPPLE AI LLC d/b/a LIVEAIHELP; there is no local code to run.
