# MCP Server Registry

*For: AI YouTube Rack project*

---

## What This Is

This document tracks MCP (Model Context Protocol) servers that **Claude Code** (the development assistant) can use during development of AI YouTube Rack.

**Important distinction:**

| Category | Purpose | Examples |
|----------|---------|----------|
| **MCP servers for Claude Code** | Tools *you* (the developer) use while coding, researching, debugging | File system, Git, GitHub, browser, database clients, API testers |
| **External API integrations for the application** | Services the *AI YouTube Rack application* will call at runtime | YouTube Data API, Anthropic API, Whisper API, vision APIs, cloud storage |

This registry covers the **first category only** — MCP servers that enhance your development workflow.

The second category is documented in:
- `docs/integrations/youtube/README.md`
- `docs/integrations/ai-providers/README.md`

---

## Current MCP Servers: NONE CONNECTED

`.mcp.json` exists with empty `mcpServers` object. No servers are configured yet.

---

## Candidate MCP Servers for Evaluation

| Server | Purpose | Status | Notes |
|--------|---------|--------|-------|
| `filesystem` | Read/write local files | Candidate | Built into Claude Code |
| `git` | Git operations | Candidate | Built into Claude Code |
| `github` | GitHub API (PRs, issues, repo mgmt) | Candidate | Requires PAT |
| `browser` / `playwright` | Web browsing, scraping | Candidate | For YouTube research, docs |
| `sqlite` / `postgres` | Database queries | Candidate | If using SQLite/Postgres |
| `aws` / `gcp` / `azure` | Cloud resource mgmt | Candidate | If using cloud backup |
| `youtube-transcript` | Fetch YouTube transcripts | Candidate | Community server |
| `web-search` | Search the web | Candidate | For research |

---

## Evaluation Criteria

Before adding any MCP server:

1. **Necessity** — Does it solve a real workflow pain point?
2. **Security** — What permissions does it need? Least privilege?
3. **Reliability** — Maintained? Actively updated?
4. **Cost** — Free? Paid? Usage limits?
5. **Complexity** — Setup effort vs. benefit?

---

## Adding an MCP Server

1. Document it here with purpose and configuration
2. Add to `.mcp.json` (valid JSON)
3. Test in Claude Code
4. Update this registry

**Example `.mcp.json` entry:**
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

---

## Security Notes

- **Never commit secrets** to `.mcp.json` — use `${ENV_VAR}` substitution
- Store actual tokens in `.env` (gitignored) or OS keyring
- Review permissions each server requests
- Prefer read-only servers where possible
- Audit periodically: "Do we still need this?"

---

## Related Documents

- `.mcp.json` — Actual configuration (empty initially)
- `docs/integrations/youtube/README.md` — YouTube API integration (application runtime)
- `docs/integrations/ai-providers/README.md` — AI provider APIs (application runtime)
- `docs/guides/SKILLS-AND-MCP.md` — Skills vs. MCP explanation