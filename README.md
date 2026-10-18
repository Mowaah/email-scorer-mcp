# B2B Email & Domain Scorer — remote MCP server

Score lead emails and company domains **before they reach your CRM or outreach sequence**. It classifies each one as business / free / role / disposable / risky / invalid, suggests fixes for typo domains, and checks MX, the mail provider and SPF/DMARC.

This is a **hosted (remote) MCP server** using Streamable HTTP. There is nothing to install or run locally, and this repository holds documentation and the MCP Registry manifest only. The server code is not open source.

| Endpoint | Auth | Limits |
|---|---|---|
| `https://email-scorer.mohamed-bahaa53000.workers.dev/demo/mcp` | none | free, 10 tool calls per IP per UTC day, no batch |
| `https://b2b-email-domain-scorer.p.rapidapi.com/mcp` | `X-RapidAPI-Key` + `X-RapidAPI-Host` headers | paid per request via [RapidAPI](https://rapidapi.com/MohamedBahaa/api/b2b-email-domain-scorer) (free Basic plan available) |

Official MCP Registry name: `dev.workers.mohamed-bahaa53000.email-scorer/b2b-email-domain-scorer`

## Tools

- `score_email`: verdict (business / free / role / disposable / risky / invalid / unknown), a 0-100 score, reasons, risk, is_disposable / is_free / is_role / accepts_mail flags, MX and mail provider, and a typo suggestion (`gmial.com` -> `gmail.com`). `deep=true` adds SPF/DMARC. No mail is sent.
- `score_domain`: MX records, mail provider (Google Workspace, Microsoft 365, ...), free/disposable flags, SPF/DMARC for a company domain, URL or email.
- `score_batch`: score a list in one call (**paid endpoint only**).

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
