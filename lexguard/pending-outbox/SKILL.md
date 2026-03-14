---
name: pending-outbox
description: Review and manage outbound messages that haven't been confirmed as sent. Requires LexGuard Leads MCP server.
metadata:
  author: lexguard
  version: "1.0.0"
  requires-mcp: lexguard-leads
---

# Pending Outbox

Review all pending outbound messages and confirm them as sent.

## Steps

1. Call **get_pending_messages** to list unconfirmed outbound messages.
2. Present them grouped by lead and channel, showing:
   - Lead name
   - Channel
   - Message content (truncated if long)
   - When it was drafted
3. For each message, the user can:
   - **Confirm** it (call **confirm_message**) if already sent externally
   - **Skip** it for now
4. Show a summary at the end: total pending, confirmed this session, still pending.
