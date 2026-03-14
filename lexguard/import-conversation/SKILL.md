---
name: import-conversation
description: Parse and import a pasted chat export (WhatsApp, Telegram, plain text) into a lead's conversation history. Requires LexGuard Leads MCP server.
metadata:
  author: lexguard
  version: "1.0.0"
  requires-mcp: lexguard-leads
---

# Import Conversation

Import a chat export into a lead's conversation history.

## Steps

1. Search for the lead with **search_leads**, or ask the user to create one with **create_lead**.
2. Ask the user to paste the chat export (WhatsApp, Telegram, or plain text format).
3. Parse the messages — extract direction (inbound/outbound), content, and timestamps.
   - WhatsApp format: `[DD/MM/YYYY, HH:MM:SS] Name: message`
   - Telegram format: various, infer from context
   - Plain text: ask the user to clarify who is who
4. Show a preview: number of messages, date range, and a sample.
5. Once confirmed, call **import_messages** with the parsed messages and the appropriate channel.
6. Show a summary of the import result.
