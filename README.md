# B2B Email & Domain Scorer — remote MCP server

**For sales and lead-gen teams: get a keep, fix, review or drop decision for every lead email before you send outreach.**

**1. Connect** (free, no key; works in Claude Desktop, Cursor, Cline, VS Code and any client that supports remote Streamable HTTP):

```json
{
  "mcpServers": {
    "email-scorer": {
      "url": "https://email-scorer.mohamed-bahaa53000.workers.dev/demo/mcp"
    }
  }
}
```

**2. Ask:** *"Is jane@gmial.com OK to email?"* Your agent calls:

```json
{"name":"score_email","arguments":{"email":"jane@gmial.com"}}
```

**3. Get** (real output from the free demo endpoint, trimmed):

```json
{"action":"fix","send_to":"jane@gmail.com",
 "action_reason":"Likely typo: send to jane@gmail.com instead",
 "verdict":"risky","score":10,"reasons":["likely_typo","typo_domain_is_disposable_or_parked"]}
```

## What makes it different

- **Decisions, not raw flags**: most email validators return 15 booleans and leave your agent to guess. This returns what to do.
- **Typos fixed, not just flagged**: `jane@gmial.com` comes back as `fix` -> `jane@gmail.com`.
- **A ready-to-send list**: `score_batch` returns `send_list`, the deduplicated list of kept plus corrected addresses, and keep/fix/review/drop counts.
- **B2B-aware**: role inboxes (info@, sales@) and free mailboxes go to `review` with the reason, because they reach a company but rarely a decision-maker.
- **No mail is sent** (no SMTP probing that can get your IP blocklisted), and it's **MCP-native with a free, keyless demo endpoint**.

## No-code version

Not building an agent? The same scoring comes in ready-made tools:

- **[Free email list cleaner](https://email-scorer.mohamed-bahaa53000.workers.dev/tools/email-list-cleaner)** checks and cleans up to 25 emails in your browser, with no sign-up.
- **[Free n8n template: Lead Scoring & Routing](https://mohabaha0.gumroad.com/l/lead-routing-free)** scores form leads with Gemini, logs them to Sheets and alerts Slack.

## Try it free

- **No key, right now:** point any MCP client at `https://email-scorer.mohamed-bahaa53000.workers.dev/demo/mcp` (10 tool calls per IP per day).
- **Free RapidAPI plan:** 100 requests/month on the [Basic plan](https://rapidapi.com/MohamedBahaa/api/b2b-email-domain-scorer/pricing), no card needed. Pro is $3.99 for 30,000.

This is a **hosted (remote) MCP server** using Streamable HTTP. There is nothing to install or run locally, and this repository holds documentation and the MCP Registry manifest only. The server code is not open source.

| Endpoint | Auth | Limits |
|---|---|---|
| `https://email-scorer.mohamed-bahaa53000.workers.dev/demo/mcp` | none | free, 10 tool calls per IP per UTC day, no batch |
| `https://b2b-email-domain-scorer.p.rapidapi.com/mcp` | `X-RapidAPI-Key` + `X-RapidAPI-Host` headers | paid per request via [RapidAPI](https://rapidapi.com/MohamedBahaa/api/b2b-email-domain-scorer) (free Basic plan available) |

Official MCP Registry name: `dev.workers.mohamed-bahaa53000.email-scorer/b2b-email-domain-scorer`

## Tools

- `score_email`: `action` (keep / fix / review / drop), `send_to`, `action_reason`, plus verdict (business / free / role / disposable / risky / invalid / unknown), a 0-100 score, reasons, risk, is_disposable / is_free / is_role / accepts_mail flags, MX and mail provider, and a typo suggestion (`gmial.com` -> `gmail.com`). `deep=true` adds SPF/DMARC. No mail is sent.
- `score_domain`: MX records, mail provider (Google Workspace, Microsoft 365, ...), free/disposable flags, SPF/DMARC for a company domain, URL or email.
- `score_batch`: clean a list in one call and get `send_list` plus action counts (**paid endpoint only**).

## Use cases

- Lead-gen and SDR agents: drop disposable and free-mail signups and flag role accounts (info@, sales@) before outreach.
- Fix typo domains in form signups before sending.
- Qualify a company domain: does it receive mail, and on which provider?

## Connect

```json
{
  "mcpServers": {
    "email-scorer": {
      "url": "https://email-scorer.mohamed-bahaa53000.workers.dev/demo/mcp"
    }
  }
}
```

Paid / production (RapidAPI):

```json
{
  "mcpServers": {
    "email-scorer": {
      "url": "https://b2b-email-domain-scorer.p.rapidapi.com/mcp",
      "headers": {
        "X-RapidAPI-Key": "YOUR_RAPIDAPI_KEY",
        "X-RapidAPI-Host": "b2b-email-domain-scorer.p.rapidapi.com"
      }
    }
  }
}
```

For clients that only support stdio: `npx -y mcp-remote https://email-scorer.mohamed-bahaa53000.workers.dev/demo/mcp`

## Notes

- Stateless. Emails aren't stored beyond standard request logs, and the server never sends SMTP probes.
- Scores are heuristics for lead quality, not a deliverability guarantee.
