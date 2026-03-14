---
name: 7-playbook-documentation
description: "Stage 7/7 — Document all campaign work, create production guides, and prepare the handoff for asset production and Meta Ads setup. Requires LexGuard Leads MCP server."
metadata:
  author: lexguard
  version: "1.0.0"
  requires-mcp: lexguard-leads
  playbook-stage: "7"
---

# 7. Playbook: Documentation & Handoff

Document everything and prepare the handoff for production.

## Output documents

### 1. Master campaign document (`campana-arquitectura-2026.md`)
- Executive summary
- Campaign architecture & funnel diagram
- Full post inventory (use **list_campaigns** + **list_posts**)
- Content matrix coverage
- Production strategy by method
- Target metrics (KPIs, benchmarks)
- Next steps (asset production, Meta Ads setup, A/B testing, publishing calendar)

### 2. Production guide (`guia-produccion.md`)
- **Figma Static**: Templates, brand colors, typography, dimensions, assigned posts
- **ElevenLabs**: Voice type, target duration, script template, assigned posts
- **Replit Animation**: Animation types, duration, assigned posts
- **Video**: What to record, script, backup plan

### 3. Quick ID reference
- Campaign IDs with names
- Post counts per campaign
- Links to key documents

### 4. Final verification
- Confirm totals match Lexguard (use **list_posts**)
- Verify no posts without media entries
- Verify no posts without customColumns

Save documents in the location the user specifies.
