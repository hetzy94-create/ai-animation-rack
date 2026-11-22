# Technical Decision Log

*All technical decisions for AI YouTube Rack are recorded here. No decision is final until explicitly approved.*

---

## Decision Template

When making a decision, add an entry using this format:

```markdown
## YYYY-MM-DD — Decision Title

**Status:** APPROVED | DEFERRED | REJECTED

**Context:** What problem does this solve? What triggered the decision?

**Options Considered:**
1. Option A — Pros / Cons
2. Option B — Pros / Cons
3. Option C — Pros / Cons

**Decision:** What was chosen

**Rationale:** Why this option over others

**Impact:** Affected components, migration needed, risks

**Approval:** Who approved / date
```

---

## Decisions Made

*None yet — all technical decisions remain UNDECIDED per requirements document.*

---

## Decisions Required (From Requirements Section 11)

### Must Answer Before Architecture Design

| # | Decision | Status | Notes |
|---|----------|--------|-------|
| 1 | Storage backend (files/JSON/MD vs SQLite vs vector DB) | UNDECIDED | Blocks data model, versioning, backup |
| 2 | Primary interface (CLI vs web UI vs VS Code extension vs Markdown) | UNDECIDED | Shapes human gates, UX |
| 3 | Scheduling mechanism (cron vs push notifications vs manual vs event-driven) | UNDECIDED | Orchestration backbone |
| 4 | YouTube data access method (official API vs yt-dlp vs hybrid) | UNDECIDED | Cost, reliability, ToS |
| 5 | Transcript source (YouTube captions vs Whisper API vs local Whisper) | UNDECIDED | Cost/quality tradeoff |
| 6 | Creative identity document format (schema, enforcement) | UNDECIDED | Production Engine input |
| 7 | Secrets management (OS keyring vs encrypted config vs secret manager) | UNDECIDED | Security baseline |

### Can Decide During Implementation

| # | Decision | Status | Notes |
|---|----------|--------|-------|
| 8 | Specific AI models per task | UNDECIDED | Can swap later |
| 9 | Caching TTLs, file naming conventions | UNDECIDED | Iterate |
| 10 | Notification channels | UNDECIDED | Start simple |
| 11 | Batch vs streaming for new uploads | UNDECIDED | Profile first |
| 12 | Visual analysis sampling strategy | UNDECIDED | Experiment |

### Optional Future Improvements

| # | Decision | Status | Notes |
|---|----------|--------|-------|
| 13 | Cross-channel comparison (add 2–3 channels) | DEFERRED | Post-MVP |
| 14 | Comment mining | DEFERRED | Post-MVP |
| 15 | Thumbnail A/B tracking | DEFERRED | Post-MVP |
| 16 | Series/playlist analysis | DEFERRED | Post-MVP |
| 17 | Local model experimentation | DEFERRED | Hardware-dependent |
| 18 | Collaborative features | DEFERRED | If team grows |

---

## Decision History

*Empty — no decisions made yet.*

---

## How to Use This Log

1. **Before implementing any technical choice:** Check if it's listed above
2. **If UNDECIDED:** Collaborate to evaluate options, then record decision here
3. **If APPROVED:** Implement with confidence
4. **If requirements change:** Add new entry superseding old (don't delete history)

---

## Related Documents

- `docs/project/requirements-summary.md` — Sections 11, 13 (undecided items, premature tech avoidance)
- `docs/guides/START-HERE.md` — Next steps for collaborative evaluation