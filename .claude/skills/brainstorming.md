---
name: brainstorming
description: Generate punchy, professional marketing headlines and sub-headlines for Edge8 service graphics — max 8 words for headlines, max 15 words for sub-headlines
type: skill
agents: ["02-graphic-designer"]
---

# Skill: brainstorming

## Purpose
Generate 3 headline options per service graphic, then select the best one based on scoring criteria. Headlines must be punchy, professional, and immediately relevant to Malaysian SME business owners.

## Invocation
Used in Agent 02, Step 2 (Generate Design Headlines).

## Headline Generation Rules

### Constraints
- **Main headline**: max 8 words, no filler words, action-oriented or insight-driven
- **Sub-headline**: max 15 words, adds context without repeating the headline
- **Tone**: professional, authoritative, helpful — not sensational or clickbait
- **Audience**: Malaysian SME owners and finance managers
- **Language**: English (no Bahasa Malaysia in graphics — use in email only)

### Scoring Criteria (pick the highest total)

| Criterion | Description | Max score |
|-----------|-------------|-----------|
| Urgency | Does it convey a deadline or time-sensitive action? | 5 |
| Clarity | Is the benefit or issue immediately obvious? | 5 |
| Relevance | Does it speak directly to an SME owner's concern? | 5 |
| Brevity | Is every word earning its place? | 5 |

### Generation Templates by Service

**Taxation (SSM / HASIL)**
- Pattern: "[New/Updated] [Regulation]: What [You/Your Business] [Must/Should] Know"
- Pattern: "[Deadline] for [Compliance Item] — [Action Verb] Now"
- Pattern: "SSM's New [Rule]: [Direct Impact on Business]"

**Audit (Jabatan Audit Negara)**
- Pattern: "[Report Finding]: [Implication for SMEs]"
- Pattern: "Audit Alert: [Key Risk Area] Under Scrutiny"
- Pattern: "New Audit Requirement for [Business Type] — [What Changes]"

**Account (ACCA / MASB)**
- Pattern: "[Standard Name] Update: [What Changes for Your Books]"
- Pattern: "ACCA Malaysia: [New Requirement] Effective [Date]"
- Pattern: "Are Your Accounts Ready for [New Standard]?"

### Output Format

For each service, produce 3 options then select the winner:

```
TAXATION OPTIONS:
1. "SSM Filing Deadline Extended: Act Before June" — Score: 18/20 ✓ SELECTED
2. "New SSM Requirements for Private Companies" — Score: 14/20
3. "Changes to Company Registration: Full Guide" — Score: 12/20

Selected headline: "SSM Filing Deadline Extended: Act Before June"
Sub-headline: "Companies must update their annual returns by 30 June or face penalties."
Service tag: "TAXATION UPDATE"
CTA: "Read More → edge8.com/taxation"
```
