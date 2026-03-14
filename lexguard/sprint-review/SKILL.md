---
name: sprint-review
description: Review a sprint's progress — check task statuses, identify blockers, update statuses, and generate a sprint summary report. Requires LexGuard Leads MCP server.
metadata:
  author: lexguard
  version: "1.0.0"
  requires-mcp: lexguard-leads
---

# Sprint Review

Review sprint progress and generate a summary.

## Steps

1. Use **get_sprint** to get sprint details (dates, status).
2. Use **list_sprint_tasks** to get all tasks and their statuses.
3. Categorize tasks by status: backlog, todo, in_progress, review, done.
4. For tasks linked to posts, use **get_post** to check post statuses (copyStatus, uiStatus, scheduleStatus).
5. Generate a sprint report:
   - **Progress**: X/Y tasks done, % complete
   - **By status**: count per column
   - **Blockers**: tasks stuck in backlog/todo with high priority
   - **Post status**: which posts are ready vs need work
   - **Recommendations**: tasks to prioritize, deadlines at risk
6. Ask if the user wants to update any task statuses → use **move_sprint_task** or **update_sprint_task**.
