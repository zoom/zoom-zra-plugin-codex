# Contributing to Zoom Revenue Accelerator

This repository contains the `Zoom Revenue Accelerator` Codex plugin. Changes should stay focused on Zoom Revenue Accelerator MCP workflows.

## Scope

Accepted changes should improve one of these surfaces:

- ZRA MCP app-backed OAuth setup and token handoff guidance
- ZRA conversation review workflows
- ZRA transcript evidence workflows
- ZRA customer/account/contact context workflows
- ZRA deal review workflows
- ZRA indicator and related conversation workflows
- ZRA pipeline triage workflows
- ZRA coaching agenda workflows
- ZRA team/user scope resolution workflows
- ZRA follow-up package workflows
- ZRA MCP tool rules, scopes, pagination, and rate-limit behavior

Avoid adding non-ZRA Zoom platform material unless it directly supports ZRA MCP usage.

## Skill Guidelines

Every skill must:

- live under `skills/<skill-name>/SKILL.md`
- use YAML frontmatter with `name` and `description`
- reference [`references/zra-tool-rules.md`](references/zra-tool-rules.md) when it depends on ZRA MCP tool behavior
- resolve human-language requests to stable IDs before detail calls
- distinguish tool-returned facts, ZRA AI analysis, human comments, and agent synthesis
- avoid unsupported benchmarks, hidden historical claims, or fabricated deal risk signals
- use the current ZRA MCP tool names documented in [`references/zra-mcp.md`](references/zra-mcp.md)

## Command Guidelines

Every command in `commands/` must include:

- frontmatter with `description`
- `Preflight`
- `Plan`
- `Commands`
- `Verification`
- `Summary`
- `Next Steps`

Commands should read first, avoid printing secrets, and make it explicit when app auth or MCP access is blocked.

Do not add removed MCP tool names or stale argument shapes. Use `search_conversations`, `search_deals`, `get_deal_detail_v2`, `get_deal_activities_v2`, `get_manager_team_and_member`, and the other current tools documented in [`references/zra-mcp.md`](references/zra-mcp.md).

## Validation

Before a future commit, run:

```bash
jq empty .codex-plugin/plugin.json .app.json .agents/plugins/marketplace.json
python3 /home/dreamtcs/.codex/skills/.system/plugin-creator/scripts/validate_plugin.py .
grep -R -n -i -E "old Zoom plugin wording|copied developer plugin wording" . --exclude-dir=.git
```

The last command should return no stale generic-plugin references except intentional historical notes, if any.
