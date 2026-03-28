---
name: lexguard
description: >
  Complete reference for operating the LexGuard platform via its five MCP servers.
  Covers every tool category across Marketing (leads, campaigns, posts, sprints,
  ideas, pipelines, calendars, boxes, team notes), Business (panorama, CRM, data
  stores & jobs, file import), Finance (transactions, invoices, reports, accounts,
  scheduled payments), Office (mail, visitors, calls, reservations, locations),
  and Compliance (tracking rules, file search, collections, obligations).
  Use this skill whenever working with LexGuard data.
metadata:
  author: lexguard
  version: "3.0.0"
  requires-mcp: lexguard-marketing, lexguard-business, lexguard-finance, lexguard-office, lexguard-compliance
---

# LexGuard Platform Guide

This skill is your complete reference for operating LexGuard through its MCP tools. LexGuard is an AI-native business operating system where small business teams manage leads, campaigns, content, sprints, finance, compliance, and virtual office operations — all in Spanish.

LexGuard exposes **five MCP servers**, each grouping related tools:

| MCP Server | Endpoint | Domain |
|------------|----------|--------|
| **lexguard-marketing** | `/api/mcp/marketing` | Leads, campaigns, posts, sprints, ideas, pipelines, calendars, boxes, team notes |
| **lexguard-business** | `/api/mcp/business` | Business panorama, CRM, data stores & jobs, file import |
| **lexguard-finance** | `/api/mcp/finance` | Transactions, invoices, accounts, categories, reports, forecasting |
| **lexguard-office** | `/api/mcp/office` | Mail, visitors, calls, reservations, locations |
| **lexguard-compliance** | `/api/mcp/compliance` | Tracking rules, file search, virtual collections, obligations |

---

# Marketing Server (`lexguard-marketing`)

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
| **add_note** | Add a note to a lead's record. |
| **create_task** | Create a standalone task for a lead. |
| **complete_task** | Mark a task as complete. |

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

## 7. AI Pipelines

Pipelines are visual node graphs that automate marketing workflows — from lead auto-replies to content generation to data enrichment. They're built with nodes and edges, then executed by Convex.

| Tool | Purpose |
|------|---------|
| **get_node_types** | List all available node types with categories, descriptions, and configurable fields. |
| **create_project** | Create a new pipeline project with name and description. |
| **get_project** | View pipeline with all nodes and edges. |
| **update_project** | Change status ("active", "inactive"), name, or description. |
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

## 8. Calendars & Scheduling

Calendars manage time-based scheduling with capacity control, business hours, and slot management.

| Tool | Purpose |
|------|---------|
| **list_calendars** | List all scheduling calendars with name, capacity, slot duration, business hours, and active status. |
| **create_calendar** | Create a new scheduling calendar with business hours and capacity settings. |
| **get_calendar_events** | List events for a calendar within a date range. |
| **create_calendar_event** | Create a calendar event. Validates against parallel capacity for the time slot. |
| **update_calendar_event** | Modify an existing calendar event. |
| **cancel_calendar_event** | Cancel a calendar event. |

---

## 9. Boxes (Cajas)

Boxes are media collections used to organize marketing assets.

| Tool | Purpose |
|------|---------|
| **list_boxes** | List all media boxes/collections. |
| **get_box** | Get box details. |
| **create_box** | Create a new box. |
| **delete_box** | Remove a box. |

---

## 10. Marketing Files

Hierarchical file management for marketing assets.

| Tool | Purpose |
|------|---------|
| **list_marketing_files** | List files in a folder. |
| **create_marketing_folder** | Create a new folder. |
| **rename_marketing_file** | Rename a file or folder. |
| **delete_marketing_file** | Remove a file. |

---

## 11. Project Groups & Info

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
| **delete_project_info** | Remove project info entries. |

---

## 12. Team Notes

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

## 13. Workflow Scheduling

Schedule automated workflows for timed follow-ups, recurring data jobs, and campaign automation.

| Tool | Purpose |
|------|---------|
| **list_schedules** | List all scheduled workflows with their statuses. |
| **schedule_workflow** | Create a new scheduled workflow execution. |
| **cancel_schedule** | Cancel a pending schedule. |

---

# Business Server (`lexguard-business`)

## 14. Business Intelligence

High-level tools that aggregate data from multiple domains for a panoramic view of the business.

| Tool | Purpose |
|------|---------|
| **get_business_panorama** | Get a high-level panorama: compliance health, lead pipeline, pending tasks, unread mail, and recent activity. Each section is fetched independently so partial failures don't break the response. |
| **search_across_domains** | Search across multiple business domains (leads, companies, files) with a single query string. Returns matching results from each requested domain. |

### Getting a business snapshot

1. **get_business_panorama** → comprehensive overview
2. Drill into specific areas based on findings:
   - Low compliance health → switch to compliance tools
   - Pending leads → switch to lead tools
   - Unread mail → switch to office tools
3. Present a prioritized action list

---

## 15. CRM (Directorio)

Companies and contacts management for the business directory.

| Tool | Purpose |
|------|---------|
| **list_companies** | List companies. Supports optional search by business name or trade name. |
| **get_company** | Get a company by ID, including related addresses and personas. |
| **create_company** | Create a new company in the CRM. |
| **update_company** | Update an existing company by ID. |
| **list_personas** | List personas/contacts. Optionally filter by company. |
| **create_persona** | Create a new persona/contact in the CRM. |

---

## 16. Data Stores & Jobs

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

## 17. File Import

Import files from external sources into the LexGuard file library.

| Tool | Purpose |
|------|---------|
| **importFile** | Import a file into the library. Supports three source types: `url` (fetch from URL), `base64` (encoded binary data with mimeType), or `text` (plain text/CSV/Markdown). Returns the stored file record. |

---

# Finance Server (`lexguard-finance`)

## 18. Transactions

Core financial transaction management backed by Convex.

| Tool | Purpose |
|------|---------|
| **search_transactions** | Search and list transactions with filters. Returns up to 100 results ordered by date descending. |
| **get_transaction** | Get a single transaction by ID with full details including linked transactions. |
| **create_transaction** | Create a new financial transaction (income or expense). |
| **update_transaction** | Update an existing transaction's fields. |
| **delete_transaction** | Delete a transaction. Linked transactions are unlinked automatically. |
| **link_transactions** | Link two transactions for reconciliation (bidirectional). Matches income and expense records for the same payment. |
| **get_transaction_summary** | Financial summary: balance by currency, totals by aggregation category, transaction count. Optionally filter by date range. |

---

## 19. Scheduled Transactions

Recurring/scheduled transaction templates that auto-generate transactions at specified frequencies.

| Tool | Purpose |
|------|---------|
| **list_scheduled_transactions** | List all scheduled/recurring transactions. Optionally filter by active status. |
| **create_scheduled_transaction** | Create a recurring transaction template with frequency settings. |
| **update_scheduled_transaction** | Update a scheduled transaction. Recalculates next due date if schedule changes. |
| **toggle_scheduled_transaction** | Activate or deactivate a scheduled transaction. |
| **process_due_transactions** | Generate transactions from all scheduled transactions that are due. Returns count of processed and generated. |
| **get_pending_scheduled** | Get scheduled transactions due within a time window. Shows what will be auto-generated soon. |

---

## 20. Accounts & Categories

Financial account and category management.

| Tool | Purpose |
|------|---------|
| **list_accounts** | List all finance accounts (bank accounts, cash, credit cards, digital wallets). |
| **create_account** | Create a new finance account. |
| **update_account** | Update an account's name, type, institution, or active status. |
| **list_categories** | List finance categories. Optionally filter by type (income/expense/both). |
| **create_category** | Create a new finance category for classifying transactions. |
| **seed_default_categories** | Seed default categories (Nómina, Renta, Servicios, Software, etc.) if none exist. Safe to call multiple times. |
| **get_finance_settings** | Get finance module settings. |

---

## 21. Integrations

Billing integrations with external services.

| Tool | Purpose |
|------|---------|
| **list_integrations** | List all billing integrations (Stripe, AWS, Cloudflare, Clockify, etc.) with sync status. |
| **get_integration_stats** | Get statistics for a specific integration: total transactions, income, expenses, last transaction date. |

---

## 22. Invoices (Facturación)

Invoice management backed by PostgreSQL. Handles CFDI tax data for Mexican invoicing.

| Tool | Purpose |
|------|---------|
| **list_invoices** | List invoices with optional filters by status and date. Most recent first. Invoices are linked to contracts and contain CFDI tax data. |
| **get_invoice** | Get a single invoice with full details: products, extra charges (late fees, discounts), payment complements, and attached files. |
| **get_overdue_invoices** | Get all overdue invoices (status=pending, dueDate in the past). Useful for identifying collection issues. |
| **get_tax_config** | Get tax configuration settings. |
| **list_payment_complements** | List payment complements (Complementos de Pago) for PPD invoices. CFDI documents that confirm payment receipt. |
| **get_invoice_summary** | Invoice statistics: counts and totals by status (pending, paid, overdue, cancelled). |

---

## 23. Financial Reports

Cross-backend reporting tools combining Convex transactions with PostgreSQL invoices.

| Tool | Purpose |
|------|---------|
| **get_financial_report** | Comprehensive financial report for a period: income vs expenses by category, balance by currency, account balances, and invoice status. |
| **get_cashflow_projection** | Project cash flow for the next N days based on current balances, scheduled transactions, and pending invoices. Identifies potential shortfalls. |
| **get_reconciliation_status** | Reconciliation status: unreconciled transactions, transactions needing reconciliation, and suggested matches based on amount and date proximity. |
| **get_category_breakdown** | Expense/income breakdown by category for a period. Understand where money goes. |
| **get_monthly_trend** | Monthly income vs expense trend for the last N months. Shows month-over-month changes. |

### Financial health check

1. **get_financial_report** → overall picture for current period
2. **get_cashflow_projection** → upcoming cash flow risks
3. **get_overdue_invoices** → collection issues
4. **get_reconciliation_status** → unmatched transactions
5. **get_monthly_trend** → trajectory over time
6. Present: current health, risks, action items

---

# Office Server (`lexguard-office`)

## 24. Virtual Office

Complete virtual office management: correspondence, visitors, calls, reservations, and locations.

| Tool | Purpose |
|------|---------|
| **get_office_summary** | Summary of office activity: unread mail, upcoming visitors, recent calls, active reservations. |
| **list_mail** | List correspondence/mail items with virtual and physical location info. |
| **get_mail_item** | Get a single mail item by ID with attached files. |
| **get_unread_mail_count** | Count of unread/pending mail items. |
| **list_visitors** | List visitor invitations with location info. |
| **get_visitor** | Get a single visitor invitation by ID. |
| **create_visitor** | Create a visitor invitation. |
| **list_call_logs** | List phone call logs. Includes calls from linked portal groups. |
| **get_call_log** | Get a single call log by ID. |
| **list_virtual_locations** | List virtual office locations for this group. |
| **get_virtual_location** | Get a single virtual location by ID with its physical location info. |
| **list_physical_locations** | List physical locations accessible to this group (via virtual locations). |
| **list_reservations** | List space/room reservations for this group. |
| **get_reservation** | Get a single space reservation by ID. |
| **create_reservation** | Create a space/room reservation. |
| **list_document_types** | List available document types for mail classification. |

### Office daily briefing

1. **get_office_summary** → quick overview
2. **list_mail** → check for unread correspondence
3. **list_visitors** → upcoming visitors today
4. **list_reservations** → active/upcoming room bookings
5. Present: priority mail, visitor schedule, room availability

---

# Compliance Server (`lexguard-compliance`)

## 25. Compliance Tracking (Pulso)

Rules-based document compliance tracking with health scores. Monitors business obligations and their fulfillment via uploaded documents.

| Tool | Purpose |
|------|---------|
| **get_compliance_summary** | Compliance health summary: all active tracking rules and their status (current, warning, overdue). |
| **list_tracking_rules** | List all tracking rules (compliance obligations) for the business. |
| **get_tracking_rule** | Get a single tracking rule by ID. |
| **create_tracking_rule** | Create a new tracking rule (compliance obligation). |
| **update_tracking_rule** | Update an existing tracking rule by ID. |
| **delete_tracking_rule** | Delete a tracking rule by ID. |
| **get_rule_matching_files** | Get files that match a specific tracking rule's tag criteria. |
| **get_overdue_obligations** | Get all overdue compliance obligations. |

### Setting up compliance tracking

1. **detect_tag_groups** → discover existing tag patterns in uploaded files
2. **create_rules_from_detected** → auto-create rules from detected tag groups
3. Or manually: **create_tracking_rule** with tag criteria, frequency, and deadlines
4. **get_compliance_summary** → verify health scores
5. Monitor regularly: **get_overdue_obligations** → identify urgent items

---

## 26. File Search & Tags

Search and inspect files and their tag metadata for compliance analysis.

| Tool | Purpose |
|------|---------|
| **search_files** | Search files across the business group. |
| **get_file_detail** | Get detailed file information. |
| **list_file_tags** | List file tag field/value summary. |
| **detect_tag_groups** | Discover distinct tag field/value combinations with file counts. Helps identify what compliance rules could be created. |
| **create_rules_from_detected** | Auto-create tracking rules from detected tag groups. |

---

## 27. Virtual Collections

Automated file grouping with AI-powered batch processing.

| Tool | Purpose |
|------|---------|
| **list_virtual_collections** | List virtual collections for the group. |
| **get_collection_results** | Get results from a collection run. |
| **trigger_collection_run** | Trigger a new collection processing run. |

---

# Key Principles

- **All user-facing text must be in Spanish** (Mexican Spanish specifically)
- **Two-step messaging**: always draft → confirm. Never skip confirmation.
- **Campaign IDs matter**: save them after creation — they're needed to link posts, leads, and sprints.
- **Sequential post IDs**: use PREFIX-001, PREFIX-002, etc. for human-readable organization.
- **Verify after creation**: always call the corresponding list/get tool to confirm things were created correctly.
- **Project info is your brand context**: read it first via **get_project_info** before writing any content — it contains voice, tone, pain points, and differentiators.
- **Use the right server**: tools are distributed across five MCP servers. If a tool is not found, you may be connected to the wrong server.
- **Finance is dual-backend**: transactions live in Convex, invoices in PostgreSQL. Reports combine both sources.
- **Compliance tracks obligations**: tracking rules define what documents are needed, files prove fulfillment. Health scores derive from matching files against rules.
