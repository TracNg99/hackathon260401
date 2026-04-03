---
name: agent-skill-creator
description: Build and validate conflict-resolution logic, approval polling loops, and agent handoff patterns for the Edge8 multi-agent pipeline
type: skill
agents: ["04-scheduler"]
---

# Skill: agent-skill-creator

## Purpose
Define reusable logic patterns for the Edge8 agent pipeline — particularly conflict resolution, approval polling, agent chaining, and error recovery.

## Invocation
Used in Agent 04 when building the conflict detection and rescheduling logic.

## Pattern 1: Approval Polling Loop

Use this pattern whenever an agent waits for a human (Tommy) to reply via email:

```javascript
async function pollForApproval({
  searchQuery,       // Gmail search string e.g. "from:tommy.dan@edge8.com subject:APPROVE"
  approvalKeywords,  // ["APPROVE", "APPROVE ALL"]
  reviseKeyword,     // "REVISE"
  intervalMs = 300000,   // 5 minutes
  maxAttempts = 48,      // 4 hours total
  onApprove,         // callback when approved
  onRevise,          // callback(feedback) when revision requested
  onTimeout          // callback when max attempts exceeded
}) {
  for (let attempt = 0; attempt < maxAttempts; attempt++) {
    // Search Gmail for reply
    const messages = await gmail_search_messages({ query: searchQuery })

    if (messages.length > 0) {
      const body = await gmail_read_message({ id: messages[0].id })

      if (approvalKeywords.some(kw => body.includes(kw))) {
        return onApprove(body)
      }

      if (body.includes(reviseKeyword)) {
        const feedback = extractFeedback(body)  // parse "REVISE [service]: [feedback]"
        return onRevise(feedback)
      }
    }

    // Wait before next poll
    await sleep(intervalMs)
  }

  return onTimeout()
}
```

## Pattern 2: Agent Handoff

When one agent completes and the next should begin, write a handoff signal file:

```json
// docs/handoff-{from_agent}-{date}.json
{
  "from_agent": "02-graphic-designer",
  "to_agent": "03-web-designer",
  "status": "approved",
  "approved_at": "ISO timestamp",
  "approved_by": "tommy.dan@edge8.com",
  "inputs": {
    "design_specs": ["templates/design-spec-taxation-{date}.json", "..."],
    "graphics": ["assets/graphics/graphic_taxation_{date}.png", "..."]
  }
}
```

The receiving agent checks for this file on startup before proceeding.

## Pattern 3: Conflict Resolution (Scheduler)

```javascript
function resolvePublishDate(baseDate, holidays, calendarEvents) {
  let candidate = baseDate
  let conflictLog = []

  while (true) {
    // Weekend check
    if ([0, 6].includes(candidate.getDay())) {
      conflictLog.push({ date: candidate, reason: 'weekend' })
      candidate = addDays(candidate, 1)
      continue
    }

    // Holiday check
    const holiday = holidays.find(h => isSameDay(h.date, candidate))
    if (holiday) {
      conflictLog.push({ date: candidate, reason: `holiday: ${holiday.name}` })
      candidate = addDays(candidate, 1)
      continue
    }

    // Calendar conflict at 10am
    const conflict = calendarEvents.find(e =>
      isSameDay(e.start, candidate) && getHour(e.start) === 10
    )
    if (conflict) {
      conflictLog.push({ date: candidate, reason: `calendar: ${conflict.summary}` })
      candidate = addDays(candidate, 1)
      continue
    }

    // Valid date found
    return {
      publishDate: candidate,
      conflictDetected: conflictLog.length > 0,
      conflictLog
    }
  }
}
```

## Pattern 4: Error Recovery

For any step that can fail, wrap in a retry with exponential backoff:

```javascript
async function withRetry(fn, { maxRetries = 3, baseDelayMs = 5000, label = 'operation' }) {
  for (let attempt = 1; attempt <= maxRetries; attempt++) {
    try {
      return await fn()
    } catch (err) {
      if (attempt === maxRetries) {
        console.error(`[${label}] Failed after ${maxRetries} attempts:`, err.message)
        throw err
      }
      const delay = baseDelayMs * Math.pow(2, attempt - 1)
      console.warn(`[${label}] Attempt ${attempt} failed. Retrying in ${delay}ms...`)
      await sleep(delay)
    }
  }
}

// Usage:
await withRetry(() => gcal_create_event(eventData), { label: 'create-calendar-event' })
```

## Pattern 5: Batch Rate Limiting

For Agent 05's subscriber email sends:

```javascript
async function batchSend(subscribers, sendFn, { ratePerMinute = 50 }) {
  const delayMs = Math.ceil(60000 / ratePerMinute)  // 1200ms for 50/min
  const results = { sent: [], failed: [] }

  for (const subscriber of subscribers) {
    try {
      await sendFn(subscriber)
      results.sent.push(subscriber.email)
    } catch (err) {
      results.failed.push({ email: subscriber.email, error: err.message })
    }
    await sleep(delayMs)
  }

  // Retry failed sends once
  for (const failed of results.failed) {
    await sleep(30000)  // 30s before retry
    try {
      await sendFn(failed)
      results.sent.push(failed.email)
      results.failed = results.failed.filter(f => f.email !== failed.email)
    } catch {
      // permanently failed — already logged
    }
  }

  return results
}
```
