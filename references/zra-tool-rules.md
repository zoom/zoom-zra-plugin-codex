# ZRA MCP Tool Rules

Shared conventions for all skills that compose Zoom Revenue Accelerator MCP Server tools. Every skill in this plugin references this file. When a rule here conflicts with a skill-specific instruction, the skill-specific instruction wins.

## ID Resolution

Always resolve human language into stable IDs before detail tools.

- **Conversations:** Use `search_conversations` with `scope`, filters, and optional `return_conversation_id_only`; pass `conversation_id` to transcript/comment tools and `conversation_ids[]` to batch analysis/scorecard tools.
- **Deals:** Use `search_deals` with `scope`, keyword, stage, account, owner/team filters, or date filters; pass `deal_ids[]` to `get_deal_detail_v2` and one `deal_id` to activity/analysis tools.
- **Customers:** Use `get_customer_accounts` for account IDs and `get_customer_contacts` for contact IDs before filtering deals or conversations by customer.
- **Indicators:** Use `search_indicators` to resolve `indicator_id`.
- **Internal users:** Use `search_internal_users(keyword)` to resolve `user_id` for owner/member/conversation filters.
- **Teams:** Use `get_manager_team_and_member` for managed-team overview or `team_ids[]` detail. Use team filters only when the user's request asks for managed or specific team scope.
- **Ambiguity:** If multiple matches are plausible, present the top candidates and ask for confirmation. Never guess.

## Scope Rules

Use explicit MCP `scope` values. Do not use old sentinels such as `me`, `my_participated`, or old host/participant filters.

| User intent | Tool | Parameter | Value |
|---|---|---|---|
| My conversations | `search_conversations` | `scope` | `MINE` |
| Conversations for my managed team | `search_conversations` | `scope` | `MY_MANAGED` |
| Conversations for specific teams | `search_conversations` | `scope`, `team_ids` | `SPECIFIC`, selected IDs |
| My deals | `search_deals` | `scope` | `MINE` |
| Deals for my managed team | `search_deals` | `scope` | `MY_MANAGED` |
| Deals for specific teams | `search_deals` | `scope`, `team_ids` | `SPECIFIC`, selected IDs |

Use `ALL` only when the user explicitly asks for an account-wide view and the authenticated user is authorized.

## Parameter Rules

- Strip blank optional fields before calling tools. Blank strings can broaden or default results.
- Validate enum values client-side:
- `search_conversations.conversation_type`: `meeting`, `phone`
- `search_conversations.sort_by`: `date_desc`, `date_asc`
- `search_conversations.scope`: `MINE`, `MY_MANAGED`, `SPECIFIC`, `ALL`
- `search_deals.scope`: `MINE`, `MY_MANAGED`, `SPECIFIC`, `ALL`
- `search_deals.sort_by`: `last_activity_desc`, `close_date_asc`, `close_date_desc`, `amount_desc`
- `get_deal_activities_v2.sort_by`: `date_desc`, `date_asc`
- `get_deal_activities_v2.types`: `meeting`, `call`, `email`
- `get_deal_analysis.analysis_type`: `STORY`, `PLAYBOOK`, `ALL`
- Use ISO timestamps with timezone in date range objects, for example `{"from":"2026-07-01T00:00:00Z","to":"2026-07-31T23:59:59Z"}`.
- Use integer `page_size`; do not pass `""` or user text.

## Tool Selection

| Need | Tool path |
|---|---|
| Recent calls | `search_conversations(scope:"MINE", sort_by:"date_desc")` |
| Call analysis | `get_conversation_analysis(conversation_ids:[...])` |
| Transcript evidence | `get_conversation_transcript(conversation_id, keyword?)` |
| Manager comments | `get_conversation_comments(conversation_id)` |
| Scorecards | `get_scorecard_sessions(conversation_ids:[...])` |
| Pipeline/deal search | `search_deals(scope:"MINE", sort_by:"last_activity_desc")` |
| Deal detail | `get_deal_detail_v2(deal_ids:[...])` |
| Deal activities | `get_deal_activities_v2(deal_id, include_content?)` |
| Deal AI analysis | `get_deal_analysis(deal_id, analysis_type)` |
| Stage filters | `get_deal_stages` before using `deal_stages` |
| Customer context | `get_customer_accounts`, then `get_customer_contacts`, then filter deals/conversations |
| User/team scoping | `search_internal_users`, `get_manager_team_and_member` |
| Indicator filters | `search_indicators`, then use `indicator_id` |

## Pagination

- `next_page_token` expires. Use it immediately in the same workflow; do not store it.
- Do not claim "all records" unless every page was retrieved and the response set is explicitly complete.
- If only one page was retrieved, label the analysis as "based on the most recent [N] results" or "sample of [N]."
- Default `page_size`: use 5-20 for search/list calls. Use smaller sizes before enrichment.

## Rate Limits and Enrichment Bounds

- Batch detail calls where supported: `get_conversation_analysis` up to 5 conversation IDs, `get_scorecard_sessions` up to 20 conversation IDs, `get_deal_detail_v2` up to 20 deal IDs.
- Do not enrich every item from a large search result. Enrich top 3-5 deals or conversations unless the user asks for more.
- Transcript calls can be large. Prefer `keyword` for targeted evidence.
- Sequential pagination is required; do not fire multiple page-token requests in parallel.

## Data Integrity Rules

1. Distinguish sources: tool-returned facts, ZRA AI analysis, human comments, transcript evidence, and Codex synthesis.
2. Do not infer unavailable history. If no tool returns close-date change count, trend history, or benchmark data, do not claim it.
3. Do not invent benchmarks. Compare only to returned values, configured scorecards, or the current result set.
4. Handle private or partial objects by reporting only what tools return.
5. Missing data is a signal, not a gap to fill. Say what was empty.
6. Use probability/amount fields only when returned by deal tools or supplied by the user.
7. Treat `result.isError` inside HTTP 200 as a failure.
8. Treat 401 missing-scope errors as app-scope or user-consent issues; name the missing scope when the server provides it.

## Review Before Write

Current ZRA MCP workflows in this plugin are retrieval-oriented.

> Draft only. Show proposed emails, recaps, coaching notes, customer briefs, or next actions in Codex. Do not send, post, create, update, schedule, or modify anything outside the current conversation.

Only claim write/delete/update behavior if the live MCP server exposes a matching tool and this plugin documents how to use it.
