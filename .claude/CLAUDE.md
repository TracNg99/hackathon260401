# Edge8 Automated Marketing Campaign — Claude Project Context

## Project Overview
This is a 5-agent automated marketing pipeline for Edge8, a Malaysian accounting and advisory firm. Each Tuesday, the pipeline fetches news, generates graphics, builds landing pages, schedules publication, and sends follow-up emails — fully automated via Claude Code.

## Agent Pipeline
```
Tuesday 9am  → Agent 01: News Researcher    → docs/weekly-brief-{date}.json
               Agent 02: Graphic Designer   → assets/graphics/ + templates/design-spec-*
               Agent 03: Web Designer       → landing-pages/*.html
               Agent 04: Scheduler          → Google Calendar events + subscriber emails
Friday 10am  → Agent 05: Email Agent        → follow-up emails to all subscribers
```

## Skills Available (`.claude/skills/`)

| Skill file | Used by | Purpose |
|------------|---------|---------|
| `web-scraping.md` | Agent 01 | Extract news from SSM, NAD, ACCA, MASB, HASIL |
| `web-design.md` | Agent 02, 03 | Edge8 colour palette, typography, component patterns |
| `brainstorming.md` | Agent 02 | Generate and score marketing headlines |
| `react-nextjs.md` | Agent 03 | HTML landing page templates, form handling, checklist |
| `schedule.md` | Agent 04 | Malaysia timezone, conflict resolution, holiday calendar |
| `internal-comms.md` | Agent 04, 05 | Email copy templates, tone, unsubscribe compliance |
| `agent-skill-creator.md` | Agent 04 | Polling loops, handoff patterns, retry logic |

## Installed MCPs

| MCP | ID | Used by |
|-----|----|---------|
| Gmail | `mcp__d5763849-8766-477c-b376-654ad853af1d` | All agents (drafts) |
| Canva | `mcp__f30a5ae3-bb3a-4dd6-a071-f7db57f84bc6` | Agent 02 |
| Google Calendar | `mcp__ef3bef77-7099-431c-9014-16671172f213` | Agent 04 |
| Playwright | built-in | Agent 02, 03, 04, 05 (email send) |
| computer-use | built-in | Agent 01, 03, 04 (inbox polling) |

## Key Contacts
- **Tommy Dan** — tommy.dan@edge8.com — approves all outputs before pipeline advances

## Key Files
- `docs/weekly-brief-{date}.json` — selected news stories per service
- `docs/subscribers.json` — subscriber list with service preferences
- `assets/style-guide.json` — Edge8 brand tokens
- `templates/design-spec-{service}-{date}.json` — Canva design specs for Agent 03
- `templates/designer-handoff-{service}-{date}.md` — photo zone instructions for human designer
- `docs/schedule-{date}.json` — Google Calendar event IDs
- `docs/email-delivery-{date}.json` — send log

## Behaviour Rules
- Never proceed to the next agent without Tommy's APPROVE reply
- Always read `assets/style-guide.json` before generating any visual output
- Graphic photo zone must be at x:780, width:420, height:630 — do not alter
- Email rate limit: max 50 sends/minute via Playwright
- All dates/times in Asia/Kuala_Lumpur (UTC+8)
