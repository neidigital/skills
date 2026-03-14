---
name: 6-playbook-scale-validate
description: "Stage 6/7 — Scale from ~20 posts to 60-80 total, ensuring full content matrix coverage and balance across campaigns, platforms, and production methods. Requires LexGuard Leads MCP server."
metadata:
  author: lexguard
  version: "1.0.0"
  requires-mcp: lexguard-leads
  playbook-stage: "6"
---

# 6. Playbook: Scale & Validate

Scale from ~20 to 60-80 posts total with full content matrix coverage.

The user should provide the post ID prefix to continue the sequence.

## Steps

1. **Analyze coverage**: Use **list_posts** per campaign. Map which pain points, tones, and content types are covered. Identify gaps.
2. **Plan ~50 new posts**: Cover all pain points (3+ variants each), all tones (2+ per campaign), maintain 60/40 Instagram/Facebook balance.
3. **Create posts in batches**: Continue sequential IDs. Full content with customColumns.
4. **Add media**: Use **add_post_media** for each new post with production prompts.
5. **Validate coverage**: Generate report tables — by campaign, by pain point, by production method. List remaining gaps.

## Next stage

After scaling and validation, proceed to **/7-playbook-documentation**.
