# AI YouTube Rack

A system for building and managing a faceless YouTube channel through two cooperating engines:

1. **Competitor Intelligence Engine** — Studies reference channels to discover evidence-based content patterns
2. **Video Production Engine** — Uses those patterns as guidance to create original content, then learns from your own channel's performance

---

## Folder Structure

```
AI-YouTube-Rack/
│
├── CLAUDE.md                 # Project instructions for Claude Code
├── README.md                 # This file
├── .gitignore                # Git ignore rules
├── .mcp.json                 # MCP server configuration (empty, for preparation)
├── .env.example              # Environment variable template (comments only)
│
├── .claude/
│   └── skills/               # Skill definitions (empty, for preparation)
│
├── docs/
│   ├── project/
│   │   ├── requirements-summary.md   # Authoritative requirements (READ THIS FIRST)
│   │   └── decisions/
│   │       └── decision-log.md       # Technical decision log
│   ├── research/
│   │   ├── research-methodology.md   # Research levels, selection, caching
│   │   └── evidence-and-confidence.md # Evidence standards, confidence, causation
│   ├── production/
│   │   └── production-pipeline.md    # 10-stage pipeline (A–J)
│   ├── operations/
│   │   ├── security-and-approvals.md # Human gates, audit, rollback
│   │   └── backup-and-recovery.md    # Backup strategy, recovery procedures
│   ├── integrations/
│   │   ├── mcp/
│   │   │   └── MCP-REGISTRY.md       # MCP server registry documentation
│   │   ├── youtube/
│   │   │   └── README.md             # YouTube integration notes
│   │   └── ai-providers/
│   │       └── README.md             # AI provider integration notes
│   └── guides/
│       ├── START-HERE.md             # Beginner getting-started guide
│       └── SKILLS-AND-MCP.md         # Skills and MCP explanation
│
├── data/
│   ├── intelligence/         # Competitor Intelligence Engine data
│   │   ├── raw/              # Raw fetched data (metadata, transcripts, frames)
│   │   ├── processed/        # Processed/normalized data
│   │   ├── observations/     # Individual video observations
│   │   ├── patterns/         # Discovered patterns with evidence
│   │   ├── reports/          # Analysis reports
│   │   └── cache/            # Cached API responses, model outputs
│   ├── production/           # Video Production Engine data
│   │   ├── ideas/            # Video topic ideas
│   │   ├── research/         # Topic research briefs
│   │   ├── scripts/          # Script drafts and versions
│   │   ├── storyboards/      # Scene breakdowns and visual plans
│   │   ├── assets/           # Visual/audio assets
│   │   ├── exports/          # Rendered video exports
│   │   ├── publishing/       # Titles, thumbnails, descriptions
│   │   └── analytics/        # Your channel's performance data
│   └── shared/
│       ├── logs/             # Audit logs, automation logs
│       └── backups/          # Automated backups
│
├── templates/
│   ├── research/             # Research templates
│   ├── production/           # Production templates (scripts, storyboards, etc.)
│   └── operations/           # Operational templates
│
├── src/
│   ├── common/               # Shared utilities
│   ├── intelligence/         # Intelligence Engine code
│   ├── production/           # Production Engine code
│   └── integrations/         # External API integrations
│
├── scripts/                  # Utility scripts
├── tests/                    # Test files
└── config/                   # Configuration files
```

---

## Getting Started

### Prerequisites
- Windows (primary development environment)
- VS Code with Claude Code extension
- Python 3.10+ (recommended for scripts)
- Git (for version control)

### First Steps
1. **Read the requirements** — Start with `docs/project/requirements-summary.md`
2. **Review CLAUDE.md** — Understand project instructions and safety rules
3. **Check undecided items** — See "Missing Decisions" section in requirements
4. **No installation needed yet** — This workspace is preparation only

### Current Project Status
**Phase:** Requirements finalized, workspace prepared
**Next:** Collaborative evaluation of technical decisions (storage, interface, scheduling, data access, etc.)
**No code implemented yet** — All technical choices remain UNDECIDED

---

## Key Principles

- **Research fuels creation** — Intelligence Engine insights guide Production Engine
- **Human in the loop** — Creative decisions require your approval
- **Traceability** — Every finding carries its source and confidence level
- **Cost awareness** — Free/cheap first; paid usage requires explicit approval
- **Provider independence** — Open formats, swappable integrations
- **Simplicity first** — No frameworks/agents/services until justified

---

## Important Notes

- `.mcp.json` is empty — MCP servers will be evaluated later
- `.claude/skills/` is empty — Skills will be created in a separate task
- No packages installed, no APIs connected, no credentials configured
- All technical decisions documented in `docs/project/decisions/decision-log.md` (when made)