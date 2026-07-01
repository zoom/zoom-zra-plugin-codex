---
name: zra-indicator-review
description: >
  Review ZRA indicator trackers and related conversations. Use when the user asks
  about tracked topics, objections, competitors, risk signals, recurring themes,
  or where an indicator appeared in Zoom Revenue Accelerator data.
---

# Skill: ZRA Indicator Review

## Purpose

Resolve a ZRA indicator and review related conversations without over-fetching transcripts.

## Required Read

[`../../references/zra-tool-rules.md`](../../references/zra-tool-rules.md)

## MCP Tools Used

| Tool | Role |
|---|---|
| `search_indicators` | Resolve indicator trackers by keyword or name |
| `search_conversations` | Find conversations filtered by indicator, scope, customer, deal, team, user, or date |
| `get_conversation_analysis` | Summarize topics, next steps, deal memo, sentiment, and returned indicator context |
| `get_conversation_transcript` | Optional exact examples for selected conversations |
| `search_internal_users` / `get_manager_team_and_member` | Optional team/user scope resolution |

## Tool Path

1. Resolve the indicator with `search_indicators(keyword, page_size:5)`.
2. If multiple indicators match, ask the user to choose.
3. Resolve team/user scope if the request names a rep, team, or manager.
4. Search matching conversations with `search_conversations(scope, indicator_id, page_size:10, date filters as needed)`.
5. Fetch `get_conversation_analysis(conversation_ids:[...])` in batches of up to 5 for the top matches.
6. Fetch keyword-filtered transcript evidence only for the most useful examples or when the user asks for exact language.

## Output

```text
## Indicator Review
**Indicator:** [name/id] | **Scope:** [scope] | **Conversations reviewed:** [N]

### Pattern Summary
[What appeared across the returned conversations]

### Matching Conversations
| Conversation | Date | Customer/deal | Signal | Evidence |

### Representative Evidence
[Transcript-backed examples only when fetched]

### Follow-Up Options
[Review conversation, review deal, create follow-up package]
```

## Rules

1. Do not treat an indicator keyword as an ID; resolve it with `search_indicators`.
2. Label partial pages and do not claim all historical matches unless pagination was completed.
3. Do not dump full transcripts. Use keyword-filtered transcript calls for exact examples.
4. Keep indicator matches distinct from Codex synthesis and ZRA conversation analysis.
