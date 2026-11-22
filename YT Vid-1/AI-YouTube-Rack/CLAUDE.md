# AI YouTube Rack — Project Instructions

## Project Overview

AI YouTube Rack is a system for building and managing a faceless YouTube channel. It contains two cooperating engines:

1. **Competitor Intelligence Engine** — Studies reference channels (starting with Bernard Animation) to discover evidence-based content patterns.
2. **Video Production Engine** — Uses those patterns as guidance (not rules) to create original content, then learns from the channel's own performance.

## Authoritative Requirements

See: `docs/project/requirements-summary.md`

All work must align with that document. It contains 13 sections covering engines, research methodology, production pipeline, automation levels, constraints, risks, and undecided technical questions.

## Human Approval & Safety Rules

- **Never auto-publish** to YouTube — explicit human approval required
- **Never auto-change** validated research conclusions — human validation required
- **Never spend money** (paid APIs, services) without explicit approval
- **Never delete/overwrite** creative assets without confirmation
- **Never commit to a video topic** for production without human decision
- All important conclusions require human approval — AI confidence scores alone are insufficient

## Technical Decisions: UNDECIDED

The following have NOT been approved and must be evaluated collaboratively before any decision:
- Storage backend (files, SQLite, vector DB, etc.)
- Interface (CLI, web UI, VS Code extension, etc.)
- Scheduling mechanism (cron, push notifications, manual, etc.)
- YouTube data access method (official API, yt-dlp, hybrid)
- Transcript source (YouTube captions, Whisper API, local Whisper)
- Creative identity document format
- Secrets management
- Any framework, database, or external service

## Work Principles

- Prefer ordinary code for deterministic tasks; AI only for judgment/creativity
- Avoid unnecessary complexity — start minimal, add only when justified
- Maintain provider independence — open formats, abstracted interfaces
- Cache aggressively; verify pricing before any paid usage
- Traceability > convenience — every finding must carry its source

## Directory Structure

See `README.md` for folder structure explanation.