---
description: Draft a Zoom Revenue Accelerator coaching agenda or 1:1 prep from recent conversation data.
---

# Prepare ZRA Coaching Agenda

Use this command when a manager wants Codex to draft a coaching agenda, skill-gap analysis, or 1:1 prep based on ZRA conversations.

## Preflight

1. Identify the rep, team, or conversation set being coached.
2. Resolve team/member scope when needed.
3. Confirm the time window, defaulting to recent calls when unspecified.

## Plan

- Use `$zra-coaching-agenda`.
- Resolve users or teams before pulling conversations.
- Pull scorecards and analysis before adding transcript examples.

## Commands

1. Resolve team/user scope with `search_internal_users` and `get_manager_team_and_member` when needed.
2. Fetch recent conversations with `search_conversations`.
3. Fetch scorecards with `get_scorecard_sessions`.
4. Fetch conversation analysis with `get_conversation_analysis`.
5. Enrich the most illustrative conversations with comments or transcript evidence.
6. Draft a coaching agenda with strengths, gaps, evidence, questions, and recommended coaching actions.

## Verification

1. State the sample size and date range.
2. Use confidence language based on number of calls reviewed.
3. Avoid invented benchmarks.

## Summary

```text
## Result
- Action: prepared ZRA coaching agenda
- Status: success | partial | failed
- Details: rep/team, calls reviewed, coaching themes, missing data
```

## Next Steps

- Ask whether the user wants a manager-ready agenda, rep-facing notes, or a follow-up coaching plan.
