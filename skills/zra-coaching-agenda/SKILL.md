---
name: zra-coaching-agenda
description: >
  Draft a coaching agenda or 1:1 prep from ZRA conversations, scorecard sessions,
  transcript evidence, analysis, comments, and team/user scope.
---

# Skill: ZRA Coaching Agenda

## Purpose

Draft evidence-based coaching notes from accessible ZRA conversations.

## Required Read

[`../../references/zra-tool-rules.md`](../../references/zra-tool-rules.md)

## MCP Tools Used

| Tool | Role |
|---|---|
| `search_conversations` | Find calls by scope, team, user, customer, date, or IDs |
| `get_scorecard_sessions` | Scorecard patterns |
| `get_conversation_analysis` | Topics, indicators, good questions, deal memo, sentiment, metrics |
| `get_conversation_transcript` | Optional examples and exact evidence |
| `get_conversation_comments` | Existing manager or peer feedback |
| `search_internal_users` / `get_manager_team_and_member` | Rep/team scope resolution |

## Scope Resolution

| Scenario | Approach |
|---|---|
| Current user's calls | `search_conversations(scope:"MINE")` |
| Managed team calls | `search_conversations(scope:"MY_MANAGED")` |
| Named rep | `search_internal_users(keyword)`, then `internal_user_ids` |
| Specific team | `get_manager_team_and_member`, then `scope:"SPECIFIC", team_ids:[...]` |
| Explicit IDs | Use provided `conversation_ids` directly |

Ask one clarifying question if a rep/team name maps to multiple IDs.

## Tool Path

1. Resolve conversation set, usually 5-15 calls.
2. Fetch `get_scorecard_sessions(conversation_ids:[...])`.
3. Fetch `get_conversation_analysis(conversation_ids:[...])` in batches of up to 5.
4. Fetch comments for illustrative conversations.
5. Fetch transcripts only for exact examples or user-requested evidence.
6. Draft coaching agenda with confidence language based on sample size.

## Confidence Language

| Calls reviewed | Language |
|---|---|
| 1-2 | Observation |
| 3-4 | Early pattern |
| 5+ | Pattern |

## Output

```text
## Coaching Agenda
**Scope:** [rep/team/filter] | **Calls reviewed:** [N] | **Period:** [range]

### Strengths
[Evidence-backed strengths]

### Priority Coaching Focus
[Specific behavior, evidence, recommendation]

### Scorecard Patterns
[Returned scorecard data]

### Transcript/Call Examples
[Only if transcript evidence was fetched]

### 1:1 Questions
[Questions grounded in data]
```

## Rules

1. Strengths first.
2. No invented benchmarks.
3. Use transcript evidence sparingly and only when useful.
4. Bound enrichment to avoid rate-limit pressure.
