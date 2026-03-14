---
name: ideas-triage
description: Triage and organize the Ideas Board — review backlog, evaluate ideas, set priorities, and link approved ideas to sprints. Requires LexGuard Leads MCP server.
metadata:
  author: lexguard
  version: "1.0.0"
  requires-mcp: lexguard-leads
---

# Ideas Triage

Review and organize the strategic ideas backlog.

## Steps

1. Use **list_ideas** with status "backlog" to get unreviewed ideas.
2. For each idea, use **get_idea** for full details.
3. Check related context:
   - Use **list_project_groups** and **get_project_info** for brand context
   - Use **list_campaigns** to see if similar campaigns exist
   - Use **list_team_notes** for current team priorities
4. For each idea, recommend:
   - **Priority**: low/medium/high based on impact and effort
   - **Status**: evaluating, planned, or rejected with reasoning
5. Update ideas with **update_idea** (status, priority, category).
6. For ideas approved as "planned":
   - Find or create a sprint with **list_sprints** / **create_sprint**
   - Create a sprint task with **create_sprint_task**
   - Link the idea with **link_idea_to_sprint**
7. Show summary: total reviewed, planned, rejected, still evaluating.
