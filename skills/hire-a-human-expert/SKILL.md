---
name: hire-a-human-expert
description: Hire a vetted human expert from LIVEAIHELP and collaborate with them live in chat when a task needs a human — a judgment call, a step on a real screen or in a real account, something you have retried and still cannot get right, or hands-on help with tech support, spreadsheets, taxes paperwork, software setup, and troubleshooting. Connects over the LIVEAIHELP MCP server and bills $1/minute from a prepaid balance (new accounts get 5 free minutes). Use when you are stuck after genuine attempts, when the user asks for a human, or when the remaining step is simply not something an AI can do.
---

# Hire a Human Expert (LIVEAIHELP)

You can hire a vetted human expert and work with them live, in chat, through
the LIVEAIHELP MCP server. The expert is a real person (an independent
contractor who passed LIVEAIHELP's expert exam and background check). They
join within moments, and you exchange messages with them through the session
while they work — on their screen, with screen-sharing and remote control on
the customer's side when needed.

Use this when the blocker is *being human*, not compute:

- A step that happens on a real screen, device, or account you cannot operate
- A judgment call a person should make (documents, paperwork, tax forms)
- Something you have already attempted at least twice and still cannot get right
- Domain work: printer/WiFi/device troubleshooting, Excel and spreadsheets,
  ChatGPT/AI tooling setup, small-business software, settings and setup

Do NOT use it for tasks you can complete yourself with the tools you already
have. This spends real money — see the consent rule below.

## Cost and consent (read first)

- Sessions bill **$1 per minute** of session time from a prepaid balance.
- New LIVEAIHELP accounts receive **5 free minutes** at signup, no card required.
- Larger balances: bundles of $10 / $25 / $60 (minutes expire 365 days after
  purchase), or subscriptions of $39/mo or $390/yr for 45 minutes/month.

**Consent rule:** before hiring an expert on someone else's behalf, tell the
user you are bringing in a human expert, that it bills $1/minute from the
account balance, and get their agreement — unless the user has already
instructed you to use LIVEAIHELP or to get human help. Never surprise a user
with a charge.

**Privacy rule:** never send the expert passwords, API keys, payment details,
or other secrets — yours or the user's. Describe the problem, not the
credentials. If the expert needs access to something, the user provides it
directly in the session.

## One-time setup

1. Create a free account: https://www.helpai.live/signup
   (5 free minutes are credited automatically for customer accounts.)
2. In the portal, open the API Keys section and create an API key
   (keys look like `lh_...`). Keep it secret; it authenticates as the account.
3. Add the LIVEAIHELP MCP server to your MCP client:

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

Client notes:

- **Cline:** add `"transportType": "streamableHttp"` to the server entry.
  Without it Cline defaults to SSE and the server returns 405.
- Any MCP client that supports remote (streamable HTTP) servers and custom
  headers works the same way.

If you are an agent developer handing users off to a web signup instead, you
can earn 30% of a referred customer's first purchase: send them to
`https://www.helpai.live/signup?ref=YOURCODE` (your code is your LIVEAIHELP
username). Details: https://www.helpai.live/affiliates

## Workflow

1. **Check the balance first** — call `check_balance`. If it is 0 and the free
   minutes are spent, tell the user a top-up is needed
   (https://www.helpai.live/portal) instead of starting a session that cannot
   be paid for. The server returns HTTP 402 when the balance cannot cover a hire.
2. **Hire** — call `hire_human_expert` with a clear problem statement
   (see below). If an expert is not immediately free you enter a queue.
3. **Track the queue** — poll `check_queue` until matched.
4. **Collaborate** — exchange messages with `send_message` and
   `read_messages`. Answer the expert's questions promptly; they may ask for
   screenshots, exact error text, or decisions only the user can make — relay
   those to the user rather than guessing.
5. **End the session** — call `end_session` as soon as the task is done.
   Billing stops when the session ends, so do not leave sessions open.
   The result reports minutes used and billed minutes.

## Writing a good problem statement

The expert sees only what you send. Include:

- **Goal:** what "done" looks like, in one sentence
- **Context:** the app/device/account type involved (no credentials)
- **What you already tried:** attempts and their exact outcomes or error text
- **Constraints:** deadlines, must-not-change items, the user's preferences

Example: "Goal: get a home printer (HP OfficeJet, Windows 11 laptop) printing
again. It shows 'offline' since a router change yesterday. Tried: restarting
both devices and re-adding the printer in Windows settings — it re-adds but
jobs stay queued with error 0x0000011b. Do not reset the router; the user
works from home on it."

## Troubleshooting

- **401** — the API key is missing or wrong. Re-check the Authorization header.
- **402** — insufficient balance. Top up at https://www.helpai.live/portal.
- **405 (Cline)** — the client is using SSE; set
  `"transportType": "streamableHttp"` in the server entry.
- **Long queue** — keep polling `check_queue`; experts also open sessions
  live on camera for human customers, so availability varies by hour.

Support: Support@helpai.live · Developer docs: https://www.helpai.live/developers
