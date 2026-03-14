---
name: sprint-planning
description: Plan and set up a new sprint with tasks linked to posts, scheduled date blocks, and Kanban organization. Requires LexGuard Leads MCP server.
metadata:
  author: lexguard
  version: "1.0.0"
  requires-mcp: lexguard-leads
---

# Sprint Planning

Set up a new sprint with tasks, scheduling, and post linking.

## Steps

1. Use **list_sprints** to see existing sprints and avoid duplicates.
2. Use **create_sprint** with a descriptive name, start/end dates, and status "planning".
3. Use **list_campaigns** and **list_posts** to find posts that need work.
4. For each post that needs work, use **create_sprint_task** with:
   - A descriptive title
   - Link to the post via `postId`
   - Priority (low/medium/high)
   - Initial status (backlog or todo)
5. For tasks that need scheduling, use **schedule_sprint_task** with labeled date blocks:
   - "Diseño" — design phase
   - "Desarrollo" — development/production
   - "Revisión" — review phase
   Provide startDate (ISO datetime) and durationHours or endDate.
6. Review with **list_sprint_tasks** and confirm the Kanban layout.
7. When ready, use **update_sprint** to change status to "active".
