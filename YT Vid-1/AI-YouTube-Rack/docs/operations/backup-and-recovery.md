# Backup & Recovery

*Derived from: `docs/project/requirements-summary.md` (Section 10)*

---

## What Gets Backed Up (All Project-Critical Data)

| Category | Path | Contents |
|----------|------|----------|
| **Research database** | `data/intelligence/` | Observations, patterns, hypotheses, conclusions, source traces, reports |
| **Production artifacts** | `data/production/` | Scripts, storyboards, asset manifests, creative identity, research briefs, publishing packages |
| **Configuration** | `config/`, `.env` (excluded from git), `.claude/skills/` | Skills, prompts, thresholds, automation policies, secrets references |
| **Audit logs** | `data/shared/logs/` | Automation logs, decision trails, cost tracking |

---

## Backup Strategy

### Frequency
- **Daily incremental** — Changes since last backup
- **Weekly full** — Complete snapshot

### Storage Locations (At Least Two Independent)

| Location | Type | Status |
|----------|------|--------|
| Local (secondary drive) | Primary | REQUIRED |
| Cloud (OneDrive, Google Drive, S3, GCS, etc.) | Secondary | REQUIRED |
| External USB/NAS | Tertiary | RECOMMENDED |

**UNDECIDED:** Specific cloud provider and configuration — evaluate during implementation.

---

## Recovery Requirements

| Scenario | Recovery Target | Method |
|----------|-----------------|--------|
| Accidental file deletion | Single file/folder | Restore from latest incremental |
| Database corruption | Research database (patterns, observations) | Point-in-time restore from weekly full + incrementals |
| Config loss | Skills, thresholds, policies | Restore config from latest backup |
| Full workspace loss | Entire project | Restore weekly full to new machine |
| Ransomware/malware | Clean state before infection | Restore from pre-infection backup (offline/immutable) |

### Point-in-Time Restore (Research Database)
- Research data is append-mostly with versioning
- Restore to any daily increment point
- Pattern versions preserved — can recover superseded conclusions

### Single-File Recovery (Production Artifacts)
- Scripts, storyboards, assets versioned individually
- Restore specific version without full database restore

### Integrity Verification
- **Checksums** (SHA-256) for all backed files
- Manifest with file list + hashes stored with each backup
- Verify on restore; alert on mismatch

---

## Backup Implementation (UNDECIDED — Options)

| Approach | Pros | Cons |
|----------|------|------|
| **Robocopy / rsync + Windows Task Scheduler** | Native, free, reliable | Manual scripting |
| **FreeFileSync / Syncthing** | GUI, real-time option | Extra dependency |
| **Python script + cloud SDK** | Programmable, testable | Code to maintain |
| **Git + Git LFS** | Versioned, familiar | Not ideal for large media; LFS costs |
| **Cloud provider native (OneDrive, GDrive sync)** | Zero config | Less control, no point-in-time easily |

**Evaluation criteria:** Reliability, restore testability, cost, Windows-native, beginner-friendly.

---

## Automated Backup Checklist (Per Run)

- [ ] Verify source paths exist
- [ ] Compute checksums for new/changed files
- [ ] Copy to local secondary location
- [ ] Copy to cloud location
- [ ] Verify cloud copy checksums match
- [ ] Update backup manifest (date, files, hashes, size)
- [ ] Log success/failure with details
- [ ] Alert on failure (immediate notification)

---

## Recovery Testing (Quarterly Drill)

| Test | Frequency | Success Criteria |
|------|-----------|------------------|
| Single file restore | Quarterly | File restored, checksum matches, < 5 min |
| Full research database restore | Quarterly | All patterns/observations intact, versions accessible, < 30 min |
| Config + skills restore | Quarterly | System runs with restored config, < 10 min |
| Full workspace restore (simulated) | Semi-annual | Clean machine → working project, < 2 hours |

**Documentation:** Record test date, duration, issues, lessons in `data/shared/backups/recovery-drill-log.md`

---

## Soft-Delete Protection

| Asset Type | Retention Before Purge |
|------------|------------------------|
| Scripts, storyboards | 30 days |
| Research patterns/observations | 30 days (versioned, so rarely deleted) |
| Assets (images, audio) | 30 days |
| Cache entries | 7 days (re-fetchable) |
| Logs | 90 days (then archive) |

**Implementation:** Move to `data/shared/backups/soft-delete/<date>/` with manifest. Purge script runs weekly.

---

## What NOT to Back Up (Regeneratable)

| Path | Reason |
|------|--------|
| `data/intelligence/cache/` | Re-fetchable from source |
| `data/intelligence/raw/frames/` | Large; re-extractable from video |
| `__pycache__/`, `node_modules/`, `.venv/` | Build artifacts |
| `*.log` (rotated) | Archived separately |

---

## Related Documents

- `docs/project/requirements-summary.md` — Authoritative requirements (Section 10)
- `docs/operations/security-and-approvals.md` — Audit logging, rollback
- `config/` — Backup configuration (to be created)