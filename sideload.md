# Sideload Zoom ZRA

Local sideloading requires the Zoom ZRA app connector mapping in [`.app.json`](.app.json).

Use a marketplace root that contains the plugin under `plugins/zoom-zra/`. The recommended
developer flow is to make the plugin visible in Codex, but leave it uninstalled so the user can
click **Install** in the Codex app. That click is what starts the app connector OAuth flow.

Do not run `codex plugin add zoom-zra@zoom-zra-local` for this flow unless you intentionally want
to install from the CLI instead of letting the user click **Install**.

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
- the marketplace policy uses `"installation": "AVAILABLE"` and `"authentication": "ON_INSTALL"`
- the expected final CLI status before handoff is `zoom-zra@zoom-zra-local  not installed`
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

Register the sideload marketplace with Codex:

```bash
codex plugin marketplace add "$SIDELOAD"
```

If `zoom-zra-local` is already configured but points at an old sandbox, replace that marketplace
registration:

```bash
codex plugin marketplace remove zoom-zra-local
codex plugin marketplace add "$SIDELOAD"
```

Remove any previous local install so the next user sees the **Install** button:

```bash
codex plugin remove zoom-zra@zoom-zra-local || true
rm -rf "$HOME/.codex/plugins/cache/zoom-zra-local/zoom-zra"
rmdir "$HOME/.codex/plugins/cache/zoom-zra-local" 2>/dev/null || true
```

Verify that the marketplace is registered and the plugin is available but not installed:

```bash
codex plugin marketplace list
codex plugin list
```

The relevant lines should look like this:

```text
MARKETPLACE      ROOT
zoom-zra-local   /path/to/zoom-zra-sideload

zoom-zra@zoom-zra-local  not installed  /path/to/zoom-zra-sideload/plugins/zoom-zra
```

At this point, stop using the CLI. Open the Zoom ZRA plugin in the Codex app and have the user click
**Install** manually. Because the marketplace policy is `ON_INSTALL` and `.app.json` maps the Zoom
ZRA app connector, the manual install should trigger the OAuth flow.

If the app does not prompt for OAuth, the active Codex or ChatGPT user may already have an existing
Zoom Revenue Accelerator app connection. Disconnect that app connection in the app settings, then
return to the plugin page and click **Install** again.
