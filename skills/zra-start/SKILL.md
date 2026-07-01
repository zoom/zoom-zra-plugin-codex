---
name: zra-start
description: >
  Route Zoom Revenue Accelerator requests to the right skill. Use when the user asks
  about ZRA conversations, transcripts, deals, pipeline, customers, contacts,
  indicators, team scope, coaching, or post-call draft artifacts.
---

# Skill: ZRA Start

## Purpose

Classify a ZRA request, load the shared tool rules, and route to one primary workflow.

## Required Read

Load [`../../references/zra-tool-rules.md`](../../references/zra-tool-rules.md) before routing.

## Routing

| User intent | Route to |
|---|---|
| Understand one call or meeting | `zra-conversation-review` |
| Find exact transcript evidence or quotes | `zra-transcript-evidence` |
| Draft post-call follow-up material | `zra-follow-up-package` |
| Review one deal | `zra-deal-review` |
| Review pipeline or forecast risk | `zra-pipeline-triage` |
| Prepare coaching or 1:1 notes | `zra-coaching-agenda` |
| Review tracked indicators, objections, competitors, or recurring themes | `zra-indicator-review` |
| Understand an account, contact, or customer relationship | `zra-customer-context` |
| Resolve reps, teams, managed scope, or team-specific filters | `zra-team-scope` |
| Auth/setup failures | `/setup-zra-mcp` or `/debug-zra-mcp-auth` |

## Disambiguation

- "Review" + conversation/call/meeting -> `zra-conversation-review`.
- "Show where they said..." or "find transcript evidence" -> `zra-transcript-evidence`.
- "Follow up", "email", "recap", or "action items" after a call -> `zra-follow-up-package`.
- "How is this deal?" -> `zra-deal-review`.
- "At-risk deals", "forecast", "pipeline" -> `zra-pipeline-triage`.
- "Coach", "1:1", "call quality across calls" -> `zra-coaching-agenda`.
- "Indicator", "objection", "competitor", "tracked topic", or "where did this theme show up?" -> `zra-indicator-review`.
- "Customer/account/contact context" -> `zra-customer-context`.
- "Team", "rep", "manager", "managed team", "scope" -> `zra-team-scope`.

Ask one clarifying question when the target object or desired artifact is ambiguous. Do not guess IDs.

## Current Tool Surface

Use the live 15-tool ZRA MCP surface documented in [`../../references/zra-mcp.md`](../../references/zra-mcp.md). Do not call removed tools such as `list_all_conversations`, `list_all_deals`, `get_deal_detail`, or `get_conversation_content_analysis_by_id`.

## Shared Rules

1. Resolve names to IDs before detail calls.
2. Use `scope` values (`MINE`, `MY_MANAGED`, `SPECIFIC`, `ALL`) instead of old sentinel filters.
3. Strip blank optional fields and validate enums before tool calls.
4. Paginate responsibly and label partial results.
5. Bound enrichment to avoid rate-limit pressure.
6. Distinguish tool facts, ZRA AI analysis, comments, transcript evidence, and Codex synthesis.
7. Never fabricate data, benchmarks, or history the tools do not return.
