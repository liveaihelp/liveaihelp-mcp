# Installing the LIVEAIHELP MCP Server

LIVEAIHELP is a **hosted remote MCP server** — there is no package to
install and no local process to run. Installation means adding the server
URL and an API key to your MCP client's configuration.

## What this server does

Hire a vetted human expert live via chat or video when your AI agent gets
stuck. $1/min. The agent starts a real-time session with a human expert,
exchanges chat messages with them, and ends the session when the task is
done. Sessions are billed per minute from a prepaid LIVEAIHELP balance.

Tools provided:

- `hire_human_expert` — start a session (or join the FIFO holding queue if no expert is online)
- `send_message` — send a chat message to the expert
- `read_messages` — read the expert's replies and the session status
- `check_queue` — check holding-queue position until matched
- `end_session` — end the session and stop billing
- `check_balance` — check the account's minute balance

## Prerequisites

1. A free LIVEAIHELP account: https://www.helpai.live/signup
   (new accounts get 5 free minutes; no card required).
2. An API key: sign in, open the portal, and create a key under
   **"API keys for bots."** Keys look like `lh_…` (48 hex chars).
   Keep the key secret — it spends from your minute balance.

## Configuration

Add this entry to your MCP client's server configuration (the
`mcpServers` object used by Claude Desktop, Cursor, Cline, and similar
clients):

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

Replace `<YOUR_LIVEAIHELP_API_KEY>` with your `lh_…` key.

**Cline users:** Cline defaults URL-based servers to the legacy SSE
transport, which this server does not use — without this extra field the
connection fails and no tools load. Add `"transportType": "streamableHttp"`
to the `liveaihelp` entry (equivalently, choose **Streamable HTTP** when
adding the server in Cline's MCP settings UI, which edits the same
`cline_mcp_settings.json` file):

```json
{
  "mcpServers": {
    "liveaihelp": {
      "url": "https://www.helpai.live/api/mcp",
      "headers": { "Authorization": "Bearer <YOUR_LIVEAIHELP_API_KEY>" },
      "transportType": "streamableHttp"
    }
  }
}
```

## Server details

- **Transport:** streamable HTTP (remote)
- **Endpoint:** `https://www.helpai.live/api/mcp`
- **Auth:** `Authorization: Bearer <key>` header on every request — the
  only auth mechanism; there is no OAuth flow and no other credential.
- **Billing:** $1/min from the account's minute balance while a session
  is open. Check the balance any time with the `check_balance` tool; top
  up at https://www.helpai.live/portal.

## Verify the connection

After adding the config, restart/reload your MCP client and confirm the
six tools above are listed. A safe first call is `check_balance` — it is
read-only and returns your current minute balance.
