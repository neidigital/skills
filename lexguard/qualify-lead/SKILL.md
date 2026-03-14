---
name: qualify-lead
description: Walk through qualifying a marketing lead — search, review context and conversation history, recommend a status update. Requires LexGuard Leads MCP server.
metadata:
  author: lexguard
  version: "1.0.0"
  requires-mcp: lexguard-leads
---

# Qualify Lead

Qualify a lead by reviewing their full context and recommending a status update.

## Steps

1. Use **search_leads** to find leads matching the user's query (name, email, or phone).
2. Pick the best match and call **get_lead** to pull the full context (campaign, sessions, messages, tasks).
3. Review the conversation history with **get_messages** (last 20 messages).
4. Provide a brief qualification summary:
   - Lead source & channel
   - Key conversation topics
   - Current status and recommended next status
   - Any open tasks
5. Ask if the user wants to update the lead status, and if so, call **update_lead** with the new status.
6. Use **get_campaign_statuses** if the lead belongs to a campaign with custom statuses.
