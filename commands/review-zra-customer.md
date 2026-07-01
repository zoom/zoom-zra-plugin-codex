---
description: Build a Zoom Revenue Accelerator customer, account, or contact context brief.
---

# Review ZRA Customer

Use this command when the user asks about an account, customer, contact, stakeholder, or relationship history.

## Preflight

1. Identify whether the user named an account, company, contact, email, or ID.
2. Confirm desired scope: account profile, stakeholders, active deals, recent conversations, or full customer brief.
3. Confirm whether to include only the current user's visible data or a broader authorized scope.

## Plan

- Use `$zra-customer-context`.
- Resolve account/contact IDs before filtering deals or conversations.
- Label partial visibility.

## Commands

1. Resolve accounts with `get_customer_accounts` or contacts with `get_customer_contacts`.
2. Fetch related contacts for the account with `get_customer_contacts`.
3. Fetch related deals with `search_deals`.
4. Fetch related conversations with `search_conversations`.
5. Optionally fetch `get_conversation_analysis` for recent conversations.
6. Draft a customer context brief with gaps and recommended next actions.

## Verification

1. Confirm the selected account/contact is intended.
2. Separate returned facts from Codex synthesis.
3. Avoid exposing unnecessary personal contact data.

## Summary

```text
## Result
- Action: reviewed ZRA customer context
- Status: success | partial | failed
- Details: account/contact, related deals, related conversations, missing data
```

## Next Steps

- Run `/review-zra-deal` for an active deal.
- Run `/review-zra-conversation` for a recent call.
