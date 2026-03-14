---
name: 5-playbook-media-production
description: "Stage 5/7 — Add media entries to each post with production prompts, classify by production method (Figma, ElevenLabs, Replit, Video). Requires LexGuard Leads MCP server."
metadata:
  author: lexguard
  version: "1.0.0"
  requires-mcp: lexguard-leads
  playbook-stage: "5"
---

# 5. Playbook: Media & Production

Add media entries and production prompts to all existing posts.

## Steps

1. Use **list_campaigns** and **list_posts** to get all posts.
2. Classify each post by production method:
   - **Figma Static** (55-65%): Static images and carousels
   - **ElevenLabs** (15-25%): UGC voice-over, AI testimonials
   - **Replit Animation** (10-15%): Motion graphics, data animations
   - **Video** (0-5%): Only when face-to-face authenticity is essential
3. For each post, use **add_post_media** with detailed production prompts (visual description, copy text, colors, dimensions, method).
4. For carousels: one media entry per slide. For videos: one per scene.
5. Search for real testimonials and metrics to incorporate.
6. Verify with **list_post_media** for random posts.

## Next stage

After all media entries are added, proceed to **/6-playbook-scale-validate**.
