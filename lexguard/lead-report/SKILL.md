---
name: lead-report
description: Generate a comprehensive report for a marketing lead with contact info, conversation summary, tasks, timeline, and recommendations. Requires LexGuard Leads MCP server.
metadata:
  author: lexguard
  version: "1.0.0"
  requires-mcp: lexguard-leads
---

# Lead Report

Generate a full structured report for a lead.

## Steps

1. Use **search_leads** to find the lead.
2. Call **get_lead** for full context.
3. Retrieve all messages with **get_messages**.
4. Present a structured report:
   - **Contact info**: name, email, phone, tags, metadata
   - **Status**: current status, campaign, available statuses
   - **Conversation summary**: key points from messages, grouped by channel/session
   - **Tasks**: open and completed tasks
   - **Timeline**: first contact date, last activity, total messages
   - **Recommendations**: suggested next actions
