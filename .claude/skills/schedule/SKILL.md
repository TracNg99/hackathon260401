---
name: schedule
description: Cron scheduling logic, Malaysia timezone handling (Asia/Kuala_Lumpur, UTC+8), conflict resolution against Malaysia public holidays, and next-working-day calculation
type: skill
agents: ["04-scheduler"]
---

# Skill: schedule

## Purpose
Calculate correct publication dates for Edge8's weekly campaign, detect conflicts with Malaysia public holidays and existing calendar events, and resolve to the next available working day.

## Invocation
Used in Agent 04, Steps 2–3 (Calculate Target Date + Check Conflicts).

## Timezone

All Edge8 campaign times use **Asia/Kuala_Lumpur (UTC+8)**. Always include timezone explicitly in Google Calendar API calls.

```
publish_time: "10:00:00"
timezone: "Asia/Kuala_Lumpur"
offset: "+08:00"
```

## Publication Date Calculation

```python
# Target: next Wednesday from today at 10:00 AM MYT
from datetime import date, timedelta

today = date.today()
days_until_wednesday = (2 - today.weekday()) % 7  # Wednesday = weekday 2
if days_until_wednesday == 0:
    days_until_wednesday = 7  # if today is Wednesday, target next Wednesday
base_date = today + timedelta(days=days_until_wednesday)
```

## Conflict Resolution Algorithm

```python
def find_publish_date(base_date, holidays, existing_events):
    candidate = base_date
    max_attempts = 14  # never push more than 2 weeks out

    for _ in range(max_attempts):
        # Skip weekends
        if candidate.weekday() >= 5:  # 5=Saturday, 6=Sunday
            candidate += timedelta(days=1)
            continue

        # Skip Malaysia public holidays
        if candidate in holidays:
            conflict_reason = f"Malaysia public holiday: {holidays[candidate]}"
            candidate += timedelta(days=1)
            continue

        # Skip days with existing 10am events
        if has_conflict(candidate, "10:00", existing_events):
            conflict_reason = f"Existing calendar event at 10:00 AM"
            candidate += timedelta(days=1)
            continue

        return candidate, conflict_reason if candidate != base_date else None

    raise Exception("No available date found within 14 days")
```

## Malaysia Public Holidays Calendar ID

```
en.malaysia#holiday@group.v.calendar.google.com
```

Use with `gcal_list_events` to check for holidays on a given date range.

## Google Calendar API Date Format

Always pass dates in ISO 8601 with timezone offset:
```
{candidate_date}T10:00:00+08:00
```

Example:
```json
{
  "start": { "dateTime": "2025-04-02T10:00:00+08:00", "timeZone": "Asia/Kuala_Lumpur" },
  "end":   { "dateTime": "2025-04-02T11:00:00+08:00", "timeZone": "Asia/Kuala_Lumpur" }
}
```

## Event Colour IDs (Google Calendar)

| Service | Colour | colorId |
|---------|--------|---------|
| Taxation | Blueberry | `9` |
| Audit | Grape | `3` |
| Account | Basil | `10` |

## Conflict Log Format

When a conflict is detected, log before rescheduling:
```json
{
  "original_date": "YYYY-MM-DD",
  "conflict_reason": "Malaysia public holiday: Hari Raya Aidilfitri",
  "rescheduled_to": "YYYY-MM-DD",
  "days_shifted": 2
}
```

## Common Malaysia Public Holidays (reference)

| Holiday | Typical Date |
|---------|-------------|
| New Year's Day | 1 Jan |
| Thaipusam | Jan/Feb (varies) |
| Federal Territory Day | 1 Feb (KL only) |
| Chinese New Year | Jan/Feb (varies, 2 days) |
| Labour Day | 1 May |
| Wesak Day | May (varies) |
| Yang di-Pertuan Agong Birthday | 1st Mon June |
| Hari Raya Aidilfitri | varies (2 days) |
| Hari Raya Aidiladha | varies |
| Awal Muharram | varies |
| National Day | 31 Aug |
| Malaysia Day | 16 Sep |
| Deepavali | Oct/Nov (varies) |
| Prophet Muhammad's Birthday | varies |
| Christmas Day | 25 Dec |

> Always check the live Google Calendar holiday feed rather than hardcoding dates — Islamic holidays shift annually.
