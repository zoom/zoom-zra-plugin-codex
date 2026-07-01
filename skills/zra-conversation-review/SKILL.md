---
name: zra-conversation-review
description: >
  Review a ZRA conversation using search, AI analysis, transcript, scorecard, and
  comment data. Use when the user asks to understand a specific sales call,
  meeting, phone conversation, or conversation ID from Zoom Revenue Accelerator.
---

# Skill: ZRA Conversation Review

## Purpose

Deep-dive one ZRA conversation and produce an evidence-backed in-chat review.

## Required Read

[`../../references/zra-tool-rules.md`](../../references/zra-tool-rules.md)

## MCP Tools Used

| Tool | Role |
|---|---|
| `search_conversations` | Resolve conversation by scope, keyword, date, customer/contact, deal, indicator, or IDs |
| `get_conversation_analysis` | ZRA AI analysis for up to 5 conversations |
| `get_conversation_transcript` | Transcript evidence for exact language or moments |
| `get_scorecard_sessions` | Scorecard sessions and scoring items |
| `get_conversation_comments` | Human comments on the conversation |

## Tool Path

1. Resolve the conversation:
   - ID provided -> call `search_conversations(scope:"MINE", conversation_ids:[id])` unless the user asks for another scope.
   - Latest/my calls -> `search_conversations(scope:"MINE", sort_by:"date_desc", page_size:5)`.
   - Customer/contact/deal named -> resolve with `zra-customer-context` or `zra-deal-review` as needed, then filter with `customer_account_id`, `customer_contact_ids`, or `deal_ids`.
2. If multiple matches are plausible, ask the user to choose.
3. Fetch:
   - `get_conversation_analysis(conversation_ids:[conversation_id])`
   - `get_scorecard_sessions(conversation_ids:[conversation_id])`
   - `get_conversation_comments(conversation_id)`
4. Fetch `get_conversation_transcript(conversation_id)` only when the user asks for exact evidence, quotes, objections, or "where did they say it?".
5. Synthesize the review and clearly label ZRA analysis, transcript evidence, comments, and Codex synthesis.

## Output

```text
## Conversation Review
**Conversation:** [title] | **Type:** [meeting/phone] | **Date:** [time]

### Summary
[ZRA analysis and high-level synthesis]

### Key Topics and Signals
[Topics, deal memo, indicators, sentiment, or metrics returned by analysis]

### Next Steps
[Commitments or next steps from ZRA analysis; mark missing owners/dates]

### Scorecard and Coaching
[Scorecard sessions and score items if returned]

### Evidence
[Transcript snippets or paraphrased moments only when transcript was requested]

### Comments
[Human comments if returned]

### Follow-Up Options
[Offer follow-up package, deal review, or transcript evidence drill-down]
```

## Rules

1. Do not use removed detail/interaction tools.
2. Use `get_conversation_analysis` as the core analysis source.
3. Transcript can be large; use `keyword` for targeted evidence when possible.
4. Empty comments or scorecards are normal; note only when relevant.
5. Do not invent interaction metrics that the new tools do not return.
