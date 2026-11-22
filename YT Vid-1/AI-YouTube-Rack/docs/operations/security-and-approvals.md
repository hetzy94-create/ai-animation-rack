# Security, Approvals & Audit

*Derived from: `docs/project/requirements-summary.md` (Sections 7, 8, 9)*

---

## Human Approval Boundaries (Hard Rules)

**These actions NEVER happen without explicit human permission:**

| Action | Why |
|--------|-----|
| Publishing/uploading to YouTube | Public commitment; irreversible |
| Changing validated research conclusions | Affects all future decisions |
| Modifying creative identity / style constitution | Defines your channel's identity |
| Spending money (paid APIs, services) | Budget control; requires verification |
| Deleting/overwriting creative assets | Your work; irrecoverable |
| Committing to a video topic for production | Resource allocation; creative direction |

---

## Automation Levels (Per-Task, Not Global)

| Level | Name | Behavior | Example Tasks |
|-------|------|----------|---------------|
| 1 | Manual assistance | System informs → you decide everything | Publishing, changing validated conclusions |
| 2 | Assisted execution | System prepares + recommends → you approve important actions | Idea generation, script drafting, QC |
| 3 | Conditional automation | Auto-executes low-risk → asks on important/uncertain | Metadata collection, cache updates, asset organization |
| 4 | High autonomy | Plans + executes multi-step → reports results/exceptions | **Not used initially** |

**Level 3 Guard:** When uncertain or important, system **escalates to Level 2** (asks you).

---

## Suggested Task → Level Mapping

### Intelligence Engine

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

### Production Engine

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

---

## Audit Logging

**Every automated action logs:**

| Field | Description |
|-------|-------------|
| `timestamp` | ISO 8601 UTC |
| `trigger` | What started it (schedule, event, manual) |
| `task_id` | Unique task identifier |
| `automation_level` | 1–4 |
| `inputs` | Parameters, data sources used |
| `decision_logic` | Why this action (rule, threshold, AI reasoning) |
| `output` | Result, files created, API calls made |
| `cost_estimate` | API tokens, dollars (if applicable) |
| `human_reviewed` | Boolean — was human in loop? |

**Storage:** `data/shared/logs/audit.log` (append-only, rotated)

---

## Confidence Thresholds for Level 3 Automation

**Template (UNDECIDED — to be set collaboratively):**

```
Auto-action allowed IF ALL:
  - confidence_score >= THRESHOLD (e.g., 0.85)
  - confirmation_count >= MIN_CONFIRMATIONS (e.g., 5)
  - sample_size >= MIN_SAMPLE (e.g., 10)
  - source_diversity >= MIN_SOURCES (e.g., 3)
  - contradictory_findings == 0
  - action_reversibility == "high" (e.g., cache update, not conclusion change)
  - action_importance == "low" (e.g., not creative, not public-facing)

ELSE → Escalate to Level 2 (human approval)
```

**Key principle from requirements:** Numerical confidence score alone is **not sufficient**. Must also weigh evidence quality, sample size, source diversity, contradictory findings, importance, reversibility.

---

## Rollback Capability

| Action Type | Rollback Method |
|-------------|-----------------|
| Cache update | Delete cache entry; re-fetch on next run |
| Pattern update | Restore previous pattern version (versioned storage) |
| Asset organization | Move files back; manifest tracks original locations |
| Report generation | Delete report; regenerate |
| **Creative assets** | **Never auto-deleted** — soft-delete with 30-day retention |
| **Validated conclusions** | **Never auto-changed** — human-only |

**Command:** `revert-last-auto-action` (to be implemented)

---

## Notification Design

| Priority | Delivery | Examples |
|----------|----------|----------|
| **Routine** | Batched daily digest | "Processed 3 new videos, updated 2 patterns, 0 surprises" |
| **Decision needed** | Immediate (console, file, webhook) | "New video format detected — select for deep analysis?" |
| **Exception/Surprise** | Immediate + flagged in audit | "Contradiction found: Pattern X violated by new video" |
| **Cost alert** | Immediate | "Projected monthly API cost exceeds $X — approve?" |

**Goal:** Avoid notification fatigue. Batch routine; surface only decisions, exceptions, cost alerts.

---

## Learning from Corrections

When you override a Level 2/3 recommendation:

1. System records: `task_id`, `system_recommendation`, `your_choice`, `context`
2. Preference stored in `config/automation-preferences.json`
3. Future similar tasks: system adjusts recommendation or escalates sooner
4. Quarterly review: you audit learned preferences

---

## Secrets Management (UNDECIDED)

**Requirements from authoritative doc:**
- No hardcoded credentials
- `.env` file (gitignored) for local development
- Production: OS keyring, encrypted config, or secret manager
- Rotation policy for long-lived tokens

**Options to evaluate:**
- Windows Credential Manager
- `keyring` Python library (cross-platform)
- Encrypted `.env.enc` with age/sops
- HashiCorp Vault (overkill?)
- GitHub Codespaces secrets (if cloud)

**Decision deferred to implementation.**

---

## Provider Independence (Security Aspect)

- No API keys in code — always via environment/config
- Provider abstraction layer: swap Anthropic ↔ OpenAI ↔ Google without code changes
- Skills/prompts stored as plain text — not tied to Claude Code
- If provider changes, only integration adapter changes

---

## Related Documents

- `docs/project/requirements-summary.md` — Authoritative requirements (Sections 7, 8, 9)
- `docs/operations/backup-and-recovery.md` — Backup strategy
- `docs/production/production-pipeline.md` — Automation levels per stage
- `config/` — Configuration files (to be created)