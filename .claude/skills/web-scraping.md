---
name: web-scraping
description: Structured data extraction from websites — parse headlines, dates, summaries, and URLs from Malaysian government and accounting portals (SSM, NAD, ACCA, MASB, HASIL)
type: skill
agents: ["01-news-researcher"]
---

# Skill: web-scraping

## Purpose
Extract structured news data from Malaysian government and accounting portals using Claude's built-in `WebFetch` tool. Output clean, validated JSON fields ready for the weekly brief.

## Invocation
Used automatically in Agent 01, Steps 1–3.

## Extraction Pattern

For each fetched page, extract the following fields:

```json
{
  "headline": "string — exact article title",
  "publication_date": "YYYY-MM-DD",
  "summary": "2–3 sentence plain-English summary",
  "source_url": "string — direct link to the article or press release"
}
```

## Target Sources & Selectors

| Source | URL | What to extract |
|--------|-----|-----------------|
| SSM Malaysia | https://www.ssm.com.my/Pages/News_&_Updates/Media_Releases.aspx | `div.sfContentBlock` — press release titles + dates |
| HASIL | https://www.hasil.gov.my/en/media/media-release/ | Article list items — title, date, href |
| Jabatan Audit Negara | https://www.audit.gov.my/index.php/en/news-highlights | News cards — title, date |
| ACCA Malaysia | https://www.accaglobal.com/my/en/news.html | News article list — title, date, summary |
| MASB | https://www.masb.org.my/ | Standard updates section |

## Parsing Rules

1. **Date normalisation**: Convert all date formats to `YYYY-MM-DD`
   - "1 April 2025" → "2025-04-01"
   - "01/04/2025" → "2025-04-01"

2. **Headline cleaning**: Strip HTML tags, decode entities, trim whitespace

3. **Summary generation**: If no summary exists on the page, generate 2–3 sentences from the article body using `WebFetch` on the article URL

4. **Deduplication**: Skip any article whose headline closely matches a story in the last 4 weekly briefs

5. **Recency filter**: Only include articles published within the last 7 days. If nothing found in 7 days, expand to 14 days and note it.

## Fallback Strategy

If `WebFetch` returns an error or blocked page:
1. Try `WebSearch` with `site:` operator for the same domain
2. If still blocked, log `{ "source": "...", "status": "blocked", "fallback": "search_used" }`
3. Never leave a service with zero results — use search as fallback

## Output Validation

Before passing to Step 4, validate each extracted item:
- `headline` is not empty and < 200 chars
- `publication_date` is a valid date within last 14 days
- `source_url` starts with `https://`
- `summary` is 1–5 sentences
