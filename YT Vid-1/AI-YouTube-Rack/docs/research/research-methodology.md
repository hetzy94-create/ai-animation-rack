# Research Methodology

*Derived from: `docs/project/requirements-summary.md` (Sections 4, 5)*

---

## Five Research Levels

| Level | Content | Cost | Use When |
|-------|---------|------|----------|
| **A — Metadata** | Title, description, date, duration, views, likes, thumbnail | ~Zero (API) | Channel overview, publishing patterns, topic frequency |
| **B — Transcript** | Full spoken text, hook, structure, narration, CTA | Low (API/Whisper) | Storytelling analysis, hook comparison, script structure |
| **C — Visual** | Scenes, composition, animation, characters, transitions | High (vision API) | Visual technique questions, thumbnail analysis, animation style |
| **D — Audio** | Voice, music, SFX, timing, sync | Medium | Branding sounds, pacing, audio-visual sync |
| **E — Combined** | Cross-modal: how script+visuals+audio work together | Very high | Only when question *requires* it; expensive |

**Principle:** Match level to question. Don't climb the ladder for every video.

---

## Selective Deep Analysis

### Default Coverage
- **Level A (metadata)** for ALL videos in reference channel — cheap, complete library

### Deep Analysis (Levels B–E) — Selection Criteria
Only selected videos receive deeper research. Selection must be **explainable**:

1. **Performance outliers** — Both high AND low performers (negative cases are valuable)
2. **Format changes & experiments** — New structure, topic, style, series boundaries
3. **Research gaps** — Missing pattern examples (e.g., "we have 10 hook type A, 0 type B")
4. **Temporal diversity** — Early, middle, recent periods to detect evolution

### Stopping Rule
Stop deep analysis when new videos add no new patterns (diminishing returns).

---

## Incremental Processing & Caching

### Workflow
```
New video detected
       │
       ▼
┌──────────────────┐
│ Check: already   │──Yes──► Skip (or refresh metadata only)
│ in records?      │
└────────┬─────────┘
         │No
         ▼
┌──────────────────┐
│ Fetch metadata   │──Cache metadata (Level A)
│ (Level A)        │
└────────┬─────────┘
         ▼
┌──────────────────┐
│ Need transcript? │──No──► Use cached transcript
│ (Level B)        │
└────────┬─────────┘
         │Yes
         ▼
┌──────────────────┐
│ Fetch transcript │──Cache with provenance
│ (API/Whisper)    │   (source, retrieval_date, version)
└────────┬─────────┘
         ▼
... continue for Levels C/D/E if selected ...
```

### Cache Provenance (Required)
Every cached item carries:
- `source` — where it came from (API, yt-dlp, Whisper, etc.)
- `retrieval_date` — when fetched
- `version` — source version if available (e.g., caption track version)
- `model_version` — for AI-generated analysis (visual, audio, combined)

### Reuse by Default, Recheck When Needed
- **Reuse** cached data by default
- **Allow rechecking** when: source updates, model improves, explicit re-analysis requested
- **Update patterns** only when new evidence confirms/contradicts/extends — versioned, not overwritten

---

## Video Selection: Explainable Criteria

The system must document *why* each video was selected for deep analysis. Examples:

| Criterion | Example Selection Logic |
|-----------|------------------------|
| High outlier | "Top 5% by views in last 6 months" |
| Low outlier | "Bottom 10% by views — understand what didn't work" |
| Format change | "First video with new intro format (detected via visual analysis)" |
| Series boundary | "First/last video of 'History Mysteries' series" |
| Pattern gap | "No examples of 'listicle' format in deep set — adding 2" |
| Temporal | "One video from each quarter of 2023" |

**Both successful and less successful examples must be included** — negative cases enable pattern discrimination.

---

## Data Collection Methods: UNDECIDED

The following remain open for evaluation (feasibility, cost, reliability, ToS compliance):

- [ ] Official YouTube Data API v3
- [ ] yt-dlp / similar tools
- [ ] Hybrid approach (API for metadata, yt-dlp for transcripts/frames)
- [ ] Web scraping
- [ ] Official transcript API vs. Whisper (cloud/local) for transcription
- [ ] Frame extraction strategies (keyframes, scene boundaries, fixed interval)
- [ ] Vision API providers for visual analysis

**No method is prohibited or automatically approved.** Evaluation required before selection.

---

## Related Documents

- `docs/project/requirements-summary.md` — Authoritative requirements (Sections 4, 5)
- `docs/research/evidence-and-confidence.md` — Evidence standards, confidence levels, causation rules
- `docs/integrations/youtube/README.md` — YouTube integration notes