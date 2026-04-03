# Agent 03 — Web Designer: MCPs & Skills

## MCPs

### 1. `git` (MCP Git Server)
- **Purpose:** Commit and push generated HTML landing pages to the repository
- **Source:** https://github.com/modelcontextprotocol/servers/tree/main/src/git
- **Setup:**
  1. Install:
     ```bash
     npm install -g @modelcontextprotocol/server-git
     ```
  2. Add to Claude Code MCP config:
     ```json
     {
       "mcpServers": {
         "git": {
           "command": "npx",
           "args": ["-y", "@modelcontextprotocol/server-git"],
           "env": {
             "GIT_REPO_PATH": "/Users/tracnguyendang/Downloads/Hackathon260401"
           }
         }
       }
     }
     ```
  3. Ensure Git is configured with author identity:
     ```bash
     git config user.name "Edge8 Web Designer Agent"
     git config user.email "agent@edge8.com"
     ```
- **Key tools used:** `git_add`, `git_commit`, `git_push`, `git_status`

---

### 2. `gmail` (Google Gmail MCP)
- **Purpose:** Send landing page previews + approval request to tommy.dan@edge8.com
- **Source:** Already installed (MCP ID: `d5763849-8766-477c-b376-654ad853af1d`)
- **Setup:** Already configured — no additional setup required
- **Key tools used:** `gmail_create_draft`, `gmail_search_messages` (poll for APPROVE reply)

---

## Skills

### 1. `react-nextjs`
- **Purpose:** Modern component patterns, responsive mobile-first layout, accessibility best practices
- **Invoke:** `Skill("react-nextjs")`
- **When to use:** Step 2 (building HTML pages) — apply responsive structure, semantic HTML, ARIA labels

### 2. `web-design`
- **Purpose:** Design system compliance — match color palette, typography, and spacing from design spec
- **Invoke:** `Skill("web-design")`
- **When to use:** Step 2 (Color Mapping) — translate `design-spec-*.json` values to CSS variables

---

## Environment Variables Required

| Variable | Description | Where to Get |
|----------|-------------|--------------|
| `GIT_REPO_PATH` | Absolute path to the project repo | Local machine path |
| `GITHUB_TOKEN` *(if pushing to remote)* | GitHub PAT with `repo` scope | https://github.com/settings/tokens |

---

## Input Files
- `templates/design-spec-{service}-{date}.json` × 3 (from Agent 02)
- `assets/graphics/graphic_{service}_{date}.png` × 3 (from Agent 02)
- `assets/style-guide.json`

## Output Files
- `landing-pages/tax-landing.html`
- `landing-pages/audit-landing.html`
- `landing-pages/account-landing.html`
- `landing-pages/index.html` (updated weekly section)
