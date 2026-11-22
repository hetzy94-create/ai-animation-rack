# AI YouTube Rack — Requirements Summary (Final Revised)

*Consolidated from Stages 1–8 with all corrections. This is a requirements document, not technical architecture.*

---

## 1. Project Overview

**AI YouTube Rack** is a unified system for building and managing a faceless YouTube channel. It has two core engines that share infrastructure but maintain separate responsibilities:

- **Competitor Intelligence Engine** — Studies reference channels (starting with Bernard Animation) to discover evidence-based content patterns.
- **Video Production Engine** — Uses those patterns as *guidance* (not rules) to create original content, then learns from the channel's own performance.

**Core philosophy:** Research fuels creation. Both engines learn continuously. Human creative control is preserved at key decisions. Simplicity and cost-efficiency are constraints, not afterthoughts.

---

## 2. Main Engines

### Competitor Intelligence Engine

| Aspect | Details |
|--------|---------|
| **Responsibility** | Research, observe, analyze, learn from reference channels |
| **Inputs** | Public YouTube data: metadata, transcripts, (selectively) visuals/audio |
| **Outputs** | Organized observations, recurring patterns, hypotheses, validated conclusions, reports, recommendations — all **source-traceable** with confidence levels |
| **Key behaviors** | Incremental processing, selective deep analysis, continuous monitoring, versioned knowledge |

### Video Production Engine

| Aspect | Details |
|--------|---------|
| **Responsibility** | Develop and produce original videos for your channel |
| **Inputs** | Your ideas, research guidance from Intelligence Engine, your creative identity/style guide, your performance analytics |
| **Outputs** | Video ideas, evaluated topics, research briefs, scripts, storyboards, asset plans, finished videos, publishing packages, performance insights |
| **Key behaviors** | 10-stage pipeline (A–J), human gates at creative decisions, learns from own analytics (Loop 2) |

---

## 3. Shared vs. Separate Responsibilities

### Shared Infrastructure (allowed)
- Storage layer, scheduling, caching, API clients, prompt/skill library, logging, notification system
- **General lessons** extracted from research — *but with source traceability intact*
  - *Example:* "Question-hook openings appear in 12 of the 15 highest-view-count videos [source: Bernard #3, #7, #12, #15, #18, #21, #23, #27, #31, #34, #42, #45; confidence: pattern]"
  - **Clearly labeled as correlation** — public view counts are a proxy, not proof of causation. Private retention data is not accessible.

### Must Remain Separate (no mixing)

| Intelligence Engine Data | Production Engine Data |
|--------------------------|------------------------|
| Competitor video records | Your scripts & storyboards |
| Competitor observations/patterns | Your asset library |
| Competitor performance proxies (public views) | Your private analytics (retention, CTR, demographics) |
| Hypotheses about *their* strategy | Your creative identity / style constitution |

**Rule:** Traceability > convenience. If a lesson loses its source, it loses credibility.

---

## 4. Research Requirements

### Five Research Levels (match level to question)

| Level | Content | Cost | Use when... |
|-------|---------|------|-------------|
| **A — Metadata** | Title, description, date, duration, views, likes, thumbnail | ~Zero (API) | Channel overview, publishing patterns, topic frequency |
| **B — Transcript** | Full spoken text, hook, structure, narration, CTA | Low (API/Whisper) | Storytelling analysis, hook comparison, script structure |
| **C — Visual** | Scenes, composition, animation, characters, transitions | High (vision API) | Visual technique questions, thumbnail analysis, animation style |
| **D — Audio** | Voice, music, SFX, timing, sync | Medium | Branding sounds, pacing, audio-visual sync |
| **E — Combined** | Cross-modal: how script+visuals+audio work together | Very high | Only when question *requires* it; expensive |

### Selective Deep Analysis
- **Default:** Level A for all videos (cheap, complete library)
- **Deep (B–E):** Only for selected videos — **explainable selection criteria:**
  - High-performing outliers
  - Format experiments / strategy shifts
  - Temporal boundaries (first, recent, series edges)
  - Pattern gaps (need negative examples too)
- Stop when new videos add no new patterns

### Incremental Processing & Caching
- **Check existence** → skip or refresh metadata only
- **Cache with provenance:** Transcripts stored with `source`, `retrieval_date`, `version` (where available). Visual analysis outputs cached with model version.
- **Reuse by default** — but **allow rechecking** when: source updates, model improves, or explicit re-analysis requested.
- **Update patterns** only when new evidence confirms/contradicts/extends — versioned, not overwritten

### Evidence, Confidence & Causation
Every finding carries:
- `source_level` (A–E)
- `source_video_ids`
- `confidence`: `observation` | `pattern` | `hypothesis` | `validated`
- `confirmation_count`
- `sample_size` (number of videos supporting)
- `source_diversity` (how many distinct videos/channels)
- `last_confirmed_date`
- `limitations` (e.g., "no retention data," "correlation only")
- `contradictory_findings` (if any)

**Causation rule:** Public competitor data **cannot establish causation** between a creative technique and audience retention. All conclusions from competitor research are **correlative**. The system must explicitly distinguish:
- **Correlation** — "Pattern X appears in high-performing videos"
- **Causation** — "Pattern X *causes* higher retention" → **never claimed from public data**

**Automation guard:** A numerical AI confidence score **alone must not** authorize automatic changes to important conclusions. Automation decisions must also consider:
- Evidence quality
- Sample size
- Source diversity
- Contradictory findings
- The importance and reversibility of the action

**Important conclusions must still require human approval** — hypothesis → validated transitions are never automatic.

### Pattern Discovery & Knowledge Updates
- Observation → Pattern (repeated across N videos) → Hypothesis (testable why) → Validated (evidence-supported)
- **Never auto-escalate** — human validates hypothesis→validated
- Historical versions preserved
- Contradictions flagged, not silently resolved

### Cost & Resource Efficiency
- **Tiered API usage:** Free (YouTube Data API) → Cheap (Gemini Flash, GPT-4o-mini) → Expensive (GPT-4o, vision) only for selected videos
- **Never assume free** — verify current pricing before any paid usage; **explicit approval required** before enabling paid APIs/services
- Local code for: scheduling, dedup, file mgmt, calculations, format conversion, API polling
- AI only for: interpretation, creativity, judgment, "good taste" tasks
- Batch/overnight processing; queue-based

### Local AI
- **Not ruled out** — kept as **optional future possibility**
- Heavy local AI workloads may be impractical on current hardware (i7-4th gen, 16GB RAM), but **smaller models and other local AI tools can be evaluated through practical testing when justified**
- Decision deferred until implementation phase

---

## 5. Research Depth — Adaptive Deep Research (Confirmed Strategy)

*This confirms the research depth strategy, NOT the technical data collection method.*

- Every reference-channel video receives **basic research** (Level A metadata).
- Only **selected videos** receive **deeper research**.
- Deep research can include: transcripts, video-frame retrieval, visual analysis, audio analysis, and combined multimodal analysis (Levels B–E).
- **Video selection must use explainable criteria**, including:
  - Performance outliers (both high and low)
  - Format changes and experiments
  - Research gaps (missing pattern examples)
  - Temporal diversity (early, middle, recent periods)
- **Include both successful and less successful examples** — negative cases are valuable for pattern discrimination.
- The system should **avoid unnecessary repeated analysis** through caching and incremental processing.

**Important distinction:** Web scraping, official YouTube API access, video retrieval, frame extraction, and other collection methods remain **open for evaluation**. Their feasibility, cost, reliability, and applicable platform rules must be evaluated before selection. None are prohibited or automatically approved.

---

## 6. Production Requirements

### 10-Stage Pipeline (A–J)

| Stage | Name | Key Output | Human Gate? |
|-------|------|------------|-------------|
| A | Idea discovery | Topic candidates | You pick |
| B | Topic evaluation | Scored/ranked shortlist | You approve |
| C | Research | Research brief with sources | You verify |
| D | Script development | Draft script | You edit/approve |
| E | Storyboard & visual planning | Scene breakdown + visual plan | You refine |
| F | Asset preparation | Organized/created assets | Auto-organize; create→ask |
| G | Video production | Assembled video | Template→conditional; creative→assisted |
| H | Quality control | Issue list + pass/fail | You judge |
| I | Publishing preparation | Title, thumbnail, description, tags | **You hit publish** |
| J | Performance learning | Analytics insights → refined decisions | You interpret |

### Loop 2: Your Channel Learning
- Ingest **private YouTube Studio analytics** (retention graphs, CTR, traffic sources, demographics)
- Compare against production decisions → refine creative choices
- Feed back into Stages A–B for next video

### Originality & Copyright Constraints
- Reference channel = learning source, **not template**
- **Learn general creative principles** (storytelling structures, pacing techniques, hook formats, visual grammar) — **do not reproduce** competitor's:
  - Scripts or substantial text passages
  - Characters, character designs, or distinctive visual identities
  - Music, sound effects, or audio branding
  - Branding elements (logos, intros, outros, catchphrases)
  - Specific creative expressions protected by copyright
- **Creative identity document** (style constitution) defines: your voice, visual style, values, boundaries
- Production Engine checks against this before applying any competitor-derived suggestion
- **Transformative use** — insights must be adapted into original expression

---

## 7. Automation Requirements

### Four Levels (applied per-task, not globally)

| Level | Name | Behavior |
|-------|------|----------|
| 1 | Manual assistance | System informs → you decide everything |
| 2 | Assisted execution | System prepares + recommends → you approve important actions |
| 3 | Conditional automation | Auto-executes low-risk tasks → asks on important/uncertain decisions |
| 4 | High autonomy | Plans + executes multi-step → reports results/exceptions |

### Suggested Mapping (Intelligence Engine)

| Task | Level | Notes |
|------|-------|-------|
| Check new uploads | 3 | Scheduled, low-risk |
| Collect metadata | 3 | Cheap, deterministic |
| Retrieve transcripts | 2–3 | Auto if cached; ask if new cost |
| Select videos for deep research | 2 | System proposes, you approve |
| Analyze videos (B/C/D) | 2 | AI cost + interpretation |
| Update observations | 3 | If high-confidence confirmation |
| Create hypotheses | 1–2 | Creative leap — you in loop |
| Change established conclusions | 1 | **Never auto-change** validated patterns |
| Send reports | 3 | Routine; Level 2 if flags surprises |

### Suggested Mapping (Production Engine)

| Task | Level | Notes |
|------|-------|-------|
| Idea generation | 2 | System suggests → you pick |
| Topic evaluation | 2 | System scores → you decide |
| Research gathering | 3 | Fetch sources → you review |
| Script drafting | 2 | System writes → you edit/approve |
| Storyboard creation | 2 | System proposes → you refine |
| Asset preparation | 3 | Organize→auto; create→ask |
| Video assembly | 2–3 | Template→conditional; creative→assisted |
| Quality control | 2 | System flags → you judge |
| Publishing | 1–2 | **Never auto-publish** |
| Performance review | 2 | System surfaces → you interpret |

### Hard Boundaries (Never Without Explicit Permission)
- **Publishing/uploading** to YouTube
- **Changing validated research conclusions**
- **Modifying creative identity / style constitution**
- **Spending money** (API calls beyond free tier, paid services)
- **Deleting or overwriting** your creative assets
- **Committing to a video topic** for production

### Error Handling, Audit, Rollback
- **Audit log:** Every automated action — timestamp, trigger, inputs, decision logic, output
- **Confidence thresholds:** Your policy (e.g., "auto-update pattern if confidence > 0.85 and confirmations > 5 and no contradictions")
- **Rollback:** "Revert last auto-action" capability
- **Notification design:** Batch routine; surface only exceptions, surprises, decisions-needed
- **Learning from corrections:** When you override, system records preference for future

---

## 8. Technical & Practical Constraints

| Constraint | Implication |
|------------|-------------|
| **Windows** | Native tooling, PowerShell-friendly, no Linux-only deps |
| **Intel i7 4th gen, 16GB RAM** | Heavy local AI workloads may be impractical; API-based reasoning for now; local code for orchestration |
| **Free/very low budget** | Tiered API strategy; aggressive caching; avoid paid services until proven value; **explicit approval for any paid usage** |
| **Beginner-friendly** | Clear CLI or simple UI; readable logs; no opaque black boxes; good error messages |
| **Avoid unnecessary complexity** | Start with minimal orchestration; add agents only when autonomy genuinely needed |
| **Prefer ordinary code** | Deterministic tasks = code; judgment tasks = AI |
| **Maintainable** | Versioned data, documented skills, testable components |

---

## 9. Provider Independence

- **No lock-in** to Claude Code, one AI model, or one external provider
- Important data (research, scripts, storyboards, assets, analytics) stored in **open, portable formats** (JSON, Markdown, plain text, standard media formats)
- Workflows expressed in **provider-agnostic logic** (skills as plain instructions, not framework-specific code)
- API clients abstracted behind interfaces — swappable without rewriting orchestration
- If provider changes, the project continues with minimal migration

---

## 10. Backup & Recovery

- **Automated backups** of all project-critical data:
  - Research database (observations, patterns, hypotheses, conclusions, source traces)
  - Production artifacts (scripts, storyboards, asset manifests, creative identity)
  - Configuration (skills, prompts, thresholds, automation policies)
  - Audit logs
- **Backup frequency:** Daily incremental; weekly full
- **Storage:** At least two independent locations (local + cloud/external)
- **Recovery requirements:**
  - Point-in-time restore for research database
  - Single-file recovery for scripts/storyboards/assets
  - Corruption detection (checksums/integrity verification)
  - Tested restore procedure (quarterly drill)
- **Accidental deletion protection:** Soft-delete with 30-day retention before purge

---

## 11. Missing Decisions

### Must Answer Before Architecture Design

| Question | Why It Blocks Architecture |
|----------|---------------------------|
| **Storage strategy?** Files (JSON/MD) vs SQLite vs vector DB? | Determines data model, query patterns, versioning approach, backup method |
| **Interface?** CLI, local web UI, VS Code extension, Markdown files? | Shapes how you interact, where human gates live |
| **Scheduling/triggering?** Cron, YouTube push notifications, manual, event-driven? | Determines orchestration backbone |
| **YouTube data access method?** Official API (quota-limited) vs yt-dlp vs hybrid? | Affects cost, reliability, legal compliance, ToS adherence |
| **Transcript source?** YouTube captions (free, spotty) vs Whisper API (cost) vs local Whisper (hardware)? | Cost/quality tradeoff for Level B |
| **Creative identity document format?** What fields? How enforced? | Production Engine needs this as input |
| **Secrets management?** Where do API keys, OAuth tokens live? | Security baseline |

### Can Decide During Implementation

| Question | Notes |
|----------|-------|
| Specific AI models per task (which cheap model for what) | Can swap later |
| Exact caching TTLs, file naming conventions | Iterate |
| Notification channels (console, file, webhook, email) | Start simple |
| Batch vs. streaming processing for new uploads | Profile first |
| Visual analysis sampling strategy (keyframes vs. scene boundaries vs. fixed interval) | Experiment |

### Optional Future Improvements

- Cross-channel comparison (add 2–3 more reference channels)
- Comment mining for qualitative signals
- Thumbnail A/B tracking via historical capture
- Series/playlist-level analysis
- Local model experimentation (if hardware upgrades)
- Collaborative features (if team grows)

---

## 12. Potential Risks

| Risk | Impact | Mitigation Idea |
|------|--------|-----------------|
| **YouTube API quota limits** (10k/day) | Can't scan full channel daily | Hybrid: API for metadata, yt-dlp for transcripts; cache aggressively |
| **Transcript gaps/errors** | Level B analysis unreliable for some videos | Whisper fallback budget; flag low-confidence transcripts |
| **Visual analysis cost explosion** | $6–30/video at 1fps vision API | Smart sampling (scene boundaries, keyframes only); thumbnail-only track |
| **Data availability drift** | YouTube hides/changes public metrics | Design for graceful degradation; don't build on fragile fields |
| **Survivorship bias** | Only see published videos, not failed experiments | Explicitly seek negative examples; track "missing" patterns |
| **Pattern staleness** | 2022 patterns ≠ 2024 effectiveness | Timestamp + "last confirmed" on every pattern; decay logic |
| **No ground truth for causality** | Can't prove "hook caused retention" | All conclusions correlative; explicit `limitation` tags |
| **Creative homogenization** | Over-reliance on competitor patterns → generic output | Style constitution as hard constraint; human gate on creative decisions |
| **Notification fatigue** | Level 3 reports drown real signals | Batch routine; surface only exceptions/decisions-needed |
| **Single point of failure (you)** | If you're unavailable, Level 3 queues/stalls | Define "safe defaults" for common uncertainties |
| **Cost creep** | API usage grows unnoticed | Budget tracking per workflow run; hard monthly caps; **explicit approval for paid usage** |
| **Legal/ToS compliance** | Scraping, data storage, API usage | Review YouTube ToS; store only what's necessary; respect robots.txt |
| **Provider lock-in** | Can't migrate if Claude Code/API changes | Open formats, abstracted interfaces, portable skills |
| **Data loss** | Hardware failure, accidental deletion, corruption | Automated backups, tested recovery, soft-delete retention |

---

## 13. Premature Technology Avoidance

**The following are NOT assumed necessary. Each must be justified by demonstrated need before adoption:**

- ❌ Vector database
- ❌ Multiple AI agents
- ❌ Complex scheduling framework
- ❌ Web interface / dashboard
- ❌ Message queue / event bus
- ❌ Container orchestration
- ❌ Graph database
- ❌ Custom model fine-tuning

**Tentative starting recommendation (not a decision):** Files + SQLite + cron + single orchestration script. **No storage system, scheduling method, interface, or architecture has been approved yet.** These will be evaluated collaboratively before any technical decision is finalized.

---

## Undecided / Open Questions (Explicitly Marked)

- [ ] Storage backend
- [ ] Primary interface
- [ ] Scheduling mechanism
- [ ] YouTube data access method
- [ ] Transcript strategy
- [ ] Creative identity schema
- [ ] Secrets management
- [ ] Exact confidence thresholds for Level 3 automation
- [ ] Visual analysis sampling strategy
- [ ] Whether to start with 1 reference channel or 2–3
- [ ] Backup storage locations (local + which cloud/external)
- [ ] Recovery test schedule

---

## Summary of Four Corrections Made

1. **Section 3 (Shared Infrastructure example)** — Replaced the "retention correlation" example with a public-view-count example ("Question-hook openings appear in 12 of the 15 highest-view-count videos"), explicitly labeled as **correlation only**, since private retention data is not accessible from public sources.

2. **Section 6 (Confidence Thresholds)** — Clarified that numerical confidence scores and confirmation counts **alone are not sufficient** to authorize automatic changes. Automation decisions must also weigh evidence quality, sample size, source diversity, contradictory findings, and the importance/reversibility of the action. **Important conclusions still require human approval.**

3. **Section 7 (Local AI)** — Removed the framing that current hardware permanently rules out local LLMs. Instead: heavy local AI workloads may be impractical on current hardware, but **smaller models and other local AI tools can be evaluated through practical testing when justified**. Decision deferred to implementation.

4. **Section 13 (Premature Technology Avoidance)** — Changed "Start with files + SQLite + cron + single orchestration script" from a **default decision** to a **tentative starting recommendation**. Explicitly stated: **No storage system, scheduling method, interface, or architecture has been approved yet.** These will be evaluated collaboratively before any technical decision is finalized.

---

## Confirmation

**No architecture decisions have been finalized.** This document remains a requirements summary only. All technical choices (storage, interface, scheduling, data access methods, models, etc.) are explicitly marked as undecided and will be evaluated collaboratively before implementation begins.