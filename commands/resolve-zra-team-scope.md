---
description: Resolve Zoom Revenue Accelerator team, manager, member, or internal-user scope for downstream workflows.
---

# Resolve ZRA Team Scope

Use this command when the user asks about a rep, team, manager, managed team, or wants a scoped pipeline/coaching/conversation workflow.

## Preflight

1. Identify whether the request names a person, team, managed team, or explicit team/user ID.
2. Confirm the downstream workflow: pipeline, conversation search, coaching, or customer/deal review.
3. Avoid account-wide `ALL` scope unless explicitly requested and authorized.

## Plan

- Use `$zra-team-scope`.
- Resolve stable IDs before downstream searches.
- Return recommended filters instead of guessing.

## Commands

1. Resolve people with `search_internal_users`.
2. Resolve managed teams with `get_manager_team_and_member`.
3. Resolve specific team detail with `get_manager_team_and_member(team_ids)`.
4. Produce recommended filters for `search_conversations` or `search_deals`.

## Verification

1. Confirm IDs map to the intended user/team.
2. Ask for clarification if multiple matches exist.
3. State if user/team lookup returns empty.

## Summary

```text
## Result
- Action: resolved ZRA team/user scope
- Status: success | partial | failed
- Details: resolved IDs, recommended downstream filters, ambiguities
```

## Next Steps

- Run `/triage-zra-pipeline`, `/prepare-zra-coaching-agenda`, or `/review-zra-conversation` with the resolved filters.
