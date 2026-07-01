# Zoom ZRA

This repository contains the `Zoom ZRA` plugin for Codex.

Its purpose is to help users:

- connect Codex to Zoom Revenue Accelerator through an app-backed MCP flow
- review ZRA conversations, transcripts, scorecards, comments, and content analysis
- build customer/account/contact context from returned ZRA data
- assess deal health using ZRA deal detail, deal activities, and linked conversations
- resolve managed-team, team, and internal-user scope for downstream workflows
- triage visible pipeline and forecast risk from returned ZRA deal data
- prepare coaching agendas from recent ZRA conversation data
- draft reviewed post-call follow-up packages in Codex

The live ZRA MCP surface changed on 2026-07-01. Use the current 15 tools documented in `references/zra-mcp.md`; do not reintroduce removed `list_all_*`, old `_by_id`, or old team tools.

Capability boundary: the current live ZRA MCP tools used by this plugin are retrieval-oriented. Do not claim that the plugin sends emails, updates CRM, schedules meetings, creates external tasks, writes comments, deletes conversations, modifies ZRA records, or performs other external side effects unless a write-capable MCP tool is actually exposed and documented.

The plugin uses the Zoom ZRA app connector ID configured in `.app.json`. Keep metadata, screenshots, skills, and command references aligned with the current ZRA MCP surface before future releases.
