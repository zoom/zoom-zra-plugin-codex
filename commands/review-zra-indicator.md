---
description: Review Zoom Revenue Accelerator indicators and related conversations.
---

# Review ZRA Indicator

Use this command when the user asks about a tracked topic, indicator, objection, competitor, risk signal, or recurring sales theme in ZRA.

## Preflight

1. Identify the indicator name, topic, keyword, or tracker intent.
2. Confirm the scope: my conversations, managed team, specific team/user, account, contact, deal, or date range.
3. Confirm whether the user wants a quick list of matching conversations or an evidence-backed pattern review.

## Plan

- Use `$zra-indicator-review`.
- Resolve indicator IDs before filtering conversations.
- Use transcript evidence only for the most relevant examples.

## Commands

1. Search indicator trackers with `search_indicators`.
2. Resolve team/user scope with `search_internal_users` and `get_manager_team_and_member` when requested.
3. Search related conversations with `search_conversations` using the resolved `indicator_id` and requested scope/date filters.
4. Fetch `get_conversation_analysis` for the top matching conversations.
5. Optionally fetch `get_conversation_transcript` with a keyword for exact examples.
6. Summarize matched indicators, related conversations, pattern strength, evidence, and recommended follow-up.

## Verification

1. Confirm the selected indicator is the intended tracker.
2. State whether the result is based on one page or all retrieved pages.
3. Separate indicator matches, ZRA analysis, transcript evidence, and Codex synthesis.

## Summary

```text
## Result
- Action: reviewed a ZRA indicator
- Status: success | partial | failed
- Details: indicator, scope, conversations reviewed, evidence gaps
```

## Next Steps

- Run `/find-zra-transcript-evidence` for exact moments from a selected conversation.
- Run `/review-zra-conversation` for a full review of a matching conversation.
- Run `/review-zra-deal` if the indicator is tied to a specific deal risk.
