# Zoom Revenue Accelerator MCP Reference

Public references:

- Zoom MCP registry entry: https://github.com/zoom/mcp-registry/blob/main/zoom-revenue-accelerator/server.json
- Zoom MCP connection guide: https://developers.zoom.us/docs/mcp/servers/connect-to-zoom-mcp-servers/

The 15-tool list below comes from live `tools/list` testing against the production streamable endpoint on 2026-07-01.

## Server

Production streamable endpoint:

```text
https://mcp.zoom.us/mcp/revenue_accelerator/streamable
```

Production SSE endpoint:

```text
https://mcp.zoom.us/mcp/revenue_accelerator/sse
```

This plugin is designed for the streamable endpoint. It does not include `.mcp.json`; the expected model is app-backed OAuth through a Zoom Marketplace app connector.

## App-Backed OAuth Flow

1. User installs or enables the Zoom Revenue Accelerator app connector in Codex.
2. The connector starts the Zoom Marketplace OAuth flow.
3. Zoom returns the authorization result to the app connector callback.
4. Codex receives the resulting app access token through the connector.
5. The connector uses the token with the ZRA MCP server.

Do not ask users to paste OAuth secrets, refresh tokens, or client secrets into Codex. The app connector must own the OAuth app credentials and token handoff.

## Live Tool Surface

Live `tools/list` testing against the production streamable endpoint on 2026-07-01 returned 15 tools. This replaced the earlier 14-tool surface.

| Tool | Purpose | Notes |
|---|---|---|
| `search_conversations` | Search conversations by scope, IDs, customer, contact, deal, indicator, internal user, team, keyword, type, and date range | Required `scope` |
| `get_conversation_analysis` | Batch AI analysis for up to 5 conversation IDs | Omit `analysis_types` for all available analysis |
| `get_conversation_transcript` | Retrieve transcript for one conversation, optionally keyword-filtered | Returns `speaker_defs` and compact `transcript` |
| `get_conversation_comments` | Retrieve comments for one conversation | Returns all comments, no pagination |
| `get_scorecard_sessions` | Retrieve scorecard sessions for up to 20 conversation IDs | Replaces old scorecard-by-id tool |
| `search_deals` | Search deals by scope, owner/team/member IDs, customer account, stage, amount, close date, activity date, keyword, and activity presence | No direct `deal_ids` parameter |
| `get_deal_detail_v2` | Batch deal detail for up to 20 deal IDs | Optional custom fields |
| `get_deal_activities_v2` | Paginated deal activities by deal ID, type, date range, and content inclusion | `types`: `meeting`, `call`, `email` |
| `get_deal_analysis` | AI analysis for one deal | `analysis_type`: `STORY`, `PLAYBOOK`, `ALL` |
| `get_deal_stages` | Account deal stage configuration | Use to validate stage filters |
| `search_indicators` | Search indicator trackers | Use returned `indicator_id` in conversation/activity filters |
| `get_customer_accounts` | Search/detail customer account records | Supports keyword, IDs, and pagination |
| `get_customer_contacts` | Search/detail customer contacts | Supports account/contact filters and recent deal/meeting/custom-data includes |
| `search_internal_users` | Search internal ZRA users by display name or email fragment | Required `keyword`; may return empty |
| `get_manager_team_and_member` | Retrieve managed-team overview or team detail/member lists by team IDs | Replaces old team-manager/member/detail tools |

## Removed Tool Names

Do not use these older tool names in skills or commands:

```text
account_indicators_settings
get_conversation_comments_by_id
get_conversation_content_analysis_by_id
get_conversation_detail
get_conversation_interaction
get_conversation_scorecards_by_id
get_deal_activities
get_deal_detail
get_team_detail
list_all_conversations
list_all_deals
list_team_managers
list_team_members
list_teams
```

## Observed Output Shapes

Live shape testing on 2026-07-01 returned these stable top-level shapes. See [`zra-output-shapes.md`](zra-output-shapes.md) for sanitized field-level shapes.

| Tool | Top-level shape |
|---|---|
| `search_conversations` | `conversations`, `from`, `to`, `next_page_token`, `page_size` |
| `get_conversation_analysis` | `results[]` with `conversation_id` and `analysis` |
| `get_conversation_transcript` | `speaker_defs[]`, `transcript` |
| `get_conversation_comments` | `data_items[]`, `total` |
| `get_scorecard_sessions` | `sessions[]` |
| `search_deals` | `data_items[]`, `last_activity_date_range`, `has_activities`, `sort_by`, `next_page_token`, `page_size` |
| `get_deal_detail_v2` | `data_items[]`, `total` |
| `get_deal_activities_v2` | `data_items[]`, `next_page_token`, `page_size` |
| `get_deal_analysis` | may return an empty object for sampled deals |
| `get_deal_stages` | `stages[]`, `total` |
| `search_indicators` | `data_items[]`, `total` |
| `get_customer_accounts` | `accounts[]`, `next_page_token`, `page_size` |
| `get_customer_contacts` | `contacts[]`, `next_page_token`, `page_size` |
| `search_internal_users` | may return an empty object or result set |
| `get_manager_team_and_member` | `data_items[]`, `total` |

## Command and Skill Coverage

Each current MCP tool has at least one primary command and skill route.

| MCP tool | Primary command | Primary skill |
|---|---|---|
| `search_conversations` | `/review-zra-conversation` | `zra-conversation-review` |
| `get_conversation_analysis` | `/review-zra-conversation` | `zra-conversation-review` |
| `get_conversation_transcript` | `/find-zra-transcript-evidence` | `zra-transcript-evidence` |
| `get_conversation_comments` | `/review-zra-conversation` | `zra-conversation-review` |
| `get_scorecard_sessions` | `/prepare-zra-coaching-agenda` | `zra-coaching-agenda` |
| `search_deals` | `/triage-zra-pipeline` | `zra-pipeline-triage` |
| `get_deal_detail_v2` | `/review-zra-deal` | `zra-deal-review` |
| `get_deal_activities_v2` | `/review-zra-deal` | `zra-deal-review` |
| `get_deal_analysis` | `/review-zra-deal` | `zra-deal-review` |
| `get_deal_stages` | `/triage-zra-pipeline` | `zra-pipeline-triage` |
| `search_indicators` | `/review-zra-indicator` | `zra-indicator-review` |
| `get_customer_accounts` | `/review-zra-customer` | `zra-customer-context` |
| `get_customer_contacts` | `/review-zra-customer` | `zra-customer-context` |
| `search_internal_users` | `/resolve-zra-team-scope` | `zra-team-scope` |
| `get_manager_team_and_member` | `/resolve-zra-team-scope` | `zra-team-scope` |

## OAuth App Scopes

The Zoom Marketplace app connected to this plugin should include the ZRA scopes needed by the live MCP tool surface. Some configured app scopes are write, update, or delete scopes, but the current MCP tools exposed to this plugin are retrieval-oriented; the presence of those OAuth scopes does not mean this plugin can write back to ZRA.

```text
zra:write:conversation_comment
zra:delete:conversations
zra:delete:conversation_comment
zra:delete:deal_activity
zra:read:conversation_crm_associations
zra:update:conversation_comment
zra:update:conversation_host
zra:read:conversation_analysis
zra:read:list_conversation_comments
zra:read:conversation_participants
zra:read:conversation_scorecards
zra:read:deal
zra:read:list_deal_activities
zra:read:list_conversations
zra:read:conversations
zra:read:list_deals
zra:read:indicator
zra:read:crm_customer_contact
zra:read:user
```

Keep the app scope list aligned with the live MCP tool surface and the Zoom Marketplace app configuration.

## Capability Boundary

The live ZRA MCP tools used by this plugin are retrieval-oriented. Skills may synthesize recommendations, coaching notes, risk flags, customer-context briefs, transcript evidence, and follow-up drafts in Codex, but they must not claim that the plugin sends emails, updates CRM records, schedules meetings, writes comments, deletes conversations, modifies ZRA records, or creates external tasks.

Use the live MCP `tools/list` result, missing-scope errors, and the official MCP page as the source of truth if the tool list or scopes change.

## Tool Rules

All workflows should also follow [`zra-tool-rules.md`](zra-tool-rules.md) and use [`zra-output-shapes.md`](zra-output-shapes.md) when field names matter.
