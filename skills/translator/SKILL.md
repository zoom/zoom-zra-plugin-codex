---
name: translator
description: Use when using AI Services Translator.
---

# Zoom AI Services Translator

Use this skill for Zoom AI Services Translator workflows over plain text or stored text files. If the user needs transcript generation or summarization first, chain Scribe and Summarizer before translation.

## Workflow

1. Confirm the input text source, source language if known, target locale, latency needs, and whether fast mode or batch mode fits.
2. Choose fast mode for bounded inline text or batch mode for text files stored in S3.
3. Set up Build-platform credentials and JWT handling separately from application translation logic.
4. Keep each request or job to one target language; submit separate requests for multiple target languages.
5. Implement response parsing around `result.translations`, then add polling, webhook callbacks, persistence, and retry handling for batch jobs.
6. Debug by checking credential audience, source/target language support, English-pivot expectations, input format, backend timeout, and webhook delivery.

## References

- Full preserved guide: [references/full-guide.md](references/full-guide.md)
- High-level scenarios: [scenarios/high-level-scenarios.md](scenarios/high-level-scenarios.md)
- Fast mode Node example: [examples/fast-mode-node.md](examples/fast-mode-node.md)
- Batch webhook pipeline: [examples/batch-webhook-pipeline.md](examples/batch-webhook-pipeline.md)
- API reference: [references/api-reference.md](references/api-reference.md)
- Environment variables: [references/environment-variables.md](references/environment-variables.md)
- Common drift and breaks: [troubleshooting/common-drift-and-breaks.md](troubleshooting/common-drift-and-breaks.md)
