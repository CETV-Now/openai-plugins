# CETV Now — OpenAI plugins (ChatGPT & Codex)

OpenAI-format plugins published by CETV Now. The Claude version lives separately in [CETV-Now/claude-plugins](https://github.com/CETV-Now/claude-plugins); both connect to the same CETV MCP server (`https://mcp.cetvnow.com/mcp`) and the same advertiser account.

| Plugin | Description |
|---|---|
| [CETV Campaigns](plugins/cetv/README.md) (`cetv`) | Create and track CETV screen-advertising campaigns from ChatGPT or Codex. |

## Install

**Codex**

```
codex plugin marketplace add CETV-Now/openai-plugins
```

then install **CETV Campaigns** from the plugin browser.

**ChatGPT** — during testing, via Developer mode (Settings → Security and login → Developer mode, then add the MCP server `https://mcp.cetvnow.com/mcp` under Plugins). After directory approval it will be listed in ChatGPT's plugin directory.

## Layout

```
.agents/plugins/marketplace.json   ← marketplace "cetv-now" (Codex: `codex plugin marketplace add`)
plugins/cetv/
├── plugin.json      ← Agent Plugins manifest + extensions["com.openai"].interface (name, logo, brand color, prompts)
├── mcp.json         ← remote server "cetv" (streamable-http → https://mcp.cetvnow.com/mcp)
├── skills/          ← new-campaign, campaign-report (same SKILL.md format as the Claude plugin, neutral wording)
└── assets/          ← logo.png (512×512), icon.png (96×96)
```

## Development

- Bump `version` in `plugins/cetv/plugin.json` on every change.
- Server-side tool changes deploy in `mammoth-campaigns-service` and need no plugin release.
- Never rename the plugin (`cetv`) or the marketplace (`cetv-now`).
- `.app.json` (the ChatGPT app mapping, `plugin_asdk_app…`) is added once the MCP server is registered in ChatGPT Developer mode.
