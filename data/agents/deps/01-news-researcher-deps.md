# Agent 01 — News Researcher: MCPs & Skills

## Built-in Claude Tools (no installation required)

### 1. `WebSearch`
- **Purpose:** Semantic web search across news sources with date filtering
- **Replaces:** `exa-search` MCP — Claude has this natively
- **Usage:** Search queries like `"SSM Malaysia taxation updates site:ssm.com.my"` with recency filtering
- **No setup required**

### 2. `WebFetch`
- **Purpose:** Direct URL scraping of official gov/org pages (SSM, NAD, ACCA, MASB, HASIL)
- **Replaces:** `fetch` MCP — Claude has this natively
- **Usage:** `WebFetch(url)` returns page content as markdown
- **No setup required**

---

## MCPs

### 1. `gmail` (Google Gmail MCP)
- **Purpose:** Send weekly brief to tommy.dan@edge8.com for topic confirmation
- **Source:** Already installed in this project (MCP ID: `d5763849-8766-477c-b376-654ad853af1d`)
- **Setup:** Already configured — no additional setup required
- **Key tools used:** `gmail_create_draft`, `gmail_search_messages`, `gmail_read_thread`

---

## Skills

### 1. `web-scraping`
- **Purpose:** Structured data extraction from websites — parse headlines, dates, summaries from gov portals
- **Invoke:** `Skill("web-scraping")`
- **When to use:** Step 1–3 (extracting news fields from SSM, NAD, ACCA pages)

### 2. `claude-mem:mem-search`
- **Purpose:** Search past weekly briefs to avoid republishing duplicate stories
- **Invoke:** `Skill("claude-mem:mem-search")`
- **When to use:** Step 4 (scoring) — cross-check headlines against prior weeks

---

## Environment Variables Required

None — all search and fetch is handled by Claude's built-in tools.

---

## Output Files
- `docs/weekly-brief-{YYYY-MM-DD}.json`
