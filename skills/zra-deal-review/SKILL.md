---
name: zra-deal-review
description: >
  Review a ZRA deal using search_deals, deal detail v2, activity history, deal AI
  analysis, linked conversations, and customer context. Use for deal health checks
  and deal-review prep.
---

# Skill: ZRA Deal Review

## Purpose

Draft an evidence-backed health check or prep memo for a specific deal.

## Required Read

[`../../references/zra-tool-rules.md`](../../references/zra-tool-rules.md)

## MCP Tools Used

| Tool | Role |
|---|---|
| `search_deals` | Resolve deal by keyword, scope, owner, team, stage, amount, dates, or account |
| `get_deal_detail_v2` | Batch deal detail, owner, stage, amount, contacts, probability, dates |
| `get_deal_activities_v2` | Activity timeline and optional content |
| `get_deal_analysis` | ZRA deal story/playbook analysis |
| `search_conversations` | Linked conversations by `deal_ids` |
| `get_conversation_analysis` | Conversation evidence for linked calls |
| `get_customer_accounts` / `get_customer_contacts` | Optional customer context |

## Tool Path

1. Resolve the deal:
   - ID provided -> `get_deal_detail_v2(deal_ids:[id])`.
   - Name/account/stage/owner -> `search_deals(scope:"MINE", keyword?, filters...)`.
   - Team or managed view -> resolve team/user scope first with `zra-team-scope`.
2. If multiple deals match, ask the user to choose.
3. Fetch `get_deal_detail_v2(deal_ids:[deal_id], include_custom_fields:false)`.
4. Fetch `get_deal_activities_v2(deal_id, page_size:20, sort_by:"date_desc")`.
5. Fetch `get_deal_analysis(deal_id, analysis_type:"ALL")`; handle empty objects as "no deal analysis returned."
6. For enriched review, use `search_conversations(scope:"MINE", deal_ids:[deal_id], page_size:5)` and `get_conversation_analysis` for the most relevant calls.

## Risk Framework

Use only returned data:

| Risk signal | Evidence |
|---|---|
| Past-due close date | `deal_close_date` before today |
| Stale activity | `last_activity_date` or activity timeline gap |
| Low engagement | Few or stale linked activities/conversations |
| Single-threaded | One visible key contact or participant pattern |
| Early stage + high amount | Current result set only; label as "in this set" |
| Low probability | `probability` returned by deal detail |

Do not claim close-date push count, historical trend, or benchmarks unless tools return that data.

## Output

```text
## Deal Review
**Deal:** [name] | **Stage:** [stage] | **Amount:** [amount] | **Owner:** [owner]

### Health Summary
[GREEN/YELLOW/RED with evidence]

### Activity Timeline
[Recent activities from get_deal_activities_v2]

### ZRA Deal Analysis
[Story/playbook if returned]

### Linked Conversation Signals
[Conversation analysis if pulled]

### Recommended Next Actions
[Specific actions grounded in returned data]
```

## Rules

1. Use `deal_id`, not account-name inference, for linked conversations when available.
2. Treat object-level access failures as partial access, not missing scope unless the server names a scope.
3. Enrich only the selected deal or top 3-5 deals.
