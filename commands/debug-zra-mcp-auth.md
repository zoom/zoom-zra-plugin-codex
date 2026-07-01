---
description: Diagnose Zoom Revenue Accelerator MCP app OAuth, token handoff, endpoint, and authorization failures.
---

# Debug ZRA MCP Auth

Use this command when the Zoom Revenue Accelerator plugin installs but cannot authenticate or call the ZRA MCP server.

## Preflight

1. Capture the exact error text and whether it happens during install, OAuth callback, MCP connection, or a tool call.
2. Check whether `.app.json` contains the expected Zoom Revenue Accelerator app connector ID.
3. Confirm the server URL is `https://mcp.zoom.us/mcp/revenue_accelerator/streamable`.
4. Confirm the user has access to Zoom Revenue Accelerator data.

## Plan

- Separate app connector issues from ZRA MCP server authorization issues.
- Verify app ID, OAuth callback, token handoff, scopes, and endpoint.
- Avoid asking for client secrets or raw tokens.

## Commands

1. Inspect plugin and app metadata.
2. Confirm the app ID belongs to the intended Zoom Marketplace app.
3. Verify OAuth completion in the connector flow.
4. Check scopes against [`references/zra-mcp.md`](../references/zra-mcp.md).
5. Check whether the failing tool requires conversation, transcript, deal, activity, comment, scorecard, indicator, customer/contact, user, or team scope.
6. If the failing tool is unknown, compare it with the current live tool list in [`references/zra-mcp.md`](../references/zra-mcp.md).
7. Produce a ranked diagnosis and minimal fix.

## Verification

1. Re-run the failing install or tool path if possible.
2. Confirm whether the fix changed auth state, endpoint state, or only metadata.
3. State any remaining Zoom account, license, or permissions blocker.

## Summary

```text
## Result
- Action: debugged ZRA MCP auth
- Status: success | partial | failed
- Details: failing layer, app ID state, endpoint, scopes, account permissions
```

## Next Steps

- Fix the app connector metadata if app ID or callback configuration is wrong.
- Update app scopes if authorization is missing.
- Escalate account/license access if the app authenticates but ZRA data is unavailable.
