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
the Exo dashboard → **Settings → Integrations → Connect to Claude Code**, then
restart Claude Code.

> The plugin **supplements** `claude mcp add exo …`; it does not replace it. Use
> **one** of the two — running both registers two `exo` servers that collide. To
> switch, remove the other first (`claude mcp remove exo`).

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

## Local verification (before publishing)

Install from local disk and confirm three surfaces:

1. Add the local marketplace + install the plugin, supplying a real test
   `exo_prod_…` key at the prompt.
2. Verify: the `exo` MCP registered and its tools appear (`tools/list`);
   `/exo-research` and `/exo-setup` show in the command list; the `using-exo`
   skill is listed.
3. **Duplicate-name check:** with the plugin installed AND a prior
   `claude mcp add exo …` present, note whether Claude Code errors on the
   duplicate `exo` server name or silently takes one. Record the result here so
   the "pick one" guidance matches reality.

> ⚠️ Duplicate-name behavior: _to confirm on first local install (O6)._
