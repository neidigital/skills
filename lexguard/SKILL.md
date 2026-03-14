---
name: lexguard
description: >
  Complete reference for operating the LexGuard marketing platform via MCP tools.
  Covers every tool category: leads & CRM, messaging, campaigns, posts & media,
  sprints & scheduling, ideas board, team notes, data stores & jobs, AI pipelines,
  project groups & info, and workflow scheduling. Use this skill whenever working
  with LexGuard data — managing leads, creating campaigns, planning sprints,
  building pipelines, setting up data stores, reviewing outbox messages, generating
  team briefings, or any task that touches the LexGuard MCP server.
metadata:
  author: lexguard
  version: "2.0.0"
  requires-mcp: lexguard-leads
---

# LexGuard Platform Guide

This skill is your complete reference for operating LexGuard through its MCP tools. LexGuard is an AI-native marketing automation platform where small business teams manage leads, campaigns, content, sprints, and AI pipelines — all in Spanish.

Every tool below belongs to the LexGuard Leads MCP server. When you see a tool name in **bold**, that's the exact MCP tool to call.

---

## 1. Leads & CRM

Leads are the core entity — every person who interacts with the business is a lead with a status, channel history, and linked campaign.

| Tool | Purpose |
|------|---------|
| **search_leads** | Find leads by name, email, phone, or tags. Supports `pending=true` for leads needing attention. |
| **get_lead** | Full lead context: campaign, sessions, messages, tasks, metadata. |
| **create_lead** | Create a new lead with name, channel, and optional campaign. |
| **update_lead** | Change status, tags, metadata, or campaign assignment. |
| **find_lead_by_channel** | Look up a lead by their channel identifier (phone number, email, etc.). |
| **merge_leads** | Merge duplicate leads into one record. |
| **link_channel** | Connect an additional channel (WhatsApp, Telegram, email) to an existing lead. |

### Qualifying a lead

1. **search_leads** → find the lead
2. **get_lead** → pull full context (campaign, sessions, messages, tasks)
3. **get_messages** → review recent conversation (last 20 messages)
4. Summarize: source, channel, key topics, current status, recommended next status, open tasks
5. If updating: use **get_campaign_statuses** for custom statuses, then **update_lead**

### Generating a lead report

1. **search_leads** → **get_lead** → **get_messages**
2. Structure: contact info, status, conversation summary (grouped by channel/session), tasks, timeline (first contact, last activity, total messages), recommendations

---

## 2. Messaging

Messages flow through a two-step process: draft → confirm. This ensures a human reviews every outbound message before it actually gets delivered.

| Tool | Purpose |
|------|---------|
| **get_messages** | Retrieve conversation history for a lead (supports limit parameter). |
| **send_message** | Draft an outbound message on a specific channel. Message stays pending until confirmed. |
| **confirm_message** | Mark a pending message as confirmed/sent. |
| **get_pending_messages** | List all unconfirmed outbound messages across all leads. |
| **import_messages** | Bulk-import messages from a chat export (WhatsApp, Telegram, plain text). |

### Following up with a lead

1. **search_leads** → **get_lead** → **get_messages** (recent conversation)
2. Check for AI pipelines with **list_pipelines** — if one matches, offer to **run_pipeline** for an auto-generated response
3. Otherwise, draft manually based on conversation context
4. **send_message** to draft on the chosen channel
5. Remind the user the message is pending, then **confirm_message** when ready

### Importing a conversation

1. **search_leads** or **create_lead** to identify/create the lead
2. User pastes chat export — parse messages extracting direction, content, timestamps
3. WhatsApp format: `[DD/MM/YYYY, HH:MM:SS] Name: message`
4. Show preview: # messages, date range, sample
5. **import_messages** with parsed array and channel name

### Reviewing the outbox

1. **get_pending_messages** → list all unconfirmed messages
2. Group by lead and channel, show content and draft time
3. For each: user can **confirm_message** or skip
4. Show summary: total pending, confirmed this session, still pending

---

## 3. Campaigns

Campaigns are the organizational unit for marketing content. Each campaign has custom statuses for its sales pipeline, and custom postsColumns for categorizing posts.

| Tool | Purpose |
|------|---------|
| **list_campaigns** | List all campaigns with their IDs and descriptions. |
| **get_campaign** | Full campaign detail including postsColumns and lead statuses. |
| **create_campaign** | Create a new campaign with name, description, and optional project group link. |
| **update_campaign** | Modify postsColumns, custom statuses, AI instructions, tone settings. |
| **delete_campaign** | Remove a campaign. |
| **get_campaign_statuses** | Get the custom lead statuses defined for a campaign. |

### Setting up postsColumns

postsColumns define the categorization matrix for posts within a campaign. Format:
```json
{
  "postsColumns": [
    { "id": "slug", "label": "Display Name", "values": ["Value1", "Value2"], "multiple": false }
  ]
}
```
Use **update_campaign** to set them. Each column becomes a dimension in the content matrix.

---

## 4. Posts & Media

Posts are individual content pieces linked to a campaign. Each post can have multiple media entries (for carousels, videos with scenes, etc.).

| Tool | Purpose |
|------|---------|
| **list_posts** | List posts, optionally filtered by campaign. |
| **get_post** | Full post detail: copy, status fields (postStatus, copyStatus, uiStatus, scheduleStatus), customColumns. |
| **create_post** | Create a post with title, content, platform, campaign link, and customColumns. |
| **update_post** | Modify post content, statuses, or column values. |
| **update_post_status** | Quick status update for a specific status field. |
| **delete_post** | Remove a post. |
| **add_post_media** | Add a media entry (image, carousel slide, reel) with production prompt. |
| **list_post_media** | List all media entries for a post. |
| **update_post_media** | Modify an existing media entry. |
| **delete_post_media** | Remove a media entry. |

### Creating posts

- Use sequential IDs in titles: PREFIX-001, PREFIX-002, etc.
- Write copy in Mexican Spanish — natural, direct, empathetic, no corporate jargon
- Assign all customColumns matching the campaign's postsColumns
- Set platform: Instagram (~60%) / Facebook (~40%)
- Each post should work independently

### Adding media

- For **images**: one media entry with detailed production prompt (visual description, colors, dimensions)
- For **carousels**: one media entry per slide
- For **reels/videos**: one media entry with scenes array (title, description, duration per scene)
- Include production method in the prompt: Figma Static, ElevenLabs, Replit Animation, or Video

---

## 5. Sprints & Task Scheduling

Sprints organize work into time-boxed periods. Tasks within sprints can link to posts and have scheduled date blocks for different production phases.

| Tool | Purpose |
|------|---------|
| **list_sprints** | List all sprints with their dates and statuses. |
| **get_sprint** | Full sprint detail. |
| **create_sprint** | Create with name, start/end dates, status ("planning" initially). |
| **update_sprint** | Change status to "active", "completed", etc. |
| **delete_sprint** | Remove a sprint. |
| **list_sprint_tasks** | List all tasks in a sprint with statuses. |
| **create_sprint_task** | Create a task with title, postId link, priority (low/medium/high), initial status. |
| **update_sprint_task** | Modify task details. |
| **move_sprint_task** | Change a task's Kanban column (backlog → todo → in_progress → review → done). |
| **delete_sprint_task** | Remove a task. |
| **schedule_sprint_task** | Add a scheduled date block with label, startDate (ISO), and durationHours or endDate. |
| **remove_sprint_task_schedule** | Remove a schedule from a task. |
| **complete_task** | Mark a task as complete. |

### Planning a sprint

1. **list_sprints** → check existing sprints
2. **create_sprint** → name, dates, status "planning"
3. **list_campaigns** → **list_posts** → find posts needing work
4. **create_sprint_task** for each → link via postId, set priority
5. **schedule_sprint_task** with labeled blocks: "Diseño" (design), "Desarrollo" (production), "Revisión" (review)
6. **list_sprint_tasks** → review the Kanban layout
7. **update_sprint** → change to "active" when ready

### Reviewing a sprint

1. **get_sprint** → sprint details
2. **list_sprint_tasks** → categorize by status (backlog, todo, in_progress, review, done)
3. For linked posts: **get_post** → check copyStatus, uiStatus, scheduleStatus
4. Report: X/Y tasks done (%), count per column, blockers, post status, recommendations
5. Offer to update statuses → **move_sprint_task** or **update_sprint_task**

---

## 6. Ideas Board

A workspace-wide strategic backlog for marketing ideas. Ideas flow through: backlog → evaluating → planned → in_progress → done | rejected.

| Tool | Purpose |
|------|---------|
| **list_ideas** | List ideas, filterable by status and project group. |
| **get_idea** | Full idea detail. |
| **create_idea** | Create with title, description, priority, status, optional project group. |
| **update_idea** | Change status, priority, category. |
| **delete_idea** | Remove an idea. |
| **link_idea_to_sprint** | Connect a planned idea to a sprint task. |

### Triaging ideas

1. **list_ideas** with status "backlog" → get unreviewed ideas
2. **get_idea** for each → full details
3. Check context: **list_project_groups**, **get_project_info**, **list_campaigns**, **list_team_notes**
4. Recommend priority (low/medium/high) and status (evaluating/planned/rejected)
5. **update_idea** to apply changes
6. For "planned" ideas: **list_sprints** / **create_sprint** → **create_sprint_task** → **link_idea_to_sprint**
7. Summary: total reviewed, planned, rejected, still evaluating

---

## 7. Team Notes

Shared team board for async communication and context sharing.

| Tool | Purpose |
|------|---------|
| **list_team_notes** | List all team notes. |
| **create_team_note** | Post a new note with authorName and content. |
| **update_team_note** | Modify an existing note. |
| **delete_team_note** | Remove a note. |

### Generating a team briefing

Gather data from across the platform and compose a comprehensive update:

1. **list_sprints** → find active → **list_sprint_tasks** for each
2. **list_campaigns** → active campaigns with lead counts
3. **search_leads** with pending=true → leads needing attention
4. **get_pending_messages** → unconfirmed messages
5. **list_ideas** with status "evaluating" → ideas awaiting decision
6. **list_schedules** with status "pending" → upcoming workflows
7. Compose sections: Sprint Status, Campaign Health, Action Items, Upcoming, Ideas Pipeline
8. **create_team_note** with authorName "AI Briefing"

---

## 8. Data Stores & Jobs

Data stores hold structured data (like contact lists, product catalogs, or scraped results) that can feed into AI pipelines. Data jobs are JavaScript scripts that populate stores automatically.

| Tool | Purpose |
|------|---------|
| **list_data_stores** | List all data stores. |
| **get_data_store** | Get store details and schema. |
| **create_data_store** | Create with name and schema columns (key, title, type: text/number/boolean/date). |
| **update_data_store** | Modify store settings. |
| **delete_data_store** | Remove a store. |
| **clear_data_store** | Wipe all rows. |
| **get_data_store_rows** | Read stored data. |
| **write_data_store_rows** | Write rows with mode "append" or "replace". |
| **list_data_jobs** | List all data jobs. |
| **get_data_job** | Get job details including code. |
| **create_data_job** | Create a JS job: code that returns an array of objects, linked dataStoreId, optional env vars, write mode ("replace" or "upsert"). |
| **update_data_job** | Modify job code or settings. |
| **delete_data_job** | Remove a job. |
| **trigger_data_job** | Execute a job immediately. |
| **get_data_job_runs** | View execution history. |

### Setting up a data store

1. **list_data_stores** → check for duplicates
2. **create_data_store** → name + schema columns
3. Populate manually: **write_data_store_rows** (mode "append" or "replace")
4. Or auto-populate: **create_data_job** (JS code returning array of objects) → **trigger_data_job** → **get_data_store_rows** to verify

---

## 9. AI Pipelines

Pipelines are visual node graphs that automate marketing workflows — from lead auto-replies to content generation to data enrichment. They're built with nodes and edges, then executed by Convex.

| Tool | Purpose |
|------|---------|
| **get_node_types** | List all available node types with categories, descriptions, and configurable fields. |
| **create_project** | Create a new pipeline project with name and description. |
| **get_project** | View pipeline with all nodes and edges. |
| **update_project** | Change status ("active", "inactive"), name, or description. |
| **delete_project_info** | Remove project metadata. |
| **add_node** | Add a node to the pipeline (input, processing, or output). |
| **update_node** | Modify node configuration. |
| **remove_node** | Delete a node. |
| **add_edge** | Connect two nodes (source → target). |
| **remove_edge** | Disconnect nodes. |
| **add_note** | Add a comment/note to the pipeline canvas. |
| **run_pipeline** | Execute the pipeline with a test lead or trigger. |
| **list_pipelines** | List all pipeline projects. |

### Building a pipeline

1. **get_node_types** → understand available nodes (input, LLM, conditional, data lookup, output)
2. Clarify purpose: lead processing, content generation, or data enrichment
3. **create_project** → descriptive name
4. **add_node** → start with input node (e.g., inputUserMessages), add processing nodes, end with output nodes (sendMessage, updateLead, etc.)
5. **add_edge** → connect source to target
6. **get_project** → verify all nodes are connected
7. **update_project** → set status to "active"
8. **run_pipeline** → test with a real lead

---

## 10. Project Groups & Info

Project groups are organizational containers that link campaigns, leads, posts, and sprints together. Project info stores key-value brand context (voice, tone, instructions) that feeds into AI workflows.

| Tool | Purpose |
|------|---------|
| **list_project_groups** | List all project groups with their IDs. |
| **get_project_group** | Full group detail. |
| **create_project_group** | Create a new group. |
| **update_project_group** | Modify group settings. |
| **delete_project_group** | Remove a group. |
| **get_project_info** | Get brand context (voice, tone, pain points, USP) stored as key-value pairs. |
| **upsert_project_info** | Create or update project info entries. |

---

## 11. Marketing Files & Boxes

Hierarchical file management for marketing assets.

| Tool | Purpose |
|------|---------|
| **list_marketing_files** | List files in a folder. |
| **create_marketing_folder** | Create a new folder. |
| **rename_marketing_file** | Rename a file or folder. |
| **delete_marketing_file** | Remove a file. |
| **list_boxes** | List media boxes (collections). |
| **get_box** | Get box details. |
| **create_box** | Create a new box. |
| **delete_box** | Remove a box. |

---

## 12. Workflow Scheduling

Schedule automated workflows for timed follow-ups, recurring data jobs, and campaign automation.

| Tool | Purpose |
|------|---------|
| **list_schedules** | List all scheduled workflows with their statuses. |
| **schedule_workflow** | Create a new scheduled workflow execution. |
| **cancel_schedule** | Cancel a pending schedule. |

---

## Key Principles

- **All user-facing text must be in Spanish** (Mexican Spanish specifically)
- **Two-step messaging**: always draft → confirm. Never skip confirmation.
- **Campaign IDs matter**: save them after creation — they're needed to link posts, leads, and sprints.
- **Sequential post IDs**: use PREFIX-001, PREFIX-002, etc. for human-readable organization.
- **Verify after creation**: always call the corresponding list/get tool to confirm things were created correctly.
- **Project info is your brand context**: read it first via **get_project_info** before writing any content — it contains voice, tone, pain points, and differentiators.
