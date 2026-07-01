---
description: Find transcript-backed evidence in a Zoom Revenue Accelerator conversation.
---

# Find ZRA Transcript Evidence

Use this command when the user asks where something was said, wants exact language, or wants to search a ZRA transcript by keyword.

## Preflight

1. Identify the target conversation or resolve it with `search_conversations`.
2. Identify the keyword, phrase, topic, objection, or commitment to search.
3. Confirm whether the user wants concise evidence or a broader transcript summary.

## Plan

- Use `$zra-transcript-evidence`.
- Prefer keyword-filtered transcript retrieval.
- Avoid dumping full transcripts.

## Commands

1. Resolve the conversation with `search_conversations`.
2. Call `get_conversation_transcript` with `keyword` when provided.
3. Optionally call `get_conversation_analysis` for context.
4. Return concise speaker-attributed evidence and interpretation.

## Verification

1. Confirm the selected conversation is correct.
2. Separate transcript evidence from ZRA analysis and Codex synthesis.
3. State when no keyword match or transcript evidence is available.

## Summary

```text
## Result
- Action: found ZRA transcript evidence
- Status: success | partial | failed
- Details: conversation, keyword/topic, evidence count, gaps
```

## Next Steps

- Run `/create-zra-follow-up-package` if the evidence should become a follow-up draft.
