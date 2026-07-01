---
description: Review one Zoom Revenue Accelerator conversation using ZRA analysis, transcript, scorecard, and comment data.
---

# Review ZRA Conversation

Use this command when the user wants to understand a specific ZRA sales call, meeting, or phone conversation.

## Preflight

1. Identify the conversation by ID, title, participant, customer, contact, deal, date, or "latest" intent.
2. If no conversation ID is provided, resolve it with `search_conversations`.
3. Check whether the user wants quick recap, transcript evidence, scorecards, comments, or a full review.

## Plan

- Use `$zra-conversation-review`.
- Use the current 15-tool surface, not removed `list_all_conversations` or `_by_id` tools.
- Fetch transcript only when exact evidence is useful.

## Commands

1. Resolve the conversation with `search_conversations`.
2. Fetch `get_conversation_analysis`.
3. Fetch `get_scorecard_sessions` and `get_conversation_comments` for enriched review.
4. Fetch `get_conversation_transcript` when exact wording or keyword evidence is requested.
5. Synthesize summary, key topics, next steps, scorecards, comments, transcript evidence, and deal-relevant signals.

## Verification

1. Confirm the selected conversation is the intended one.
2. Label whether the response is based on full or partial data.
3. Separate ZRA AI analysis, transcript evidence, comments, and Codex synthesis.

## Summary

```text
## Result
- Action: reviewed a ZRA conversation
- Status: success | partial | failed
- Details: conversation, tools used, missing data
```

## Next Steps

- Run `/find-zra-transcript-evidence` for exact transcript moments.
- Run `/create-zra-follow-up-package` for an email, recap, or action-item package.
- Run `/review-zra-deal` if the conversation is tied to a deal health question.
