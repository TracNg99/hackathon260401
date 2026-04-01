# Agent Dependencies Index

Quick reference for all MCPs and Skills across the 5 Edge8 marketing agents.

## MCP Installation Status

| MCP | Used By | Status | Install Command |
|-----|---------|--------|-----------------|
| `gmail` | 01, 02, 03, 04, 05 | ✅ Installed | — |
| `WebSearch` (built-in) | 01 | ✅ Native to Claude | — |
| `WebFetch` (built-in) | 01 | ✅ Native to Claude | — |
| `canva` | 02 | ❌ Missing | `npx @canva/mcp-server` |
| `git` | 03 | ❌ Missing | `npx @modelcontextprotocol/server-git` |
| `google-calendar` | 04 | ❌ Missing | `npx @modelcontextprotocol/server-google-calendar` |

## Skills Reference

| Skill | Used By | Purpose |
|-------|---------|---------|
| `web-scraping` | 01 | Extract structured data from gov/org sites |
| `claude-mem:mem-search` | 01 | Deduplicate against past briefs |
| `web-design` | 02, 03 | Design system compliance |
| `superpowers:brainstorming` | 02 | Headline ideation |
| `react-nextjs` | 03 | Responsive HTML, semantic structure |
| `schedule` | 04 | Cron/timezone/conflict-resolution logic |
| `internal-comms` | 04, 05 | Professional business email copy |

## Per-Agent Dep Docs

- [01 — News Researcher](./01-news-researcher-deps.md)
- [02 — Graphic Designer](./02-graphic-designer-deps.md)
- [03 — Web Designer](./03-web-designer-deps.md)
- [04 — Scheduler](./04-scheduler-deps.md)
- [05 — Email Agent](./05-email-agent-deps.md)

## Env Variables Checklist

```env
# Agent 01 — no env vars needed (uses Claude built-in WebSearch + WebFetch)

# Agent 02
CANVA_CLIENT_ID=
CANVA_CLIENT_SECRET=

# Agent 03
GIT_REPO_PATH=
GITHUB_TOKEN=          # only if pushing to remote

# Agent 04
GOOGLE_CREDENTIALS_PATH=
GOOGLE_TOKEN_PATH=

# Agent 05 (optional upgrade)
RESEND_API_KEY=
```
