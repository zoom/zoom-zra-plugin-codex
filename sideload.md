# Sideload Zoom ZRA

Local sideloading requires the Zoom ZRA app connector mapping in [`.app.json`](.app.json).

Use a marketplace root that contains the plugin under `plugins/zoom-zra/`.

Recommended sideload sandbox:

```text
zoom-zra-sideload/
|-- .agents/plugins/marketplace.json
`-- plugins/
    `-- zoom-zra/
        |-- .codex-plugin/plugin.json
        |-- .app.json
        |-- agents/
        |-- assets/
        |-- commands/
        |-- references/
        |-- skills/
        |-- AGENTS.md
        |-- README.md
        `-- ...
```

Important details:

- `.agents/plugins/marketplace.json` lives at the marketplace root.
- the marketplace entry points at `./plugins/zoom-zra`
- `plugins/zoom-zra/.codex-plugin/plugin.json` is the canonical manifest
- `plugins/zoom-zra/.app.json` maps the Zoom ZRA app connector ID
- this plugin intentionally does not include `.mcp.json`; the app connector owns OAuth and MCP token handoff

Create a clean sideload sandbox from this repo:

```bash
REPO="/path/to/zoom-zra-plugin-codex"
SIDELOAD="/tmp/zoom-zra-sideload"

rm -rf "$SIDELOAD"
mkdir -p "$SIDELOAD/plugins/zoom-zra"

rsync -a \
  --exclude ".git" \
  --exclude ".DS_Store" \
  --exclude "zra-mcp-skills.zip" \
  "$REPO"/ "$SIDELOAD/plugins/zoom-zra"/

mkdir -p "$SIDELOAD/.agents/plugins"
cat > "$SIDELOAD/.agents/plugins/marketplace.json" <<'JSON'
{
  "name": "zoom-zra-local",
  "interface": {
    "displayName": "Zoom ZRA Local"
  },
  "plugins": [
    {
      "name": "zoom-zra",
      "source": {
        "source": "local",
        "path": "./plugins/zoom-zra"
      },
      "policy": {
        "installation": "AVAILABLE",
        "authentication": "ON_INSTALL"
      },
      "category": "Sales"
    }
  ]
}
JSON
```

Then point Codex at the sideload marketplace root and install `Zoom ZRA`.
