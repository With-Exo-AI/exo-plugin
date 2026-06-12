---
description: Deep cross-session briefing on what you already know/decided about a topic, using Exo.
argument-hint: <topic>
---

The user wants a deep briefing on everything Exo knows about: **$ARGUMENTS**

Do this in order, then synthesize — do not answer from your own assumptions:

1. Call `exo_context` with `mode="query"`, `detail="full"`, `question="$ARGUMENTS"`
   to pull the ranked sources, prior decisions, and any contradictions.
2. Call `exo_context` with `mode="evolution"`, `question="$ARGUMENTS"` to see how
   the user's thinking on this changed over time.
3. If the topic names two things to compare, call `exo_context` with
   `mode="compare"` using an "A vs B" question.

Then write a briefing that leads with: what the user has already decided, where
they've contradicted themselves, and what's still unresolved — citing the sources
Exo returned. If Exo returned nothing, say so plainly and ask whether to proceed
without prior context.
