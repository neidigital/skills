---
name: pipeline-builder
description: Design and build an AI pipeline with nodes and edges for automated lead processing, content generation, or data enrichment. Requires LexGuard Leads MCP server.
metadata:
  author: lexguard
  version: "1.0.0"
  requires-mcp: lexguard-leads
---

# Pipeline Builder

Build an AI pipeline for automated marketing workflows.

## Steps

1. Use **get_node_types** to see all available node types with categories, descriptions, and configurable fields.
2. Ask the user about the pipeline's purpose:
   - Lead processing (auto-reply, qualification, routing)
   - Content generation (post copy, media prompts)
   - Data enrichment (research, scoring)
3. Use **create_project** with a descriptive name and description.
4. Add nodes with **add_node**:
   - Start with an input node (e.g. inputUserMessages for lead-triggered pipelines)
   - Add processing nodes (LLM, conditional, data lookup)
   - End with output nodes (sendMessage, updateLead, etc.)
5. Connect nodes with **add_edge** (source → target).
6. Verify the pipeline with **get_project** — check all nodes are connected.
7. Use **update_project** to set status to "active" when ready.
8. Test by using **run_pipeline** with a test lead.
