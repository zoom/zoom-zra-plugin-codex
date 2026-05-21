---
name: summarizer
description: Use when using AI Services Summarizer.
---

# Zoom AI Services Summarizer

Use this skill for Zoom AI Services Summarizer workflows over existing transcript or dialogue text. If the user still needs audio or video transcribed first, route through Scribe before summarization.

## Workflow

1. Confirm the input is transcript text or transcript files, not raw media.
2. Choose fast mode for a bounded inline transcript or batch mode for transcript files stored in S3.
3. Set up Build-platform credentials and JWT handling separately from application summarization logic.
4. Select the summary task: `recap`, `summary`, `action_items`, or `full_summary`.
5. Implement response parsing around `result.text`, then add polling, webhook callbacks, persistence, and retry handling for batch jobs.
6. Debug by checking credential audience, input size, task value, transcript file format, backend timeout, and webhook delivery.

## References

- Full preserved guide: [references/full-guide.md](references/full-guide.md)
- High-level scenarios: [scenarios/high-level-scenarios.md](scenarios/high-level-scenarios.md)
- Fast mode Node example: [examples/fast-mode-node.md](examples/fast-mode-node.md)
- Batch webhook pipeline: [examples/batch-webhook-pipeline.md](examples/batch-webhook-pipeline.md)
- API reference: [references/api-reference.md](references/api-reference.md)
- Environment variables: [references/environment-variables.md](references/environment-variables.md)
- Common drift and breaks: [troubleshooting/common-drift-and-breaks.md](troubleshooting/common-drift-and-breaks.md)
