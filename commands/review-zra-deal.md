---
description: Review one Zoom Revenue Accelerator deal using deal detail v2, activities, analysis, and linked conversation signals.
---

# Review ZRA Deal

Use this command when the user asks about a specific deal or wants deal-review prep.

## Preflight

1. Identify the deal by ID, name, owner, team, stage, or account.
2. If no deal ID is provided, resolve it with `search_deals`.
3. Confirm whether the user wants a quick health check or a deeper review with activity and conversation evidence.

## Plan

- Use `$zra-deal-review`.
- Enrich only the selected deal and the most relevant linked conversations.
- Use returned activity and deal fields only; do not invent benchmarks.

## Commands

1. Resolve the deal with `search_deals` when needed.
2. Fetch `get_deal_detail_v2`.
3. Fetch `get_deal_activities_v2`.
4. Fetch `get_deal_analysis` and handle empty results.
5. For enriched review, use `search_conversations` with `deal_ids`, then fetch `get_conversation_analysis` for top linked conversations.
6. Synthesize deal state, risk flags, activity evidence, conversation insights, and recommended next actions.

## Verification

1. Confirm the selected deal is the intended one.
2. Separate tool-returned facts from ZRA AI analysis and risk synthesis.
3. State which risk signals were unsupported by returned data.

## Summary

```text
## Result
- Action: reviewed a ZRA deal
- Status: success | partial | failed
- Details: deal, activity evidence, risk flags, missing data
```

## Next Steps

- Run `/review-zra-conversation` for a specific linked call.
- Run `/create-zra-follow-up-package` if the next action is customer follow-up.
