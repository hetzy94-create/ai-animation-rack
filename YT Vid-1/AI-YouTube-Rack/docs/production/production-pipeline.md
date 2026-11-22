# Production Pipeline (Stages A–J)

*Derived from: `docs/project/requirements-summary.md` (Section 6)*

---

## 10-Stage Pipeline Overview

| Stage | Name | Key Output | Human Gate | Automation Level |
|-------|------|------------|------------|------------------|
| A | Idea discovery | Topic candidates | You pick | 2 (assisted) |
| B | Topic evaluation | Scored/ranked shortlist | You approve | 2 (assisted) |
| C | Research | Research brief with sources | You verify | 3 (conditional) |
| D | Script development | Draft script | You edit/approve | 2 (assisted) |
| E | Storyboard & visual planning | Scene breakdown + visual plan | You refine | 2 (assisted) |
| F | Asset preparation | Organized/created assets | Auto-organize; create→ask | 3 (conditional) |
| G | Video production | Assembled video | Template→conditional; creative→assisted | 2–3 |
| H | Quality control | Issue list + pass/fail | You judge | 2 (assisted) |
| I | Publishing preparation | Title, thumbnail, description, tags | **You hit publish** | 1–2 (manual/assisted) |
| J | Performance learning | Analytics insights → refined decisions | You interpret | 2 (assisted) |

---

## Stage Details

### A — Idea Discovery
**Input:** Your interests, competitor research patterns, trending topics, audience requests
**Output:** List of topic candidates with brief notes
**Human role:** Curate, add personal ideas, reject misaligned topics
**Automation:** System suggests based on patterns + your history

### B — Topic Evaluation
**Input:** Topic candidates from Stage A
**Process:** Score on audience interest, originality, research availability, production difficulty, competitor coverage gaps
**Output:** Ranked shortlist with scores and rationale
**Human role:** Review scores, adjust weights, make final selection
**Automation:** System calculates scores; you approve

### C — Research
**Input:** Selected topic
**Process:** Gather reliable sources, fact-check, organize key points, identify visual/audio needs
**Output:** Research brief with citations, fact database, asset requirements
**Human role:** Verify accuracy, add domain knowledge, approve completeness
**Automation:** Fetch sources, summarize, organize (conditional)

### D — Script Development
**Input:** Research brief, competitor hook/structure patterns, your style constitution
**Process:** Draft script with hook, structure, pacing, narration, CTA
**Output:** Draft script (versioned)
**Human role:** Edit, rewrite, approve — **creative control here**
**Automation:** First draft generation; you refine

### E — Storyboard & Visual Planning
**Input:** Approved script, competitor visual patterns, your visual identity
**Process:** Break into scenes, specify visuals/animation/characters/transitions per scene
**Output:** Scene-by-scene storyboard with visual specifications
**Human role:** Refine visual choices, approve plan
**Automation:** Scene breakdown suggestion; visual reference lookup

### F — Asset Preparation
**Input:** Storyboard specifications
**Process:** Organize existing assets, create/generate new assets (images, audio, characters, backgrounds)
**Output:** Organized asset library with manifest (file paths, specs, licenses)
**Human role:** Approve created assets; verify licenses
**Automation:** Auto-organize existing; create→ask for new

### G — Video Production
**Input:** Script, storyboard, asset manifest
**Process:** Assemble scenes, animation, narration, sound, editing → complete video
**Output:** Rendered video file(s)
**Human role:** Creative decisions during assembly; approve final render
**Automation:** Template-based assembly (conditional); creative steps (assisted)

### H — Quality Control
**Input:** Rendered video, script, storyboard, style constitution
**Process:** Check storytelling, pacing, audio sync, visual consistency, factual accuracy, brand compliance
**Output:** Issue list + pass/fail decision
**Human role:** Judge issues, make final pass/fail call
**Automation:** Automated checks (audio levels, duration, format); creative review (assisted)

### I — Publishing Preparation
**Input:** Approved video, competitor title/thumbnail patterns, your style constitution
**Process:** Create title, thumbnail, description, tags, playlist assignment, schedule
**Output:** Complete publishing package
**Human role:** **You hit publish** — never auto-publish
**Automation:** Draft suggestions based on patterns; you approve

### J — Performance Learning
**Input:** Published video, YouTube Studio analytics (retention, CTR, traffic sources, demographics)
**Process:** Compare performance vs. expectations, identify what worked/didn't, extract lessons
**Output:** Performance insights → feed back to Stages A–B
**Human role:** Interpret insights, decide adjustments
**Automation:** Surface anomalies and patterns; you interpret

---

## Loop 2: Your Channel Learning

```
Your Published Videos
         │
         ▼
Private YouTube Studio Analytics
  (retention graphs, CTR, traffic sources, demographics)
         │
         ▼
Compare Against Production Decisions
  (hook type, structure, topic, length, thumbnail style, etc.)
         │
         ▼
Refined Creative Choices for Next Video
         │
         ▼
Feed Back to Stages A–B
```

**Key difference from competitor research:** You have **private analytics** (retention, CTR) enabling stronger causal insights.

---

## Originality & Copyright Constraints

| Do Learn (General Principles) | Don't Reproduce (Protected Expression) |
|-------------------------------|----------------------------------------|
| Storytelling structures (3-act, hero's journey, etc.) | Specific scripts or substantial passages |
| Pacing techniques (hook→build→payoff) | Characters, character designs |
| Hook formats (question, statement, visual) | Distinctive visual identities |
| Visual grammar (composition, color theory) | Music, sound effects, audio branding |
| Thumbnail psychology (contrast, curiosity) | Logos, intros, outros, catchphrases |

**Rule:** Insights must be **transformatively adapted** into original expression.

**Creative Identity Document (Style Constitution)** — UNDECIDED format, but must define:
- Your voice/tone guidelines
- Visual style parameters
- Values and boundaries
- Do-not-cross lines

Production Engine checks against this before applying any competitor-derived suggestion.

---

## Automation Levels by Stage (Summary)

| Level | Description | Stages |
|-------|-------------|--------|
| 1 | Manual assistance — you decide everything | I (publish) |
| 2 | Assisted — system prepares, you approve | A, B, D, E, H, I (prep), J |
| 3 | Conditional — auto low-risk, ask on important | C, F, G (template parts) |

---

## Related Documents

- `docs/project/requirements-summary.md` — Authoritative requirements (Section 6)
- `docs/operations/security-and-approvals.md` — Human gates, audit, rollback
- `docs/research/evidence-and-confidence.md` — How competitor patterns feed production
- `templates/production/` — Script, storyboard, QC templates (to be created)