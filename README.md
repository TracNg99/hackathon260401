# Edge8 Automated Marketing Campaign

> 5-Agent AI workflow: News Research → Graphic Design → Landing Page → Scheduling → Email Follow-up

## Architecture

```
[01 News Researcher] → [02 Graphic Designer] → [03 Web Designer] → [04 Scheduler] → [05 Email Agent]
     Every Tue              Tue (Auto)              Tue (Auto)         Wed 10am         Fri 10am
```

## Services Covered

| Service | News Source | Update Cadence |
|---------|-------------|----------------|
| **Taxation** | SSM Malaysia (`ssm.com.my`) | Weekly |
| **Audit** | National Audit Dept Malaysia (`audit.gov.my`) | Weekly |
| **Account** | ACCA Malaysia (`accaglobal.com/my`) | Weekly |

## Workflow Agents

| # | Agent | File | MCPs | Skills |
|---|-------|------|------|--------|
| 01 | News Researcher | `.windsurf/workflows/01-news-researcher.md` | `exa`, `fetch` | `deep-research`, `web-scraping` |
| 02 | Graphic Designer | `.windsurf/workflows/02-graphic-designer.md` | `canva`, `gmail` | `web-design`, `brainstorming` |
| 03 | Web Designer | `.windsurf/workflows/03-web-designer.md` | `git`, `gmail` | `react-nextjs`, `web-design` |
| 04 | Scheduler | `.windsurf/workflows/04-scheduler.md` | `google-calendar`, `gmail` | `agent-skill-creator` |
| 05 | Email Agent | `.windsurf/workflows/05-email-agent.md` | `gmail` | `internal-comms` |

## Approval Flow

```
Agent 02 output → tommy.dan@edge8.com (graphic approval)
Agent 03 output → tommy.dan@edge8.com (landing page approval)
Approved → Agent 04 publishes to Google Calendar (Wednesday 10am)
Subscribers → Agent 05 follow-up (Friday 10am)
```

## Project Structure

```
Hackathon260401/
├── README.md
├── Problem.pdf
├── assets/
│   ├── infographic.svg          # Workflow illustration (central landing piece)
│   └── style-guide.json         # Design system tokens
├── docs/
│   └── agent-specs.md           # Detailed agent specifications
├── landing-pages/
│   ├── index.html               # Main landing page (infographic as hero)
│   ├── tax-landing.html         # Taxation service page
│   ├── audit-landing.html       # Audit service page
│   └── account-landing.html     # Account service page
├── templates/
│   ├── design-spec.json         # Graphic design specification template
│   ├── email-confirmation.html  # Subscription confirmation email
│   └── email-followup.html      # Friday follow-up email
└── .windsurf/workflows/
    ├── 01-news-researcher.md
    ├── 02-graphic-designer.md
    ├── 03-web-designer.md
    ├── 04-scheduler.md
    └── 05-email-agent.md
```

## Running Workflows

Each workflow is a Windsurf Cowork workflow. Run in sequence each Tuesday:

1. `/01-news-researcher` — Fetches and summarises this week's news
2. `/02-graphic-designer` — Generates graphics, sends to Tommy
3. `/03-web-designer` — Builds landing pages, sends to Tommy
4. `/04-scheduler` — On approval, schedules Wednesday slot
5. `/05-email-agent` — Sends Friday follow-ups to subscribers

## Approval Email

All approval requests are routed to: **tommy.dan@edge8.com**
