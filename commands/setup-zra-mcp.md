---
description: Check and document the app-backed Zoom Revenue Accelerator MCP setup.
---

# Setup ZRA MCP

Use this command when installing or validating the Zoom Revenue Accelerator plugin's app-backed MCP connection.

## Preflight

1. Read [`.app.json`](../.app.json) and confirm the app connector ID is present.
2. Confirm [`.codex-plugin/plugin.json`](../.codex-plugin/plugin.json) points to `.app.json`.
3. Confirm the ZRA MCP endpoint is `https://mcp.zoom.us/mcp/revenue_accelerator/streamable`.
4. Confirm no local `.mcp.json` is being used for this plugin.

## Plan

- Validate plugin metadata and app mapping.
- Explain the OAuth handoff model.
- Stop if the ZRA app connector ID is missing or still uses a placeholder.

## Commands

1. Inspect `.app.json`, `.codex-plugin/plugin.json`, and README setup notes.
2. Verify the plugin name is `zoom-zra` to avoid collision with the original `zoom` plugin.
3. Verify the app connector is expected to own OAuth and token handoff.
4. Confirm the app has the ZRA OAuth scopes listed in [`references/zra-mcp.md`](../references/zra-mcp.md), including customer/contact and user read scopes for the current 15-tool surface.
5. Confirm the plugin uses the current tool names and does not reference removed pre-July 2026 tools.
6. Produce setup findings.

## Verification

1. Check JSON validity.
2. Confirm the endpoint and app ID state.
3. Confirm whether the plugin can be sideloaded and can reach the app-backed MCP path.

## Summary

```text
## Result
- Action: checked ZRA MCP setup
- Status: success | partial | failed
- Details: app ID state, endpoint, OAuth handoff, MCP readiness
```

## Next Steps

- Run `/debug-zra-mcp-auth` if authentication fails after install.
