---
description: Triage visible Zoom Revenue Accelerator pipeline or forecast risk from returned deal data.
---

# Triage ZRA Pipeline

Use this command when the user asks about pipeline health, at-risk deals, forecast prep, deals closing soon, or where to focus.

## Preflight

1. Determine scope: my deals, managed team, specific team, specific rep, stage, close date range, amount range, or named account.
2. Confirm whether the user wants a quick snapshot or enriched risk triage.
3. Note pagination and enrichment limits up front.

## Plan

- Use `$zra-pipeline-triage`.
- Start with `search_deals`.
- Use `get_deal_stages` to validate stage names.
- Enrich only the top 3-5 flagged or highest-value deals unless the user asks for more.

## Commands

1. Resolve team/user scope if needed with `/resolve-zra-team-scope`.
2. Search the scoped deals with `search_deals`.
3. Compute returned-data-only snapshot: count, visible value, stage mix, close-date pressure.
4. Flag risk using only returned fields.
5. Enrich selected deals with `get_deal_detail_v2`, `get_deal_activities_v2`, and optionally `get_deal_analysis`.
6. Synthesize a prioritized triage list and recommended next actions.

## Verification

1. State whether all pages were retrieved or only a sample.
2. Label enriched versus non-enriched deals.
3. Avoid weighted pipeline unless probabilities are returned or supplied.

## Summary

```text
## Result
- Action: triaged ZRA pipeline
- Status: success | partial | failed
- Details: scope, result count, enriched deals, risk flags
```

## Next Steps

- Run `/review-zra-deal` on any flagged deal that needs deeper review.
