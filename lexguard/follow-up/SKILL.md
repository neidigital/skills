---
name: follow-up
description: Draft and send a follow-up message to a marketing lead, optionally using an AI pipeline. Requires LexGuard Leads MCP server.
metadata:
  author: lexguard
  version: "1.0.0"
  requires-mcp: lexguard-leads
---

# Follow Up

Draft and send a follow-up message to a lead.

## Steps

1. Use **search_leads** to find the lead.
2. Call **get_lead** for full context and **get_messages** for recent conversation.
3. Check if there are AI pipelines available with **list_pipelines**.
4. If a pipeline with lead input exists, ask the user whether to:
   - **Run the pipeline** — call **run_pipeline** and show the generated response
   - **Draft manually** — draft a follow-up based on conversation context
5. Once approved, use **send_message** to draft it on the chosen channel.
6. Remind the user that the message is pending confirmation and needs external delivery, then call **confirm_message**.
