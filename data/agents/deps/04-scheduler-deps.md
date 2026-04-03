# Agent 04 — Scheduler: MCPs & Skills

## MCPs

### 1. `google-calendar` (Google Calendar MCP)
- **Purpose:** Create weekly campaign events on Wednesday 10am MYT, check Malaysia public holidays, resolve conflicts
- **Source:** https://developers.google.com/calendar/api/guides/overview
- **MCP Registry:** Search `mcp__mcp-registry__search_mcp_registry` for `google-calendar`
- **Setup:**
  1. Enable Google Calendar API at https://console.cloud.google.com
  2. Create OAuth 2.0 credentials (Desktop App type)
  3. Download `credentials.json`
  4. Install the MCP server:
     ```bash
     npx @modelcontextprotocol/server-google-calendar
     ```
  5. Add to Claude Code MCP config:
     ```json
     {
       "mcpServers": {
         "google-calendar": {
           "command": "npx",
           "args": ["-y", "@modelcontextprotocol/server-google-calendar"],
           "env": {
             "GOOGLE_CREDENTIALS_PATH": "/path/to/credentials.json",
             "GOOGLE_TOKEN_PATH": "/path/to/token.json"
           }
         }
       }
     }
     ```
  6. First run will open browser for Google OAuth consent — grant Calendar read/write
- **Key tools used:** `calendar_list_events`, `calendar_create_event`, `calendar_update_event`
- **Malaysia Public Holiday Calendar ID:** `en.malaysia#holiday@group.v.calendar.google.com`

---

### 2. `gmail` (Google Gmail MCP)
- **Purpose:** Send subscriber confirmation emails + rescheduling notifications to Tommy
- **Source:** Already installed (MCP ID: `d5763849-8766-477c-b376-654ad853af1d`)
- **Setup:** Already configured — no additional setup required
- **Key tools used:** `gmail_search_messages` (detect Tommy's APPROVE), `gmail_create_draft`

---

## Skills

### 1. `schedule`
- **Purpose:** Cron scheduling logic, timezone handling (Asia/Kuala_Lumpur), conflict resolution patterns
- **Invoke:** `Skill("schedule")`
- **When to use:** Step 2–3 (calculating publish date, checking conflicts, resolving to next working day)

### 2. `internal-comms`
- **Purpose:** Professional business notification copy for rescheduling alerts and subscriber confirmations
- **Invoke:** `Skill("internal-comms")`
- **When to use:** Step 5 (rescheduling notification email to Tommy) and Step 6 (subscriber confirmation emails)

---

## Environment Variables Required

| Variable | Description | Where to Get |
|----------|-------------|--------------|
| `GOOGLE_CREDENTIALS_PATH` | Path to Google OAuth credentials JSON | https://console.cloud.google.com |
| `GOOGLE_TOKEN_PATH` | Path to stored OAuth token (auto-generated on first run) | Generated locally |

---

## Input Files
- Approval email from tommy.dan@edge8.com (Gmail — subject contains `APPROVE`)
- `docs/weekly-brief-{YYYY-MM-DD}.json` (from Agent 01)
- `docs/subscribers.json`
- `templates/email-confirmation.html`

## Output Files
- `docs/schedule-{date}.json` — Google Calendar event IDs
- `docs/email-delivery-{date}.json` — subscriber send log
- `docs/schedule-registry.json` — appended schedule history
