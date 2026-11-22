# Skills vs. MCP — Explanation

*For: AI YouTube Rack project*

---

## Two Different Concepts

| Concept | What It Is | Who Uses It | Example |
|---------|------------|-------------|---------|
| **Skills** | Reusable instruction sets for AI to perform tasks consistently | The **AI YouTube Rack application** at runtime | "Storytelling analysis skill" — tells AI how to analyze a script's hook, structure, pacing |
| **MCP (Model Context Protocol)** | Standardized interface for **Claude Code** (the developer) to access external tools during development | **You (the developer)** while coding | "GitHub MCP" — lets Claude Code read PRs, create issues, push branches |

---

## Skills (Application Runtime)

### What They Are
- Plain-text instruction files (Markdown) that define *how* to do a task
- Used by the AI YouTube Rack application when it needs to perform analysis, generation, evaluation
- **Not code** — they're prompts/instructions for LLMs
- Stored in `.claude/skills/` (or `config/skills/` for the application)

### Example Skill Structure
```markdown
---
name: storytelling-analysis
description: Analyze a video transcript for hook, structure, pacing, narration quality
inputs:
  - transcript: string
  - video_id: string
output:
  schema: storytelling-analysis.json
---

# Storytelling Analysis Skill

You are analyzing a YouTube video transcript for storytelling quality.

## Steps
1. Identify the hook (first 30 seconds / first ~100 words)
2. Classify hook type: question / statement / visual / teaser / other
3. Identify structure: three_act / listicle / story / tutorial / other
4. Analyze pacing: hook → build → climax → resolution
5. Evaluate narration clarity and engagement
6. Output structured JSON per schema
```

### Skills We'll Need (From Requirements)

| Skill | Engine | Purpose |
|-------|--------|---------|
| `competitor-video-analysis` | Intelligence | Analyze transcript for hooks, structure, patterns |
| `storytelling-analysis` | Intelligence | Deep storytelling evaluation |
| `scriptwriting` | Production | Draft scripts from research briefs |
| `topic-evaluation` | Production | Score topics on interest, originality, difficulty |
| `thumbnail-evaluation` | Production | Analyze thumbnail effectiveness |
| `visual-planning` | Production | Break script into scenes with visual specs |
| `quality-control` | Production | Check video against script, style constitution |

**Status:** `.claude/skills/` directory created. **No skills written yet** — separate task.

---

## MCP Servers (Development Time)

### What They Are
- Servers that expose tools (file system, Git, GitHub, browser, database, etc.) to **Claude Code**
- You interact with them through Claude Code while developing
- Defined in `.mcp.json` at project root
- **Not used by the application at runtime**

### Example: GitHub MCP
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${GITHUB_PAT}"
      }
    }
  }
}
```

With this, you can ask Claude Code: "Create a PR for my changes" and it uses the GitHub MCP.

### MCP Servers We Might Use (Development)

| Server | Purpose | When Useful |
|--------|---------|-------------|
| `filesystem` | Read/write project files | Always (built-in) |
| `git` | Commit, branch, diff | Version control |
| `github` | PRs, issues, repo mgmt | Collaboration |
| `browser` / `playwright` | Web research, YouTube inspection | Competitor research |
| `sqlite` | Query local research database | Debugging data |
| `web-search` | Search documentation | Learning |

**Status:** `.mcp.json` exists with empty `mcpServers`. **None connected yet.**

---

## Key Distinction Summary

```
┌─────────────────────────────────────────────────────────────────┐
│                    DEVELOPMENT (You + Claude Code)              │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │  MCP Servers → Tools for Claude Code                    │   │
│  │  • GitHub, Browser, Database, File System               │   │
│  │  • Defined in .mcp.json                                 │   │
│  │  • Help YOU code, research, debug                       │   │
│  └─────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                    APPLICATION RUNTIME (AI YouTube Rack)        │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │  Skills → Instructions for AI workers                   │   │
│  │  • Storytelling analysis, Scriptwriting, QC             │   │
│  │  • Stored in .claude/skills/ or config/skills/          │   │
│  │  • Used by YOUR application when it runs                │   │
│  └─────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
```

---

## Why This Matters

1. **Don't confuse them** — Skills are for your app; MCP is for your development workflow
2. **Different lifecycles** — Skills evolve with your app; MCP servers change with your dev needs
3. **Different security** — MCP servers may need tokens (GitHub PAT); Skills are plain text
4. **Different documentation** — MCP registry in `docs/integrations/mcp/`; Skills in `.claude/skills/`

---

## Current State

| Item | Status |
|------|--------|
| `.mcp.json` | Created, empty |
| `.claude/skills/` | Directory created, empty |
| MCP servers connected | **None** |
| Skills written | **None** |

---

## Next Steps (When Ready)

1. **MCP:** Evaluate which servers help your development workflow → add to `.mcp.json` → document in `docs/integrations/mcp/MCP-REGISTRY.md`
2. **Skills:** Write skill files for each needed capability → store in `.claude/skills/` → test with sample inputs

**Both are preparation only — no implementation yet.**