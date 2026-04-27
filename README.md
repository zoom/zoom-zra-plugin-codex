# Zoom

Zoom is a Codex connector plugin that gives Codex access to live Zoom meeting context through the Zoom app connector.

## What This Plugin Includes

- plugin manifest: [`.codex-plugin/plugin.json`](.codex-plugin/plugin.json)
- local marketplace metadata: [`.agents/plugins/marketplace.json`](.agents/plugins/marketplace.json)
- Zoom app mapping: [`.app.json`](.app.json)
- branding assets: [`assets/`](assets/)

This plugin contains the Zoom app mapping, connector metadata, and branding assets.

## What Zoom Does In Codex

Use `Zoom` when you want Codex to work with live Zoom context, such as:

- searching meetings by topic, attendee, or content
- retrieving summaries, transcripts, recordings, and related meeting assets
- pulling meeting context into a coding or follow-up workflow
- working with Zoom data through the authenticated app connector

## Local / Sideload Testing

For local sideload testing, use a marketplace root that contains the plugin under `plugins/zoom/`.

Recommended sideload sandbox:

```text
zoom-sideload/
├── .agents/plugins/marketplace.json
└── plugins/
    └── zoom/
        ├── .codex-plugin/plugin.json
        ├── plugin.json
        ├── .app.json
        ├── assets/
        ├── AGENTS.md
        └── README.md
```

Important details:

- `.agents/plugins/marketplace.json` lives at the marketplace root.
- the marketplace entry points at `./plugins/zoom`
- `plugins/zoom/.codex-plugin/plugin.json` is the canonical manifest
- `plugins/zoom/plugin.json` can be used as a compatibility copy for installers that probe the plugin root
- `plugins/zoom/.app.json` enables the Zoom app connector

Create a clean sideload sandbox from this repo:

```bash
REPO="/path/to/zoom-plugin-codex"
SIDELOAD="/tmp/zoom-sideload"

rm -rf "$SIDELOAD"
mkdir -p "$SIDELOAD/plugins/zoom"

rsync -a \
  --exclude ".git" \
  --exclude ".DS_Store" \
  --exclude "plugins" \
  "$REPO"/ "$SIDELOAD/plugins/zoom"/

mkdir -p "$SIDELOAD/.agents/plugins"
cat > "$SIDELOAD/.agents/plugins/marketplace.json" <<'JSON'
{
  "name": "zoom-local",
  "interface": {
    "displayName": "Zoom Local"
  },
  "plugins": [
    {
      "name": "zoom",
      "source": {
        "source": "local",
        "path": "./plugins/zoom"
      },
      "policy": {
        "installation": "AVAILABLE",
        "authentication": "ON_INSTALL"
      },
      "category": "Productivity"
    }
  ]
}
JSON

cp \
  "$SIDELOAD/plugins/zoom/.codex-plugin/plugin.json" \
  "$SIDELOAD/plugins/zoom/plugin.json"
```

Then point Codex at the sideload marketplace root and install `Zoom`.

## Using In Codex

1. Install the plugin from `/plugins`.
2. Authenticate the Zoom app connector when Codex prompts for it.
3. Mention `@Zoom` or use natural language requests that need live Zoom meeting context.

The Zoom app connector auth is managed by Codex. It is not exposed as a shell environment variable or raw bearer token.

## Example Prompts

```text
Search my recent Zoom meetings for the discussion about pricing.
```

```text
Find the meeting where we discussed the SDK migration and pull the summary.
```

```text
Get the transcript and recording link for yesterday's engineering sync.
```

```text
Look across my Zoom meetings and identify the action items assigned to me.
```

## Related Plugin

Use this plugin when Codex needs authenticated Zoom meeting context.
