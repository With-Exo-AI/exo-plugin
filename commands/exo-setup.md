---
description: Check the Exo connection and your knowledge-graph status, and fix common issues.
---

Help the user verify their Exo setup is working.

1. Confirm the Exo tools are available (you should see `exo_context`,
   `exo_capture`, …). If they are NOT, the MCP isn't connected — point the user
   to the Exo dashboard → Settings → Integrations → Connect to Claude Code, then
   stop.
2. Call `exo_context` with `mode="graph"` to check whether the graph has content.
   - Has nodes → report node/edge/domain counts and confirm Exo is live.
   - Empty → tell the user their account has no imported content yet and point
     them to the dashboard Import page. An empty graph (not a connection failure)
     is why Exo "isn't returning anything."
3. If any call returns 401/unauthorized → the key is invalid or revoked. Tell the
   user to re-generate it in Settings → Integrations and re-connect with the
   trailing-slash `/mcp/` URL.

End with a short status summary: connected? graph populated? next action.
