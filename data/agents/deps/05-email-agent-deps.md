# Agent 05 — Email Agent: MCPs & Skills

## MCPs

### 1. `gmail` (Google Gmail MCP)
- **Purpose:** Batch send personalised follow-up emails to all subscribers every Friday 10am MYT; send delivery summary to Tommy
- **Source:** Already installed (MCP ID: `d5763849-8766-477c-b376-654ad853af1d`)
- **Docs:** https://developers.google.com/gmail/api/guides
- **Setup:** Already configured — no additional setup required
- **Rate limit:** Max 50 emails/minute (enforced in Step 5)
- **Key tools used:**
  - `gmail_create_draft` — stage each personalised email
  - `gmail_list_drafts` — review before batch send
  - `gmail_read_message` — read Tommy's approval/context if needed
  - `gmail_get_profile` — verify sender identity

> **Tip:** Gmail API send quota is 500 emails/day for regular accounts and 2,000/day for Google Workspace. For large subscriber lists, consider using SendGrid or Resend via their MCP instead.

---

## Skills

### 1. `internal-comms`
- **Purpose:** Professional business email writing — personalised tone, clear CTAs, unsubscribe compliance
- **Invoke:** `Skill("internal-comms")`
- **When to use:** Step 3 (composing service-specific follow-up copy) and Step 7 (Tommy's summary report)

---

## Optional Upgrade: High-Volume Email MCP

If subscriber list exceeds Gmail's daily quota, replace `gmail` for bulk sending with one of:

| MCP | Package | Docs |
|-----|---------|------|
| **Resend** | `@resend/mcp-server` | https://resend.com/docs |
| **SendGrid** | Community MCP | https://docs.sendgrid.com |

### Resend Setup (recommended for scale):
```bash
npm install -g @resend/mcp-server
```
```json
{
  "mcpServers": {
    "resend": {
      "command": "npx",
      "args": ["-y", "@resend/mcp-server"],
      "env": { "RESEND_API_KEY": "re_your_key_here" }
    }
  }
}
```
Get API key at: https://resend.com/api-keys

---

## Environment Variables Required

| Variable | Description | Where to Get |
|----------|-------------|--------------|
| *(none additional)* | Gmail MCP already configured | — |
| `RESEND_API_KEY` *(optional)* | For high-volume sending via Resend | https://resend.com/api-keys |

---

## Input Files
- `docs/weekly-brief-{latest}.json` (from Agent 01)
- `docs/schedule-{latest}.json` (from Agent 04)
- `docs/subscribers.json`
- `templates/email-followup.html`
- `assets/style-guide.json` (for `{{SERVICE_COLOR}}` placeholder)

## Output Files
- `docs/email-delivery-{date}.json` — full delivery report with per-segment stats
