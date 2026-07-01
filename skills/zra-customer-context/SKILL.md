---
name: zra-customer-context
description: >
  Build customer/account/contact context from ZRA customer accounts, contacts,
  related deals, and related conversations. Use when the user asks about an
  account, customer, contact, stakeholder, or relationship history.
---

# Skill: ZRA Customer Context

## Purpose

Produce an account or contact brief grounded in ZRA customer, deal, and conversation data.

## Required Read

[`../../references/zra-tool-rules.md`](../../references/zra-tool-rules.md)

## MCP Tools Used

| Tool | Role |
|---|---|
| `get_customer_accounts` | Search/detail customer accounts |
| `get_customer_contacts` | Search/detail contacts and optional recent deals/meetings/custom data |
| `search_deals` | Deals associated with account or contact context |
| `search_conversations` | Conversations by account/contact/deal |
| `get_conversation_analysis` | Optional conversation insight |

## Tool Path

1. Resolve account or contact:
   - Account name -> `get_customer_accounts(keyword, page_size:5)`.
   - Contact name/email -> `get_customer_contacts(keyword, page_size:5)`.
   - Known IDs -> detail mode with `customer_account_ids` or `customer_contact_ids`.
2. If multiple matches, ask the user to choose.
3. Fetch contacts for the selected account with `get_customer_contacts(customer_account_id, include_recent_deals:true, include_recent_meetings:true)`.
4. Fetch related deals with `search_deals(customer_account_id, scope:"MINE")` unless the user explicitly asks for a broader authorized scope.
5. Fetch related conversations with `search_conversations(scope, customer_account_id or customer_contact_ids)`.
6. Synthesize account profile, stakeholders, recent engagement, deal context, and open questions.

## Output

```text
## Customer Context
**Account/contact:** [name] | **Scope:** [MINE/MY_MANAGED/SPECIFIC/ALL]

### Profile
[Returned account/contact facts]

### Key People
[Contacts and roles returned]

### Active Deals
[Deal summaries from returned data]

### Recent Conversations
[Recent meetings/calls and ZRA analysis if fetched]

### Recommended Next Steps
[Specific, evidence-backed actions]
```

## Rules

1. Do not expose unnecessary personal contact data; include only what supports the user's task.
2. If account/contact lookup returns no results, ask for another name, email, domain, or ID.
3. Do not assume all customer history is visible; label scope and partial data.
