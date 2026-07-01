# Zoom ZRA

Zoom ZRA is a Codex plugin for Zoom Revenue Accelerator workflows. It connects Codex to the Zoom Revenue Accelerator MCP server through a Zoom Marketplace app connector, then uses returned ZRA data to support sales review, triage, coaching, and drafting workflows in Codex.

The production ZRA MCP endpoint is:

```text
https://mcp.zoom.us/mcp/revenue_accelerator/streamable
```

## Gold Release Status

This plugin is configured for gold release with the Zoom ZRA app connector in [`.app.json`](.app.json), the production ZRA MCP endpoint, and the live 15-tool MCP surface verified on 2026-07-01.

## Plugin Shape

- Plugin manifest: [`.codex-plugin/plugin.json`](.codex-plugin/plugin.json)
- Zoom app connector mapping: [`.app.json`](.app.json)
- Local marketplace metadata: [`.agents/plugins/marketplace.json`](.agents/plugins/marketplace.json)
- ZRA MCP command workflows: [`commands/`](commands/)
- ZRA MCP skills: [`skills/`](skills/)
- Shared ZRA MCP references: [`references/`](references/)
- Branding and screenshots: [`assets/`](assets/)
- Local sideload guide: [`sideload.md`](sideload.md)

This plugin intentionally does not include a local `.mcp.json`. The expected path is app-backed OAuth: the Zoom Marketplace app initiates the OAuth flow, Codex receives the resulting app access token through the connector, and the connector uses that token with the ZRA MCP server.

## What Zoom ZRA Does

Use Zoom ZRA when you want Codex to:

- review a ZRA sales conversation for summary, next steps, objections, coaching signals, and scorecards
- find transcript-backed evidence from a ZRA conversation
- draft a post-call follow-up package with a customer email draft, internal recap, and action items
- build customer, account, contact, stakeholder, deal, and conversation context
- assess a ZRA deal using deal detail, deal activities, and linked conversation insights
- triage visible pipeline or forecast risk from returned deal data
- resolve reps, teams, managers, and managed-team scope for downstream workflows
- prepare a manager coaching agenda from recent ZRA conversation data
- debug app-backed ZRA MCP authentication and endpoint setup

## Capability Boundary

The current live ZRA MCP tool surface used by this plugin is retrieval-oriented. Codex can retrieve ZRA data and synthesize summaries, recommendations, coaching notes, and draft artifacts in the current conversation.

The live ZRA MCP server currently exposes retrieval-oriented tools. This plugin therefore does not send emails, update CRM records, schedule meetings, create external tasks, write comments, delete conversations, modify ZRA records, or perform other external side effects. Any follow-up package is a draft for user review.

## Commands

| Command | Purpose |
|---|---|
| [`/zra-start`](commands/zra-start.md) | Route a ZRA request to the right workflow |
| [`/review-zra-conversation`](commands/review-zra-conversation.md) | Review one ZRA conversation |
| [`/find-zra-transcript-evidence`](commands/find-zra-transcript-evidence.md) | Find transcript-backed evidence in a conversation |
| [`/create-zra-follow-up-package`](commands/create-zra-follow-up-package.md) | Draft an in-chat follow-up package from a ZRA conversation |
| [`/review-zra-customer`](commands/review-zra-customer.md) | Build customer, account, contact, and relationship context |
| [`/review-zra-deal`](commands/review-zra-deal.md) | Assess one ZRA deal |
| [`/triage-zra-pipeline`](commands/triage-zra-pipeline.md) | Triage pipeline or forecast risk |
| [`/prepare-zra-coaching-agenda`](commands/prepare-zra-coaching-agenda.md) | Prepare a 1:1 or coaching agenda from visible conversation data |
| [`/resolve-zra-team-scope`](commands/resolve-zra-team-scope.md) | Resolve reps, teams, managers, and downstream scope filters |
| [`/setup-zra-mcp`](commands/setup-zra-mcp.md) | Check the app-backed ZRA MCP setup |
| [`/debug-zra-mcp-auth`](commands/debug-zra-mcp-auth.md) | Diagnose app OAuth, token handoff, and ZRA MCP auth issues |

## Skills

| Skill | Purpose |
|---|---|
| [`zra-start`](skills/zra-start/SKILL.md) | Classify ZRA requests and load shared tool rules |
| [`zra-conversation-review`](skills/zra-conversation-review/SKILL.md) | Review a sales conversation |
| [`zra-transcript-evidence`](skills/zra-transcript-evidence/SKILL.md) | Find transcript-backed evidence |
| [`zra-follow-up-package`](skills/zra-follow-up-package/SKILL.md) | Draft a post-call follow-up package |
| [`zra-customer-context`](skills/zra-customer-context/SKILL.md) | Build customer/account/contact context |
| [`zra-deal-review`](skills/zra-deal-review/SKILL.md) | Assess deal health and risk |
| [`zra-pipeline-triage`](skills/zra-pipeline-triage/SKILL.md) | Triage a scoped pipeline |
| [`zra-coaching-agenda`](skills/zra-coaching-agenda/SKILL.md) | Prepare coaching or 1:1 agendas from visible conversation data |
| [`zra-team-scope`](skills/zra-team-scope/SKILL.md) | Resolve teams, managers, members, and users for scoped workflows |

## MCP Coverage

The plugin is written around the public Zoom Revenue Accelerator MCP server documented by Zoom. Live `tools/list` testing on 2026-07-01 returned 15 tools covering conversations, transcripts, comments, scorecards, deals, deal activities, deal analysis, stages, indicators, customer accounts, customer contacts, internal users, and team/member scope.

Do not use the removed pre-July 2026 tool names (`list_all_conversations`, `list_all_deals`, old `_by_id` tools, or old team tools). Use the current tools in [`references/zra-mcp.md`](references/zra-mcp.md).

See [`references/zra-mcp.md`](references/zra-mcp.md) and [`references/zra-tool-rules.md`](references/zra-tool-rules.md) before changing tool behavior.

## Example Prompts

```text
Run /review-zra-conversation for my latest call with Acme.
```

```text
Use $zra-deal-review to tell me how the Acme renewal is looking.
```

```text
Run /review-zra-customer for Acme and summarize recent ZRA context.
```

```text
Run /find-zra-transcript-evidence for my last call and find where pricing came up.
```

```text
Run /triage-zra-pipeline for my deals closing this month and flag the ones that need attention.
```

```text
Use $zra-coaching-agenda to prep coaching notes from my recent calls.
```

```text
Run /create-zra-follow-up-package from my last call and draft the customer email, internal recap, and action items in Codex.
```
