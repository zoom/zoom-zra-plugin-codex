# Command Conventions

Every slash command in this plugin is ZRA MCP-specific and follows the same operational shape.

## Required Sections

Each user-facing command file must include:

- `Preflight`
- `Plan`
- `Commands`
- `Verification`
- `Summary`
- `Next Steps`

## Rules

- Use the app-backed ZRA MCP path; do not instruct users to configure `.mcp.json` for this plugin.
- Treat `https://mcp.zoom.us/mcp/revenue_accelerator/streamable` as the production streamable endpoint.
- Never print OAuth access tokens, refresh tokens, app secrets, or private conversation content beyond what the user asked to use.
- Resolve human-language names into IDs before detail calls.
- Use current `scope` filters exactly as documented in [`../references/zra-tool-rules.md`](../references/zra-tool-rules.md).
- Do not call removed tools such as `list_all_conversations`, `list_all_deals`, `get_deal_detail`, or `get_conversation_content_analysis_by_id`.
- Bound enrichment calls to avoid rate-limit pressure.
- Distinguish tool facts, ZRA AI analysis, human comments, and Codex synthesis.
- Produce drafts and recommendations in Codex only; do not send, post, update CRM, create external records, modify ZRA, or imply any external side effect.
