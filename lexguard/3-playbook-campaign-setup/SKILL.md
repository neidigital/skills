---
name: 3-playbook-campaign-setup
description: "Stage 3/7 — Create campaigns in Lexguard with custom columns, statuses, and settings based on the approved strategy. Requires LexGuard Leads MCP server."
metadata:
  author: lexguard
  version: "1.0.0"
  requires-mcp: lexguard-leads
  playbook-stage: "3"
---

# 3. Playbook: Campaign Setup

Create the campaigns in Lexguard based on the approved strategy from stage 2.

## Steps

1. **Create campaigns**: For each campaign in the strategy, use **create_campaign** with a descriptive name and description.
2. **Configure postsColumns**: For each campaign, use **update_campaign** to add categorization columns matching the content matrix. Format:
   ```json
   { "postsColumns": [{ "id": "slug", "label": "Name", "values": ["V1", "V2"], "multiple": false }] }
   ```
3. **Verify setup**: Use **get_campaign** for each campaign. Confirm postsColumns are correct.
4. **Show summary table**:
   | Campaign | ID | Type | # Columns | Status |
   |----------|-----|------|-----------|--------|

Save campaign IDs — they're needed for the next stages.

## Next stage

After setup is verified, proceed to **/4-playbook-post-creation**.
