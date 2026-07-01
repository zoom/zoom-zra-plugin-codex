---
name: zra-pipeline-triage
description: >
  Triage visible ZRA deals using search_deals, deal stages, deal detail v2, and
  deal activities. Use for pipeline health, forecast prep, at-risk deals, and
  where-to-focus questions.
---

# Skill: ZRA Pipeline Triage

## Purpose

Inspect a scoped set of returned deals, identify risk, and recommend where to focus.

## Required Read

[`../../references/zra-tool-rules.md`](../../references/zra-tool-rules.md)

## MCP Tools Used

| Tool | Role |
|---|---|
| `search_deals` | Pull scoped deal set |
| `get_deal_stages` | Validate stage filters |
| `get_deal_detail_v2` | Enrich flagged/high-value deals |
| `get_deal_activities_v2` | Activity evidence for selected deals |
| `get_deal_analysis` | Optional AI analysis for selected deals |
| `get_manager_team_and_member` / `search_internal_users` | Optional team/user scope |

## Tool Path

1. Resolve scope:
   - My pipeline -> `search_deals(scope:"MINE", page_size:20)`.
   - Managed team -> `search_deals(scope:"MY_MANAGED", page_size:20)`.
   - Specific teams/users -> use `zra-team-scope`, then `scope:"SPECIFIC"` and `team_ids` or user filters.
2. If stage filters are requested, call `get_deal_stages` first and use exact stage names.
3. Run `search_deals` with date/amount/stage/activity filters.
4. Compute returned-data-only snapshot: count, visible amount, stage mix, close-date pressure.
5. Enrich top 3-5 flagged/high-value deals with `get_deal_detail_v2` and `get_deal_activities_v2`.

## Risk Rules

- Past-due close date -> red.
- Close date within 14 days and low/stale activity -> yellow/red depending on evidence.
- Low probability or early stage for high-value deals -> yellow.
- Missing amount/activity fields -> data gap, not inferred risk.
- Do not compute trend or weighted pipeline unless returned data supports it.

## Output

```text
## Pipeline Triage
**Scope:** [scope] | **Deals reviewed:** [N] | **Partial page:** [yes/no]

### Snapshot
[stage mix, visible value, close-date pressure]

### At-Risk Deals
| Deal | Stage | Amount | Close date | Flag | Evidence |

### Recommended Focus
[Prioritized actions]

### Drill-Down Prompts
[Review deal / review conversation options]
```

## Rules

1. Label partial pages.
2. Do not enrich more than 3-5 deals unless asked.
3. Every risk flag cites a returned field or activity.
