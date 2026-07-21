# Exo plugin for Claude Code and Codex

Exo is a cross-session cognition layer: hosted MCP tools for personal-history recall,
identity-conditioned context, decisions, capture, and ingestion, plus an agent skill that
teaches Claude Code and Codex when to use them.

This repository intentionally contains two client manifests around the same shared skill:

- `.claude-plugin/plugin.json` configures Claude Code with its user-prompted connection key.
- `.codex-plugin/plugin.json` and `.mcp.json` configure Codex with `EXO_API_KEY`.

The plugin owns MCP registration and agent guidance. The separate `exo-setup` app owns
local transcript discovery, selection, redaction, cold-start import, and continuous sync.
That boundary matters: a hosted plugin cannot read `~/.claude/projects` or
`~/.codex/sessions` from the user's computer.

## Claude Code install

Run inside Claude Code:

```text
/plugin marketplace add With-Exo-AI/exo-plugin
/plugin install exo@exo
```

Claude Code prompts for the Exo connection key and backend URL declared in
`.claude-plugin/plugin.json`.

## Codex install

Save a freshly generated Exo connection key in the environment, then run:

```bash
export EXO_API_KEY=exo_prod_...
codex plugin marketplace add With-Exo-AI/exo-plugin
codex plugin add exo@exo
```

On Windows PowerShell, persist the key with:

```powershell
setx EXO_API_KEY "exo_prod_..."
```

Open a new terminal after `setx`. Restart Codex after installing. The plugin registers the
hosted Exo MCP from `.mcp.json` and exposes the shared `using-exo` skill. Run `/mcp` in
Codex to confirm Exo is available.

Standalone `codex mcp add` remains a compatibility fallback for older Codex builds; the
plugin path is recommended because it packages MCP registration and Exo usage guidance
together.

## Import and continuous sync

Download and open the single `exo-setup` app from the Exo dashboard. It lets the user pick
Claude Code, Codex, or both, then select up to two workspaces total. The same confirmed
selection controls cold-start history import and the one shared background sync daemon.
Codex-only setup does not create or modify anything under `~/.claude`.

## Included components

- Hosted Exo MCP registration for both clients.
- `skills/using-exo/SKILL.md`: full tool and retrieval-mode reference.
- Claude Code `/exo-research` and `/exo-setup` commands.
- Codex automatic command-skill migration for compatible command files.

## Validation

Validate the Codex manifest with the official plugin creator helper:

```powershell
python C:\Users\Aniket\.codex\skills\.system\plugin-creator\scripts\validate_plugin.py .
```

Then smoke-test a local marketplace without touching the real Codex profile by setting a
temporary `CODEX_HOME`, adding this repository as the marketplace, and running
`codex plugin add exo@exo`.