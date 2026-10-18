# Installing the B2B Email & Domain Scorer MCP server

This is a remote (hosted) MCP server. There's nothing to clone, build or install.

1. Add this entry to the MCP settings file (for Cline: `cline_mcp_settings.json`):

```json
{
  "mcpServers": {
    "email-scorer": {
      "url": "https://email-scorer.mohamed-bahaa53000.workers.dev/demo/mcp",
      "disabled": false,
      "autoApprove": []
    }
  }
}
```

2. No API key is needed for the demo endpoint (10 tool calls per IP per day).
3. For production use, the user can subscribe on RapidAPI (https://rapidapi.com/MohamedBahaa/api/b2b-email-domain-scorer) and switch the url to `https://b2b-email-domain-scorer.p.rapidapi.com/mcp` with headers `X-RapidAPI-Key` (their key) and `X-RapidAPI-Host: b2b-email-domain-scorer.p.rapidapi.com`.
4. Verify by calling `score_email` with `{"email": "jane.doe@stripe.com"}`. It should return verdict `business`.
