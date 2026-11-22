# Evidence Standards & Confidence Levels

*Derived from: `docs/project/requirements-summary.md` (Sections 4, 6)*

---

## Required Metadata for Every Finding

Every research finding must carry:

| Field | Description | Example |
|-------|-------------|---------|
| `source_level` | Research level (A–E) | `B` |
| `source_video_ids` | Video IDs this finding derives from | `["Bernard#12", "Bernard#23", "Bernard#45"]` |
| `confidence` | Confidence tier | `pattern` |
| `confirmation_count` | Number of confirming instances | `12` |
| `sample_size` | Total videos examined for this pattern | `15` |
| `source_diversity` | Distinct videos/channels supporting | `12 videos, 1 channel` |
| `last_confirmed_date` | When last verified | `2024-11-20` |
| `limitations` | Known constraints | `"correlation only; no retention data"` |
| `contradictory_findings` | Any opposing evidence | `"Bernard#31 uses different hook with high views"` |

---

## Confidence Tiers (Escalation Path)

```
Observation → Pattern → Hypothesis → Validated
     │            │            │           │
     ▼            ▼            ▼           ▼
  Single     Repeated    Testable    Evidence-
  instance   across N    explanation supported
  video      videos      (why?)      conclusion
```

| Tier | Definition | Minimum Evidence | Automation Allowed? |
|------|------------|------------------|---------------------|
| **Observation** | Single fact seen in one video | 1 video | Read-only |
| **Pattern** | Same observation across multiple videos | ≥3 videos, explainable selection | Auto-update if high-confidence confirmation only |
| **Hypothesis** | Testable explanation for *why* pattern exists | Pattern + reasoned mechanism | **Never auto-escalate** |
| **Validated** | Hypothesis supported by sufficient evidence | Pattern + evidence + human validation | **Human approval required** |

**Critical Rule:** Never auto-escalate. Human must validate Hypothesis → Validated.

---

## Causation vs. Correlation (Hard Rule)

### Public Competitor Data Cannot Establish Causation

| Claim Type | Example | Allowed from Public Data? |
|------------|---------|---------------------------|
| **Correlation** | "Question-hook openings appear in 12 of 15 highest-view-count videos" | ✅ Yes — with `limitation: "correlation only"` |
| **Causation** | "Question hooks *cause* higher retention" | ❌ **Never** — private retention data not accessible |

**Every finding from competitor research must explicitly carry:** `limitation: "correlation only; no retention data"` (or equivalent).

**Your own channel analytics (Loop 2)** can support stronger causal claims because you have access to retention graphs, CTR, etc. — but even then, correlation ≠ causation without controlled experiments.

---

## Automation Guards for Important Conclusions

A numerical AI confidence score **alone is not sufficient** to authorize automatic changes to important conclusions.

### Automation Decision Must Weigh:

1. **Evidence quality** — Level of research (A–E), source reliability
2. **Sample size** — Number of videos supporting the pattern
3. **Source diversity** — How many distinct videos/channels
4. **Contradictory findings** — Any opposing evidence (must be flagged, not ignored)
5. **Importance & reversibility** — What happens if wrong? Can it be undone?

### Human Approval Required For:

- Hypothesis → Validated transitions
- Changes to established validated conclusions
- Any conclusion that affects creative identity / style constitution
- Any conclusion that triggers production decisions

### Example Policy (Template — UNDECIDED)

```
Auto-update pattern IF:
  confidence == "pattern" AND
  confirmation_count >= 5 AND
  sample_size >= 10 AND
  source_diversity >= 3 AND
  contradictory_findings == 0 AND
  action_reversibility == "high" AND
  action_importance == "low"

ELSE → Human review required
```

**Exact thresholds are UNDECIDED** — will be set collaboratively during implementation.

---

## Pattern Versioning

- Patterns are **versioned, not overwritten**
- When evidence changes: create new version, preserve old with `superseded_by` link
- Contradictions create new hypothesis, don't silently resolve
- History enables: audit, rollback, understanding evolution

---

## Data Collection Method Impact on Evidence Quality

| Method | Evidence Quality Notes |
|--------|------------------------|
| YouTube Data API (official) | High reliability for metadata; no transcript access; quota-limited |
| yt-dlp | Transcripts, metadata, thumbnails; unofficial; may break |
| Whisper (cloud) | High accuracy transcription; cost per minute; requires approval |
| Whisper (local) | Free but hardware-dependent; quality varies by model size |
| Vision API (cloud) | High quality visual analysis; expensive; requires approval |
| Local vision models | Free but lower quality; hardware-limited |

**Evidence quality assessment must consider collection method.** A pattern based on auto-generated captions (Level B, low quality) carries less weight than one based on verified manual transcripts.

---

## Related Documents

- `docs/project/requirements-summary.md` — Authoritative requirements (Sections 4, 6)
- `docs/research/research-methodology.md` — Research levels, selection, caching
- `docs/operations/security-and-approvals.md` — Human gates, audit, rollback