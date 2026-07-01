---
name: zra-team-scope
description: >
  Resolve ZRA teams, managers, members, and internal users for scoped pipeline,
  conversation, and coaching workflows. Use when the user asks about a rep, team,
  manager, managed team, or specific scope.
---

# Skill: ZRA Team Scope

## Purpose

Resolve team/user scope so other skills can filter conversations and deals correctly.

## Required Read

[`../../references/zra-tool-rules.md`](../../references/zra-tool-rules.md)

## MCP Tools Used

| Tool | Role |
|---|---|
| `get_manager_team_and_member` | Managed-team overview and team detail/member lists |
| `search_internal_users` | Resolve internal user IDs by name/email |
| `search_conversations` | Validate scoped conversation availability |
| `search_deals` | Validate scoped deal availability |

## Tool Path

1. If the user names a person, call `search_internal_users(keyword)`.
2. If the user asks for managed team scope, call `get_manager_team_and_member()` overview.
3. If specific team IDs are needed, call `get_manager_team_and_member(team_ids:[...])`.
4. Return resolved IDs and recommended filters:
   - Conversations: `scope:"MY_MANAGED"` or `scope:"SPECIFIC", team_ids`, plus `internal_user_ids` if needed.
   - Deals: `scope:"MY_MANAGED"` or `scope:"SPECIFIC", team_ids`, plus `deal_owner_ids` or `deal_team_member_ids`.

## Output

```text
## ZRA Scope Resolution
**Scope:** [rep/team/managed team]

### Resolved IDs
[team IDs or user IDs, only as needed for next tool calls]

### Recommended Filters
[Tool and parameters for the downstream workflow]

### Ambiguities
[Multiple matches or missing users/teams]
```

## Rules

1. Do not use `ALL` when `MY_MANAGED` or `SPECIFIC` is sufficient.
2. If a user search returns empty, ask for a more specific name or email.
3. Do not present team/member lists unless the user asked for scope or roster information.
