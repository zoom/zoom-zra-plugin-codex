---
name: zra-transcript-evidence
description: >
  Retrieve and use ZRA conversation transcript evidence. Use when the user asks
  where something was said, wants exact language, needs objection evidence, or
  asks to search a transcript by keyword.
---

# Skill: ZRA Transcript Evidence

## Purpose

Find transcript-backed evidence from ZRA conversations without over-fetching or over-quoting.

## Required Read

[`../../references/zra-tool-rules.md`](../../references/zra-tool-rules.md)

## MCP Tools Used

| Tool | Role |
|---|---|
| `search_conversations` | Resolve target conversation |
| `get_conversation_transcript` | Retrieve transcript or keyword-filtered transcript |
| `get_conversation_analysis` | Optional context for topics, next steps, and indicators |

## Tool Path

1. Resolve conversation with `search_conversations` if ID is not provided.
2. If the user gives a topic/phrase, call `get_conversation_transcript(conversation_id, keyword)`.
3. If the user asks for a general transcript summary, fetch analysis first and transcript only if needed.
4. Return concise evidence with speaker attribution when available.

## Output

```text
## Transcript Evidence
**Conversation:** [title/date if available]
**Keyword/topic:** [if used]

### Relevant Moments
[Speaker-attributed snippets or paraphrases]

### Interpretation
[Codex synthesis clearly labeled]

### Gaps
[If keyword did not match or transcript was unavailable]
```

## Rules

1. Prefer keyword-filtered transcript calls for targeted questions.
2. Avoid dumping full transcripts. Summarize and quote only the minimal relevant language.
3. Separate transcript evidence from ZRA AI analysis.
