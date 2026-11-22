# Getting Started with AI YouTube Rack

Welcome! This guide walks you through the project workspace and how to begin contributing.

---

## 1. Read the Requirements First

**Start here:** `docs/project/requirements-summary.md`

This is the **authoritative specification**. Everything in this project must align with it. Key sections to understand:

- **Sections 1–3:** Project overview, two engines, shared vs. separate data
- **Sections 4–5:** Research methodology (5 levels, selective deep analysis, caching)
- **Section 6:** Production pipeline (10 stages A–J)
- **Sections 7–8:** Automation levels, human approval boundaries
- **Section 11:** Undecided technical decisions (storage, interface, scheduling, etc.)
- **Section 13:** Premature technology avoidance — what NOT to assume

---

## 2. Understand the Two Engines

```
┌─────────────────────────────────────────────────────────────┐
│                    AI YOUTUBE RACK                          │
│  ┌─────────────────────────┐   ┌─────────────────────────┐  │
│  │ Competitor Intelligence │   │   Video Production      │  │
│  │ Engine                  │   │   Engine                │  │
│  │                         │   │                         │  │
│  │ • Studies reference     │   │ • Creates YOUR videos   │  │
│  │   channels              │   │ • Uses research as      │  │
│  │ • Discovers patterns    │   │   guidance              │  │
│  │ • Continuous learning   │   │ • Learns from YOUR      │  │
│  │                         │   │   analytics             │  │
│  └───────────┬─────────────┘   └───────────┬─────────────┘  │
│              │                             │                 │
│              └──────────────┬──────────────┘                 │
│                             ▼                                │
│                    ┌─────────────────────┐                   │
│                    │   Shared Knowledge  │                   │
│                    │   (traceable lessons)                   │
│                    └─────────────────────┘                   │
└─────────────────────────────────────────────────────────────┘
```

---

## 3. Key Principles (Non-Negotiable)

| Principle | What It Means |
|-----------|---------------|
| **Human in the loop** | You approve: publishing, topic commitment, spending money, changing validated conclusions |
| **Traceability** | Every finding carries: source video, research level, confidence tier, limitations |
| **Correlation ≠ Causation** | Competitor research only shows correlation. Never claim "X causes retention." |
| **Cost awareness** | Free/cheap first. Paid APIs require explicit approval. Verify pricing. |
| **Provider independence** | Open formats (JSON, MD), abstracted interfaces. No lock-in. |
| **Simplicity first** | No frameworks/agents/databases until justified by real need. |

---

## 4. Workspace Tour

```
AI-YouTube-Rack/
├── CLAUDE.md                 # Instructions for Claude Code (READ THIS)
├── README.md                 # Project overview + folder structure
├── docs/project/requirements-summary.md  # THE SPEC
├── docs/guides/START-HERE.md # You are here
├── data/                     # All project data (versioned)
│   ├── intelligence/         # Competitor research
│   ├── production/           # Your video production
│   └── shared/               # Logs, backups
├── src/                      # Application code (empty)
├── config/                   # Configuration (empty)
├── templates/                # Templates (empty)
└── .claude/skills/           # Skills (empty - for future)
```

---

## 5. Current Status

**Phase:** Requirements finalized, workspace prepared
**No code implemented yet**
**All technical decisions UNDECIDED** — see `docs/project/requirements-summary.md` Section 11

---

## 6. How to Contribute

### For Research/Analysis Tasks
1. Check `docs/research/research-methodology.md` for levels and selection criteria
2. Check `docs/research/evidence-and-confidence.md` for evidence standards
3. Data goes in `data/intelligence/` (raw → processed → observations → patterns)

### For Production Tasks
1. Check `docs/production/production-pipeline.md` for 10-stage pipeline
2. Data goes in `data/production/` (ideas → research → scripts → storyboards → assets → exports)

### For Technical Decisions
1. Check `docs/project/decisions/decision-log.md` for past decisions
2. Any new decision: document rationale, alternatives considered, approval
3. **No decision is final until you approve it**

---

## 7. Safety Checklist (Before Any Action)

- [ ] Does this require human approval? (Publishing, spending, changing conclusions, deleting assets)
- [ ] Is this using a paid API/service? → Verify pricing, get approval
- [ ] Am I mixing Intelligence and Production data? → Keep separate
- [ ] Does this create vendor lock-in? → Use open formats, abstracted interfaces
- [ ] Is this adding complexity without proven need? → Start simpler

---

## 8. Next Steps (Collaborative)

The following need your evaluation before implementation:

1. **Storage backend** — Files (JSON/MD) vs SQLite vs vector DB?
2. **Interface** — CLI, local web UI, VS Code extension, Markdown files?
3. **Scheduling** — Cron, YouTube push notifications, manual, event-driven?
4. **YouTube data access** — Official API, yt-dlp, hybrid?
5. **Transcript source** — YouTube captions, Whisper API, local Whisper?
6. **Creative identity format** — What fields? How enforced?
7. **Secrets management** — OS keyring, encrypted config, secret manager?

---

## 9. Useful Commands (When Code Exists)

```bash
# Placeholder - no code yet
# python -m intelligence.scan_channel --channel @bernardanimationofficial
# python -m production.pipeline --stage A --topic "My Idea"
# python -m intelligence.analyze --video-id X --level B
```

---

## 10. Questions?

- Re-read `docs/project/requirements-summary.md`
- Check `docs/guides/SKILLS-AND-MCP.md` for skills vs. MCP explanation
- All undecided items are explicitly marked **UNDECIDED** in the requirements
- When in doubt: **ask before implementing**

---

**Remember:** This is a beginner-friendly project. No question is too simple. The requirements document is your north star — when something isn't clear, it's usually because the decision hasn't been made yet.