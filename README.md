# Exo plugin for Claude Code

Connect Claude Code to your hosted Exo cognition layer — cross-session memory,
identity-conditioned context, capture, and named flows — as an installable
plugin. The plugin bundles the hosted Exo MCP server (Streamable HTTP) and
prompts you for your connection key, so there's no shell command to copy.

## Install

```bash
/plugin marketplace add With-Exo-AI/exo-plugin
/plugin install exo@exo
```

You'll be prompted for your **Exo connection key** (`exo_prod_…`). Generate one in
the Exo dashboard → **Import → Connect Claude Code**, then
restart Claude Code.

> The plugin **supplements** `claude mcp add exo …`; it does not replace it. They
> don't hard-collide — a plugin's MCP server is namespaced (`plugin:exo:exo`), so it
> coexists with a standalone `exo`. But running both gives you **two copies of every
> Exo tool** (`mcp__exo__*` and `mcp__plugin_exo_exo__*`), so use **one**. To switch,
> remove the other first (`claude mcp remove exo`).

## What you get

- The hosted Exo MCP server (`exo_context`, `exo_capture`, `exo_ingest`, …),
  registered over Streamable HTTP with your key.
- `/exo-research <topic>` — a deep cross-session briefing on what you already
  know/decided about a topic.
- `/exo-setup` — check the connection + knowledge-graph status and fix common
  issues.
- The `using-exo` skill — the full reference for which Exo tool/mode to reach for.

## Build notes

Confirmed against the Claude Code plugin reference + real installed plugins
(2026-06-12):

- **`userConfig`** entries use **`sensitive: true`** (masks input, stores in
  secure storage) — *not* `secret`. **`title`** is a required per-field key.
  `type`, `description`, `required`, `default` are as documented.
- **`${user_config.<name>}`** interpolation is valid inside `mcpServers[*].url`
  and `headers` (and hook/monitor commands).
- **Inline `mcpServers`** in `plugin.json` is supported; an HTTP-transport server
  entry is `{"type":"http","url":…,"headers":{…}}` (verified against the shipped
  `linear` plugin's `type:"http"` server).
- **`marketplace.json`** `source: "./"` points at a plugin whose manifest lives at
  `./.claude-plugin/plugin.json` (verified against the shipped `ui-ux-pro-max`
  marketplace). Required top-level keys: `name`, `owner.name`, `plugins[]`.
- **`backend_url`** default is kept in sync with `paths.py::DEFAULT_BASE_URL`.

## Local verification

Verified by a local-disk install (2026-06-12, Claude Code v2.1.x):

1. `/plugin marketplace add C:\path\to\exo-plugin` then `/plugin install exo@exo`,
   supplying a real `exo_prod_…` key at the prompt. ✅
2. The `/exo-research`, `/exo-setup` commands and the `using-exo` skill appear
   immediately; the `plugin:exo:exo` MCP server registers after a Claude Code
   reload/restart (MCP servers load at startup, unlike commands/skills). ✅
3. **Duplicate-name behavior (O6):** plugin MCP servers are namespaced
   (`plugin:exo:exo`), so the plugin's server **coexists** with a standalone
   `claude mcp add exo` — Claude Code does **not** error on the shared name and does
   **not** silently drop one (confirmed: a reload reported both, alongside the same
   pattern already live for `context7` + `plugin:context7:context7`). The only
   downside of running both is duplicate tools — hence the "use one" guidance above.
