# Agent 02 — Graphic Designer: MCPs & Skills

## MCPs

### 1. `canva` (Canva MCP)
- **Purpose:** Create and export social post graphics (1200×630px) via Canva API
- **Source:** https://www.canva.com/developers/docs/
- **MCP Registry:** https://mcp.run/registry/canva
- **Setup:**
  1. Create a Canva App at https://www.canva.com/developers/
  2. Enable the Design API and get your `CLIENT_ID` + `CLIENT_SECRET`
  3. Install the Canva MCP server:
     ```bash
     npx @canva/mcp-server
     ```
  4. Add to Claude Code MCP config:
     ```json
     {
       "mcpServers": {
         "canva": {
           "command": "npx",
           "args": ["-y", "@canva/mcp-server"],
           "env": {
             "CANVA_CLIENT_ID": "your_client_id",
             "CANVA_CLIENT_SECRET": "your_client_secret"
           }
         }
       }
     }
     ```
  5. Authenticate with OAuth on first run (browser redirect)
- **Key tools used:** `canva_create_design`, `canva_export_design`

> **Note:** If Canva MCP is not yet available publicly, fall back to using the Canva HTTP API directly via the `fetch` MCP with a Bearer token.

---

### 2. `gmail` (Google Gmail MCP)
- **Purpose:** Send 3 graphic previews + approval request to tommy.dan@edge8.com
- **Source:** Already installed (MCP ID: `d5763849-8766-477c-b376-654ad853af1d`)
- **Setup:** Already configured — no additional setup required
- **Key tools used:** `gmail_create_draft`, `gmail_search_messages` (poll for APPROVE reply)

---

## Skills

### 1. `web-design`
- **Purpose:** Design system compliance — enforce Edge8 color palette, typography, and layout structure
- **Invoke:** `Skill("web-design")`
- **When to use:** Step 2 (generating headlines & layout) and Step 3 (Canva element spec)

### 2. `superpowers:brainstorming`
- **Purpose:** Generate punchy, professional 8-word headlines and sub-headlines for each service
- **Invoke:** `Skill("superpowers:brainstorming")`
- **When to use:** Step 2 (Design Headlines) — brainstorm 3 options per service, pick best

---

## Environment Variables Required

| Variable | Description | Where to Get |
|----------|-------------|--------------|
| `CANVA_CLIENT_ID` | Canva OAuth App client ID | https://www.canva.com/developers/ |
| `CANVA_CLIENT_SECRET` | Canva OAuth App client secret | https://www.canva.com/developers/ |

---

## Input Files
- `docs/weekly-brief-{YYYY-MM-DD}.json` (from Agent 01)
- `assets/style-guide.json`

## Output Files
- `assets/graphics/graphic_{service}_{date}.png` × 3
- `templates/design-spec-{service}-{date}.json` × 3
