# YouTube Integration (Application Runtime)

*For: AI YouTube Rack application — NOT Claude Code MCP*

---

## Purpose

This documents how the **AI YouTube Rack application** (the software we're building) will integrate with YouTube at runtime.

**This is NOT about MCP servers for Claude Code development.** See `docs/integrations/mcp/MCP-REGISTRY.md` for that.

---

## Required Capabilities

| Capability | Use Case | Engine |
|------------|----------|--------|
| List channel videos | Build video library, detect new uploads | Intelligence |
| Get video metadata (title, description, date, duration, views, likes, thumbnail) | Level A research | Intelligence |
| Get video transcripts/captions | Level B research | Intelligence |
| Get channel analytics (own channel) | Loop 2 learning (retention, CTR, demographics) | Production |
| Upload video | Publishing (Stage I) | Production |
| Update video metadata (title, description, tags, thumbnail) | Publishing (Stage I) | Production |
| Manage playlists | Publishing organization | Production |

---

## Access Methods: UNDECIDED (Must Evaluate)

| Method | Pros | Cons | Status |
|--------|------|------|--------|
| **Official YouTube Data API v3** | Supported, reliable, quota-managed | Quota-limited (10k/day), no transcript access, no private analytics without OAuth | Candidate |
| **YouTube Analytics API** | Private analytics (retention, CTR, traffic) | Requires OAuth, channel owner only | Candidate (for Loop 2) |
| **YouTube Reporting API** | Bulk reports | Complex, channel owner only | Candidate (for Loop 2) |
| **yt-dlp** | Transcripts, metadata, thumbnails, no quota | Unofficial, may break, ToS gray area | Candidate |
| **Hybrid** | API for metadata, yt-dlp for transcripts | Two systems to maintain | Candidate |
| **InnerTube API** (private) | Full access | Unofficial, unstable, ToS violation risk | **Not recommended** |

**No method is approved.** Evaluation required considering:
- Cost (free vs. paid)
- Reliability & maintenance burden
- YouTube ToS compliance
- Data completeness (transcripts, analytics)
- Rate limits / quotas

---

## Authentication

| Scope | Method | Notes |
|-------|--------|-------|
| Public data (metadata, public videos) | API key | Simple, quota-limited |
| Private analytics (own channel) | OAuth 2.0 | Refresh token flow; store securely |
| Video upload | OAuth 2.0 | Requires `youtube.upload` scope |
| Channel management | OAuth 2.0 | Requires `youtube.force-ssl` scope |

**Secrets management UNDECIDED** — see `docs/operations/security-and-approvals.md`

---

## Quota & Rate Limit Strategy

- YouTube Data API: 10,000 units/day default
- `search.list` = 100 units, `videos.list` = 1 unit, `channels.list` = 1 unit
- **Must implement:** Quota tracking, daily budget, graceful degradation
- **Cache aggressively:** Metadata cached indefinitely; re-fetch only on explicit refresh

---

## Data Models (Conceptual)

### Video Record
```json
{
  "video_id": "dQw4w9WgXcQ",
  "channel_id": "UC...",
  "title": "...",
  "description": "...",
  "published_at": "2024-01-15T10:30:00Z",
  "duration_seconds": 642,
  "view_count": 123456,
  "like_count": 7890,
  "comment_count": 456,
  "thumbnail_url": "https://...",
  "transcript": { "source": "yt-dlp", "retrieved_at": "...", "text": "..." },
  "research_level": "B",
  "analyzed_at": "2024-11-20T14:00:00Z"
}
```

### Channel Record
```json
{
  "channel_id": "UC...",
  "handle": "@bernardanimationofficial",
  "title": "Bernard Animation",
  "video_count": 87,
  "subscriber_count": 123000,
  "last_scanned": "2024-11-20T14:00:00Z"
}
```

---

## Legal & ToS Compliance

- Review YouTube API Terms of Service and Developer Policies
- Respect `robots.txt` and rate limits
- Store only data necessary for project function
- Don't redistribute YouTube content
- User data (OAuth tokens) — minimal scope, secure storage, user consent

---

## Related Documents

- `docs/project/requirements-summary.md` — Sections 4, 7, 11 (YouTube data access method UNDECIDED)
- `docs/research/research-methodology.md` — Data collection methods evaluation
- `docs/integrations/ai-providers/README.md` — AI provider APIs
- `docs/operations/security-and-approvals.md` — Secrets management
- `.env.example` — Environment variable template