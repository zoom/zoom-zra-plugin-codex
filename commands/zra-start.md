---
description: Route a Zoom Revenue Accelerator request to the right ZRA MCP workflow.
---

# ZRA Start

Use this command when the user asks a broad or ambiguous ZRA question involving conversations, transcripts, deals, pipeline, customers, contacts, indicators, teams, coaching, or follow-up drafts.

## Preflight

1. Confirm the request is about Zoom Revenue Accelerator data.
2. Identify the likely object: conversation, transcript, deal, pipeline, account/contact, indicator, team/user scope, coaching, or follow-up artifact.
3. Note whether app-backed MCP access is required.

## Plan

- Load `$zra-start` and [`references/zra-tool-rules.md`](../references/zra-tool-rules.md).
- Route to one primary workflow.
- Ask one clarifying question only if the target object or workflow is ambiguous.

## Commands

1. Route conversation review to `/review-zra-conversation`.
2. Route transcript evidence to `/find-zra-transcript-evidence`.
3. Route follow-up artifacts to `/create-zra-follow-up-package`.
4. Route deal health checks to `/review-zra-deal`.
5. Route pipeline or forecast risk to `/triage-zra-pipeline`.
6. Route coaching or 1:1 prep to `/prepare-zra-coaching-agenda`.
7. Route indicator, objection, competitor, or tracked-topic analysis to `/review-zra-indicator`.
8. Route customer/account/contact context to `/review-zra-customer`.
9. Route rep/team/manager scoping to `/resolve-zra-team-scope`.
10. Route auth or connector setup issues to `/setup-zra-mcp` or `/debug-zra-mcp-auth`.

## Verification

1. Confirm the chosen route matches the user's requested outcome.
2. Confirm needed IDs can be resolved through current ZRA tools.
3. State any blocker, especially missing app connector authentication or missing scope.

## Summary

```text
## Result
- Action: routed a ZRA request
- Status: success | partial | failed
- Details: selected workflow, required ZRA object, blocker if any
```

## Next Steps

- Run the selected ZRA command.
- If auth is blocked, run `/setup-zra-mcp`.
