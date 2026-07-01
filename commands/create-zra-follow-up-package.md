---
description: Draft a reviewed post-call follow-up package in Codex from a Zoom Revenue Accelerator conversation.
---

# Draft ZRA Follow-Up Package

Use this command when the user wants Codex to draft a customer email, internal recap, or action-item list from ZRA conversation data.

## Preflight

1. Resolve the target conversation by ID or with `search_conversations`.
2. Confirm the requested artifact: customer email, internal recap, action items, or full package.
3. Confirm whether deal context or transcript evidence should be included.

## Plan

- Use `$zra-follow-up-package`.
- Draft artifacts only in Codex; do not send, post, update CRM, or write back to ZRA.
- Use `get_conversation_analysis` as the primary source, transcript evidence only when needed.

## Commands

1. Resolve the conversation with `search_conversations`.
2. Fetch `get_conversation_analysis`.
3. Optionally fetch `get_conversation_transcript`, `get_conversation_comments`, and `get_scorecard_sessions`.
4. If a deal is linked, fetch `get_deal_detail_v2`.
5. Draft a customer email, internal recap, action items with owners when available, and missing-data notes.

## Verification

1. Confirm the draft is based on the selected conversation.
2. Mark uncertain owners, dates, or commitments.
3. If the user asks to send, post, update CRM, or write back to ZRA, explain that this plugin can only draft the artifact in Codex.

## Summary

```text
## Result
- Action: drafted a ZRA follow-up package
- Status: success | partial | failed
- Details: artifacts produced, source conversation, missing commitments
```

## Next Steps

- Ask the user whether to revise tone, add commitments, include transcript evidence, or include deal context.
