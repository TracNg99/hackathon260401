# Edge8 Agent Specifications

Complete technical specification for all 5 agents in the automated marketing campaign pipeline.

---

## Agent 01 — News Researcher

| Property | Value |
|----------|-------|
| **Trigger** | Every Tuesday (manual or cron) |
| **Workflow** | `.windsurf/workflows/01-news-researcher.md` |
| **Input** | None (autonomous) |
| **Output** | `docs/weekly-brief-{date}.json` |
| **Approval gate** | Yes — Tommy confirms topic selection via email reply |

### MCPs Required
| MCP | Purpose | Config |
|-----|---------|--------|
| `exa` | Semantic web search across news sources | `EXA_API_KEY` env var |
| `fetch` | Direct URL scraping of official sites | No auth required |

### Skills Required
| Skill | Purpose |
|-------|---------|
| `deep-research` | Multi-source synthesis with citations |
| `web-scraping` | Structured data extraction |

### News Sources
| Service | Primary URL | Secondary URL |
|---------|-------------|---------------|
| Taxation | `https://www.ssm.com.my` | `https://www.hasil.gov.my` |
| Audit | `https://www.audit.gov.my` | National Audit Dept press releases |
| Account | `https://www.accaglobal.com/my` | `https://www.masb.org.my` |

### Scoring Criteria
- **Relevance** (1–5): SME impact
- **Urgency** (1–5): Compliance deadline proximity
- **Engagement** (1–5): Subscriber interest level

---

## Agent 02 — Graphic Designer

| Property | Value |
|----------|-------|
| **Trigger** | On receipt of approved brief from Agent 01 |
| **Workflow** | `.windsurf/workflows/02-graphic-designer.md` |
| **Input** | `docs/weekly-brief-{date}.json`, `assets/style-guide.json` |
| **Output** | `assets/graphics/graphic_{service}_{date}.png` × 3, `templates/design-spec-{service}-{date}.json` × 3 |
| **Approval gate** | Yes — Tommy approves graphics via email reply |

### MCPs Required
| MCP | Purpose | Config |
|-----|---------|--------|
| `canva` | Create and export Canva designs | `CANVA_API_KEY` env var |
| `gmail` | Send approval request to Tommy | Gmail OAuth2 credentials |

### Skills Required
| Skill | Purpose |
|-------|---------|
| `web-design` | Design system and visual composition |
| `brainstorming` | Creative headline and layout ideation |

### Graphic Spec
- **Dimensions**: 1200 × 630px (OG/Social media)
- **Format**: PNG export
- **Template**: `assets/style-guide.json → graphic_templates`
- **Services**: 3 separate designs (Tax/Audit/Account)

---

## Agent 03 — Web Designer

| Property | Value |
|----------|-------|
| **Trigger** | On receipt of approved design specs from Agent 02 |
| **Workflow** | `.windsurf/workflows/03-web-designer.md` |
| **Input** | `templates/design-spec-{service}-{date}.json` × 3 |
| **Output** | `landing-pages/tax-landing.html`, `audit-landing.html`, `account-landing.html`, updated `index.html` |
| **Approval gate** | Yes — Tommy approves landing pages via email reply |

### MCPs Required
| MCP | Purpose | Config |
|-----|---------|--------|
| `git` | Commit and push generated pages | Git credentials |
| `gmail` | Send approval request to Tommy | Gmail OAuth2 credentials |

### Skills Required
| Skill | Purpose |
|-------|---------|
| `react-nextjs` | Modern component patterns and responsive layout |
| `web-design` | Design system compliance, accessibility, UX |

### Page Requirements
- Pure HTML + Tailwind CDN (no build step)
- Fully responsive, mobile-first
- Subscribe form: `id="subscribe-form"`, POST to `/api/subscribe`
- Hero graphic from Agent 02 as full-width header
- 3 key takeaways section
- 1-2-1 session booking CTA

---

## Agent 04 — Scheduler

| Property | Value |
|----------|-------|
| **Trigger** | On receipt of approved landing pages from Agent 03 |
| **Workflow** | `.windsurf/workflows/04-scheduler.md` |
| **Input** | Approval email, `docs/subscribers.json` |
| **Output** | Google Calendar events × 3, `docs/schedule-{date}.json`, subscriber confirmation emails |
| **Approval gate** | None — fully automated post-Tommy approval |

### MCPs Required
| MCP | Purpose | Config |
|-----|---------|--------|
| `google-calendar` | Create, read, update GCal events | Google OAuth2, Calendar API |
| `gmail` | Send subscriber confirmations + rescheduling alerts | Gmail OAuth2 credentials |

### Skills Required
| Skill | Purpose |
|-------|---------|
| `agent-skill-creator` | Build conflict-resolution scheduling logic |

### Conflict Resolution
```
Priority order:
1. No Malaysia public holiday
2. No existing Edge8 event at 10am
3. Not a weekend
4. Minimum 1 day's notice

Fallback: Move forward day-by-day until all conditions met
Max forward offset: 7 days (then alert Tommy manually)
```

### Malaysia Public Holidays Calendar ID
```
en.malaysia#holiday@group.v.calendar.google.com
```

---

## Agent 05 — Email Agent

| Property | Value |
|----------|-------|
| **Trigger** | Every Friday at 10:00am (Asia/Kuala_Lumpur) |
| **Workflow** | `.windsurf/workflows/05-email-agent.md` |
| **Input** | `docs/weekly-brief-{latest}.json`, `docs/subscribers.json`, `docs/schedule-{latest}.json`, `templates/email-followup.html` |
| **Output** | Batch Gmail follow-ups, `docs/email-delivery-{date}.json` |
| **Approval gate** | None — fully automated |

### MCPs Required
| MCP | Purpose | Config |
|-----|---------|--------|
| `gmail` | Compose and send batch follow-up emails | Gmail OAuth2 credentials, `gmail.send` scope |

### Skills Required
| Skill | Purpose |
|-------|---------|
| `internal-comms` | Professional business communication writing |

### Email Rate Limits
- Max 50 emails/minute (Gmail API limit)
- Retry failed sends once after 5 minutes
- `List-Unsubscribe` header required on all emails

---

## Data Flow Diagram

```
[subscribers.json] ──────────────────────────────────────────────→ Agent 04 → Agent 05
                                                                        ↑
[SSM / audit.gov.my / ACCA] → Agent 01 → [weekly-brief.json]
                                              ↓ (Tommy approval)
                                           Agent 02 → [design-spec.json] + [graphics/]
                                              ↓ (Tommy approval)
                                           Agent 03 → [landing-pages/]
                                              ↓ (Tommy approval)
                                           Agent 04 → [GCal events] + [confirmation emails]
                                              ↓ (Friday 10am)
                                           Agent 05 → [follow-up emails]
```

---

## Environment Variables Required

```env
# Agent 01
EXA_API_KEY=your_exa_api_key

# Agent 02
CANVA_API_KEY=your_canva_api_key

# Agents 02, 03, 04, 05
GMAIL_CLIENT_ID=your_oauth_client_id
GMAIL_CLIENT_SECRET=your_oauth_client_secret
GMAIL_REFRESH_TOKEN=your_refresh_token
APPROVAL_EMAIL=tommy.dan@edge8.com

# Agent 04
GOOGLE_CALENDAR_API_KEY=your_gcal_key
GOOGLE_CALENDAR_ID=primary

# Agent 05
CALENDLY_URL_TAX=https://calendly.com/edge8/tax
CALENDLY_URL_AUDIT=https://calendly.com/edge8/audit
CALENDLY_URL_ACCOUNT=https://calendly.com/edge8/account
```

---

## File Naming Conventions

| File Type | Pattern | Example |
|-----------|---------|---------|
| Weekly brief | `docs/weekly-brief-{YYYY-MM-DD}.json` | `docs/weekly-brief-2025-04-01.json` |
| Design spec | `templates/design-spec-{service}-{date}.json` | `templates/design-spec-taxation-2025-04-01.json` |
| Graphic | `assets/graphics/graphic_{service}_{date}.png` | `assets/graphics/graphic_taxation_2025-04-01.png` |
| Schedule | `docs/schedule-{YYYY-MM-DD}.json` | `docs/schedule-2025-04-02.json` |
| Email delivery | `docs/email-delivery-{YYYY-MM-DD}.json` | `docs/email-delivery-2025-04-04.json` |
