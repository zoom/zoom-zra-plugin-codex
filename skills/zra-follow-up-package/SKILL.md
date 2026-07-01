---
name: zra-follow-up-package
description: >
  Draft a post-call follow-up package in Codex from a ZRA conversation: customer
  email draft, internal recap, action items, transcript-backed evidence, and deal
  context when available.
---

# Skill: ZRA Follow-Up Package

## Purpose

Turn a ZRA conversation into a reviewed in-chat draft package. This skill drafts only; it does not send, post, update CRM, or write back to ZRA.

## Required Read

[`../../references/zra-tool-rules.md`](../../references/zra-tool-rules.md)

## MCP Tools Used

| Tool | Role |
|---|---|
| `search_conversations` | Resolve the call |
| `get_conversation_analysis` | Next steps, topics, deal memo, indicators |
| `get_conversation_transcript` | Optional exact wording for commitments |
| `get_conversation_comments` | Manager or peer notes |
| `get_scorecard_sessions` | Optional call-quality context |
| `get_deal_detail_v2` | Optional linked deal context |

## Tool Path

1. Resolve the conversation with `search_conversations`.
2. Fetch `get_conversation_analysis(conversation_ids:[id])`.
3. Fetch comments and scorecards when the user wants internal recap or coaching context.
4. Fetch transcript only when commitments are unclear or exact wording matters.
5. If `search_conversations` returned `deal_id`, fetch `get_deal_detail_v2(deal_ids:[deal_id])`.
6. Draft the requested artifact in Codex.

## Output

```text
## Follow-Up Package

### Action Items
| Action | Owner | Due date | Source |

### Customer Follow-Up Email Draft
[Draft email]

### Internal Recap
[Summary, risks, customer signals, deal impact]

### Evidence and Confidence Notes
[What came from ZRA analysis, transcript, comments, and Codex synthesis]
```

## Rules

1. Always include a "review before using" note for customer-facing drafts.
2. Mark uncertain owners, dates, or commitments instead of inventing them.
3. If next steps are missing, draft a softer recap and ask the user to add commitments.
4. Do not imply any external action was taken.
