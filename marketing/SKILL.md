---
name: marketing
description: >
  End-to-end marketing strategy, campaign creation, content planning, and performance
  analysis for LexGuard. Includes the 7-stage campaign playbook (research → strategy →
  setup → posts → media → scale → documentation), content matrix design, funnel
  architecture, KPI frameworks, and best practices for Mexican Spanish copywriting.
  Use this skill whenever planning a campaign, analyzing marketing performance, creating
  content strategies, designing funnels, reviewing content matrix coverage, setting KPIs,
  writing marketing copy, or doing anything related to marketing strategy and execution —
  even if the user doesn't explicitly say "marketing."
metadata:
  author: lexguard
  version: "2.0.0"
  requires-mcp: lexguard-leads
---

# Marketing Strategy & Execution

This skill covers everything about marketing strategy, campaign design, content creation, and performance analysis. It teaches you *what* to build and *why*, while the **lexguard** skill teaches you *how* to operate the platform tools.

Both skills work together: this one gives you the strategy and best practices, the lexguard skill gives you the exact tool calls.

---

## The Campaign Playbook (7 Stages)

This is the complete workflow for building a marketing campaign from scratch. Follow these stages in order. Each stage builds on the previous one.

### Stage 1: Research & Discovery

**Goal**: Understand the product deeply before designing anything.

**What to do**:
1. **Read product documentation**: brand guidelines, voice & tone, features, differentiators. Use **get_project_info** from the "Base de Conocimiento" project group.
2. **Read existing marketing materials**: pain-point docs, benefits docs, content matrix, communication guidelines, existing posts.
3. **Find real data**: customer testimonials, success stories, impact metrics (savings, ROI), market data.
4. **Competitive research**: search the web for competitors in Mexico. Analyze their Instagram/Facebook content. Identify gaps to exploit.
5. **Ask strategic questions** — but only questions you can't answer from the data:
   - Available ad budget
   - Primary objective (awareness, leads, conversions, retention)
   - Best existing customers (for testimonials)
   - Common sales objections
   - What's worked / not worked in previous marketing
   - Timeline and urgency
   - Available production resources (design, video, etc.)
   - Quantifiable metrics: instead of "is it fast?", ask "how many seconds does it take?"
   - Specific customer behavior: instead of "who are your customers?", ask "what system were your last 3 customers using before they switched?"

**Do NOT propose strategy yet.** Only research and questions. Understand first.

### Stage 2: Strategy & Campaign Architecture

**Goal**: Design the complete campaign structure based on research.

**Produce these deliverables**:

1. **Funnel diagnosis**: Why prospects aren't converting. Top 5–7 objections with estimated frequency. Where in the funnel most are lost.

2. **Campaign architecture** (2–4 campaigns covering the full funnel):
   - **TOFU** (Top of Funnel): awareness, education, problem recognition
   - **BOFU** (Bottom of Funnel): retargeting, objection handling, closing
   - **Social Proof**: testimonials, case studies, results
   - **B2B-specific** (if applicable): industry-targeted messaging

   For each campaign define: name, description, target audience, objective, pain points addressed, objections countered, success metrics.

3. **Content matrix per campaign**: Define categorization columns (these become postsColumns in LexGuard):
   - Category (benefit, objection, social proof type)
   - Tone/angle (retador, empático, educativo, con dato financiero)
   - Product dimension (specific pain point or feature)
   - Variant (urgency, credibility, ROI, emotional)

4. **Dynamic variable matrix** (4–5 variables per campaign with 3–5 values each):

   Example for a "Harto de tu Sistema?" campaign:
   | Variable | Options |
   |----------|---------|
   | Sistema Origen | Soft Restaurant, Excel, Otro POS, Sistema Local |
   | Frustración | Sin actualizaciones, Mal soporte, Muy caro, Requiere técnico |
   | Solución | Actualizaciones semanales, Soporte por WhatsApp, Precio transparente |
   | Perfil Dueño | Dueño tradicional, Gerente de cadena, Emprendedor joven |
   | Tono | Retador, Empático, Educativo, Con dato financiero |

5. **Platform distribution**: Instagram (~60%) + Facebook (~40%). Formats: feed images, carousels, reels.

6. **Production method allocation**:
   - Figma Static: 55–65% (static images and carousels)
   - ElevenLabs: 15–25% (UGC voice-over, AI testimonials)
   - Replit Animation: 10–15% (motion graphics, data animations)
   - Video: 0–5% (only when face-to-face authenticity is essential)

7. **Funnel diagram**: How campaigns connect, retargeting audiences, cross-posting strategy.

8. **Custom sales pipeline per campaign**: 5–7 deal stages (e.g., Nuevo → Contactado → Diagnóstico → Demo → Propuesta → Cerrado).

Save as `campana-arquitectura-2026.md`. Get user approval before proceeding.

### Stage 3: Campaign Setup in LexGuard

**Goal**: Create campaigns in the platform matching the approved strategy.

1. Use **create_campaign** for each campaign in the architecture.
2. Use **update_campaign** to configure postsColumns matching the content matrix.
3. Verify with **get_campaign** for each campaign.
4. Present a summary table:

| Campaign | ID | Type | # Columns | Status |
|----------|-----|------|-----------|--------|

Save campaign IDs — they're needed for every subsequent stage.

### Stage 4: Post Creation (First Batch)

**Goal**: Create the first ~20 posts distributed across campaigns.

1. Distribute proportionally (e.g., ~7 TOFU, ~7 BOFU, ~6 Social Proof).
2. For each post, use **create_post** with:
   - Sequential ID: PREFIX-001, PREFIX-002, etc. (user provides prefix)
   - Full copy in Mexican Spanish (hook, body, CTA, 5–10 hashtags)
   - Platform: alternate Instagram (~60%) / Facebook (~40%)
   - All customColumns assigned
3. For matrix-based campaigns: select 5–10 high-impact combinations from the variable matrix. Write 2 variants per combination (A: direct/data-driven, B: empathetic/story-driven) for A/B testing.
4. Verify with **list_posts** per campaign. Show summary table.

### Stage 5: Media & Production

**Goal**: Add media entries and production prompts to every post.

1. Classify each post by production method (see allocation in Stage 2).
2. For each post, use **add_post_media** with detailed production prompts:
   - Visual description (scene composition, mood, colors)
   - Copy text overlay
   - Brand colors and typography
   - Dimensions and platform specs
   - Production method label
3. For carousels: one media entry per slide.
4. For reels: one media entry with scenes array (title, description, duration per scene).
5. For carousel slides include: slide label ("Carrusel: Taquería × Reimpresión × Dato Financiero"), detailed visual brief, platform notes.
6. Incorporate real testimonials and metrics where available.
7. Verify with **list_post_media** for random posts.

### Stage 6: Scale & Validate

**Goal**: Scale from ~20 to 60–80 total posts with full content matrix coverage.

1. **Analyze coverage**: Use **list_posts** per campaign. Map which pain points, tones, content types, and production methods are represented. Identify gaps.
2. **Plan ~50 new posts**: Cover all pain points (3+ variants each), all tones (2+ per campaign), maintain 60/40 platform balance.
3. **Create in batches**: Continue sequential IDs, full content with customColumns.
4. **Add media** for each new post with production prompts.
5. **Validate coverage** with report tables:
   - By campaign: post count, platform split
   - By pain point: variants per pain point
   - By production method: distribution match target %
   - By content matrix dimension: completeness check
6. List remaining gaps and address them.

### Stage 7: Documentation & Handoff

**Goal**: Document everything and prepare for asset production.

**Output documents**:

1. **Master campaign document** (`campana-arquitectura-2026.md`):
   - Executive summary
   - Campaign architecture & funnel diagram
   - Full post inventory (verify with **list_campaigns** + **list_posts**)
   - Content matrix coverage analysis
   - Production strategy by method
   - Target KPIs and benchmarks
   - Next steps: asset production, Meta Ads setup, A/B testing, publishing calendar

2. **Production guide** (`guia-produccion.md`):
   - Figma Static: templates, brand colors, typography, dimensions, assigned posts
   - ElevenLabs: voice type, target duration, script template, assigned posts
   - Replit Animation: animation types, duration, assigned posts
   - Video: what to record, script, backup plan

3. **Quick ID reference**: campaign IDs, post counts, document links

4. **Final verification**: confirm totals match LexGuard, verify no posts without media, verify no posts without customColumns

---

## Content Creation Best Practices

### Writing Style (Mexican Spanish)

- **Natural and direct**: talk like a knowledgeable friend, not a corporation
- **Empathetic**: acknowledge the reader's pain before offering solutions
- **No corporate jargon**: avoid "sinergias", "optimizar", "soluciones integrales"
- **Each post works independently**: don't rely on other posts for context
- **Vary formats**: static images, carousels (3–7 slides), short reels (15–30 sec)
- **Strong hooks**: the first line must stop the scroll
- **Clear CTA**: every post ends with a specific call to action
- **5–10 hashtags**: mix of broad reach and niche targeting

### Copy Structure

**For feed posts**:
- Hook (1 line — grab attention)
- Problem (2–3 lines — empathize)
- Solution (2–3 lines — introduce the product)
- Proof (1–2 lines — data, testimonial, or result)
- CTA (1 line — what to do next)
- Hashtags (5–10)

**For carousels**:
- Slide 1: Bold hook question or statement
- Slides 2–5: One point per slide, visual + text
- Last slide: CTA with clear next step

**For reels**:
- Hook (0–3 sec): attention grabber
- Problem (3–8 sec): relate to the pain
- Solution (8–20 sec): show the product
- CTA (20–30 sec): tell them what to do

### A/B Testing Variants

For matrix-based campaigns, write two variants per combination:
- **Variant A**: Direct, data-driven, aggressive — leads with numbers and urgency
- **Variant B**: Empathetic, story-driven, educational — leads with a scenario or question

---

## Marketing Metrics & Analysis

### Campaign Performance KPIs

Track these metrics to evaluate campaign health:

| Metric | What it measures | How to check in LexGuard |
|--------|-----------------|--------------------------|
| **Lead volume** | Total leads per campaign | **search_leads** filtered by campaign |
| **Pipeline velocity** | How fast leads move through stages | **get_campaign_statuses** + **search_leads** by status |
| **Content coverage** | % of content matrix filled | **list_posts** + analyze customColumns distribution |
| **Production progress** | % of posts with media assigned | **list_posts** + **list_post_media** |
| **Sprint completion** | % of tasks done per sprint | **list_sprint_tasks** by status |
| **Platform balance** | Instagram vs Facebook distribution | **list_posts** → count by platform |
| **Message response rate** | Outbound vs confirmed messages | **get_pending_messages** |

### Coverage Analysis

When analyzing content matrix coverage, build a cross-reference table:

1. Pull all posts with **list_posts** per campaign
2. Extract customColumns values from each post
3. Create a pivot: rows = pain points, columns = tones/angles
4. Count posts per cell
5. Flag cells with 0 (gaps) or 1 (thin coverage — needs variants)
6. Target: 3+ variants per pain point, 2+ tones per campaign

### Sprint Velocity

Track across sprints to measure team productivity:
- Tasks completed per sprint
- Average time from "todo" to "done"
- Blockers frequency and resolution time
- Post production rate (posts with media / total posts)

### Funnel Analysis

Map the customer journey through campaign stages:
1. Pull leads by campaign: **search_leads** with campaign filter
2. Group by status: count leads at each pipeline stage
3. Calculate conversion rates between stages
4. Identify the biggest drop-off point
5. Recommend: which campaigns need more content, which pipeline stages need better nurturing

---

## Campaign Types & When to Use Them

| Type | Purpose | Best for | Content style |
|------|---------|----------|---------------|
| **TOFU** | Build awareness, educate | Cold audiences, new markets | Educational, problem-focused |
| **BOFU** | Convert warm leads | Retargeting, objection handling | Direct, proof-heavy, urgent |
| **Social Proof** | Build trust | All funnel stages | Testimonials, case studies, results |
| **B2B** | Target businesses | Industry-specific outreach | Professional, ROI-focused |
| **Retention** | Keep existing customers | Current users | Tips, updates, community |
| **Seasonal** | Time-sensitive offers | Holiday/event periods | Urgent, promotional |

---

## Production Method Guide

### Figma Static (55–65% of content)
- Best for: data points, quotes, feature highlights, simple CTAs
- Formats: 1080×1080 (feed), 1080×1350 (portrait), 1080×1920 (stories)
- Keep text readable: max 20% of image area
- Brand colors and typography are non-negotiable

### ElevenLabs Voice-Over (15–25%)
- Best for: UGC-style testimonials, AI-generated customer stories
- Target duration: 15–30 seconds
- Script template: hook → pain → solution → CTA
- Voice: natural, conversational Mexican Spanish

### Replit Animation (10–15%)
- Best for: data visualizations, before/after comparisons, process demos
- Keep animations under 15 seconds
- Focus on one message per animation

### Video (0–5%)
- Only when face-to-face authenticity is essential
- Requires real people or studio setup
- Always have a backup plan (convert to carousel if video can't be produced)

---

## Strategic Frameworks

### Content Matrix Design

A content matrix crosses multiple dimensions to generate a complete set of post combinations:

```
Pain Points × Tones × Platforms × Production Methods = Total Post Variants
```

Design the matrix so that:
- Every pain point has at least 3 variants
- Every tone appears at least 2× per campaign
- Platform split stays near 60/40 (Instagram/Facebook)
- Production method allocation matches the targets above
- No single dimension dominates (balance variety)

### Funnel Architecture

Design campaigns to cover every stage:

```
Awareness (TOFU) → Consideration (MOFU) → Decision (BOFU) → Loyalty (Retention)
         ↑                    ↑                    ↑
    Social Proof         Social Proof         Social Proof
```

Social proof feeds into every stage. Cross-posting between campaigns strengthens the funnel.

### Retargeting Strategy

- TOFU viewers who engage → retarget with BOFU
- BOFU viewers who don't convert → retarget with Social Proof
- Leads who go cold → automated follow-up via pipeline
- Time-based: re-engage after 3, 7, and 14 days of inactivity
