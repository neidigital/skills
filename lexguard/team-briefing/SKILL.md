---
name: team-briefing
description: Generate a team briefing note summarizing current sprint progress, active campaigns, pending leads, and upcoming deadlines. Requires LexGuard Leads MCP server.
metadata:
  author: lexguard
  version: "1.0.0"
  requires-mcp: lexguard-leads
---

# Team Briefing

Generate a comprehensive team briefing note.

## Steps

1. Use **list_team_notes** to see existing notes and recent context.
2. Gather data:
   - **list_sprints** → find active sprints → **list_sprint_tasks** for each
   - **list_campaigns** → active campaigns with lead counts
   - **search_leads** with pending=true → leads needing attention
   - **get_pending_messages** → unconfirmed outbound messages
   - **list_ideas** with status "evaluating" → ideas awaiting decision
   - **list_schedules** with status "pending" → upcoming automated workflows
3. Compose a briefing note with sections:
   - **Sprint Status**: tasks by status, % complete, blockers
   - **Campaign Health**: active campaigns, lead counts, recent activity
   - **Action Items**: pending leads, unconfirmed messages, overdue tasks
   - **Upcoming**: scheduled workflows, sprint deadlines
   - **Ideas Pipeline**: ideas in evaluation
4. Use **create_team_note** to post the briefing with authorName "AI Briefing".
