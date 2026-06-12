---
name: using-exo
description: Use when working with a user who has the Exo MCP connected — the full reference for which Exo tool and mode to use for personal-history recall, identity-conditioned reasoning, capture, ingest, and decisions. Load when you need more than the always-on Exo protocol.
---

# Using Exo

Exo is the user's personal cognition layer: a cross-session knowledge graph of
their content, decisions, expertise, and how they think. The always-on server
instructions cover the four reflexes; this skill is the full reference for *which
tool and mode* to reach for.

## The four reflexes (recap)

1. **Recall before answering** anything touching the user's history, decisions,
   strategy, preferences, or "my/our/we/earlier/previously" → `exo_context`.
2. **Check before you ask** a clarifying question → `exo_context` (`query`/`identity`);
   answer from the user's model if Exo resolves it.
3. **Capture after substance** (a decision, insight, new fact) → `exo_capture`.
4. **Ingest shared content** ("remember/save/note this", pasted docs) → `exo_ingest`.

If Exo returns nothing (new/sparse account), proceed normally — never block.

## Tool catalog

| Tool | Use it for |
|------|-----------|
| `exo_context` | The primary retrieval tool — personal context, decisions, contradictions, identity, graph, insights, temporal/evolution. See modes below. |
| `exo_scaffold_prompt` | Build an identity-aware prompt scaffold (focal nodes) before a big generation task. |
| `exo_capture` | Record a conversation turn / decision into the KG. |
| `exo_ingest` | Save pasted content or a conclusion the user wants kept. |
| `exo_ingest_codebase` | Ingest a project's source into the KG. |
| `exo_decide` | Decide between options using evidence from the user's KG. |
| `exo_edit` | Edit, correct, or forget a memory. |
| `exo_configure` | Adjust how Exo personalizes responses. |
| `exo_review` | Demo/review dispatcher (Captain-PR demo flows). |
| `exo_purge` | Destructive — wipe KG layers. Confirm with the user first. |

## `exo_context` modes

| mode | Returns |
|------|---------|
| `query` (default) | Full retrieval pipeline — ranked sources + contradictions. |
| `identity` | The user's cognitive profile (how they think). |
| `graph` | Knowledge-graph structure (node/edge/domain counts). |
| `insights` | Daemon discoveries — concepts, causal chains. |
| `temporal` | Time-based phases and belief evolution. |
| `evolution` | How thinking about the query topic changed over time. |
| `explain` | Deep-dive: retrieval + evolution for one topic. |
| `compare` | Compare two concepts (phrase the question "A vs B"). |
| `brain` | ExoBrain glass-box: focal nodes, gate confidence, contradictions. |

`detail` levels: `lean` (~50 tokens, key facts), `standard` (~200–500), `full`
(~1000+, graph paths + evidence). Use `lean` for a quick check, `full` for a
deep briefing. `temporal_window` accepts ranges like `"2025-Q4..2026-Q1"`.

## Which tool for which intent (decision tree)

- "What did we decide / what do I think about X?" → `exo_context` `query`.
- "How would the user approach this / what's their style?" → `exo_context` `identity`.
- "How did my view on X change?" → `exo_context` `evolution` (or `explain`).
- "Help me choose between A and B" → `exo_decide` (or `exo_context` `compare`).
- User shares a doc / "remember this" → `exo_ingest`.
- A decision was just made → `exo_capture`.

## Troubleshooting

- **Exo returns nothing** → the account has no imported content yet. Point the
  user to the dashboard Import page; it is not a connection failure.
- **401 / tools missing** → the connection key is invalid/revoked, or the URL
  lacked the trailing slash `/mcp/`. Re-generate the key in the Exo dashboard →
  Settings → Integrations → Connect to Claude Code and re-connect (use the
  trailing-slash `/mcp/` URL).
