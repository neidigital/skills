---
name: 4-playbook-post-creation
description: "Stage 4/7 — Create the first batch of ~20 posts distributed across campaigns with full content, categorization, and platform targeting. Requires LexGuard Leads MCP server."
metadata:
  author: lexguard
  version: "1.0.0"
  requires-mcp: lexguard-leads
  playbook-stage: "4"
---

# 4. Playbook: Create Posts

Create the first batch of ~20 posts distributed across campaigns.

The user should provide:
- Product name
- Post ID prefix (e.g. 'ND', 'DD', 'FL', 'LX')

## Steps

1. Use **list_campaigns** to get campaign IDs.
2. Distribute ~20 posts proportionally (e.g. ~7 TOFU, ~7 BOFU, ~6 Social Proof).
3. For each post, use **create_post** with:
   - Sequential ID in title: PREFIX-001, PREFIX-002, etc.
   - Full copy in Mexican Spanish (hook, body, CTA, 5-10 hashtags)
   - Platform: alternate Instagram (~60%) / Facebook (~40%)
   - All customColumns assigned
4. Verify with **list_posts** per campaign. Show summary table.

## Content rules

- Mexican Spanish — natural, direct, empathetic
- No corporate jargon — talk like a knowledgeable friend
- Each post must work independently
- Vary formats: static images, carousels, short reels

## Next stage

After posts are created and verified, proceed to **/5-playbook-media-production**.
