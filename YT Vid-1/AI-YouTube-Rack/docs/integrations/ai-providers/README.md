# AI Provider Integrations (Application Runtime)

*For: AI YouTube Rack application — NOT Claude Code MCP*

---

## Purpose

This documents how the **AI YouTube Rack application** will integrate with AI providers at runtime for reasoning, analysis, and generation tasks.

**This is NOT about MCP servers for Claude Code development.** See `docs/integrations/mcp/MCP-REGISTRY.md` for that.

---

## Required Capabilities

| Capability | Use Case | Engine |
|------------|----------|--------|
| Text generation (scripts, outlines, ideas) | Stages A, B, D | Production |
| Text analysis (storytelling, hooks, structure) | Levels B, E | Intelligence |
| Structured output (JSON, schema-validated) | Pattern extraction, evaluation | Both |
| Vision analysis (frames, thumbnails) | Level C research | Intelligence |
| Audio analysis (transcription, classification) | Level D research | Intelligence |
| Embeddings (semantic search, clustering) | Topic clustering, similarity | Both |

---

## Provider Options: UNDECIDED (Must Evaluate)

| Provider | Models | Strengths | Cost | Status |
|----------|--------|-----------|------|--------|
| **Anthropic** | Claude 3.5 Sonnet, Haiku, Opus | Strong reasoning, long context, structured output | Pay-per-token | Candidate |
| **OpenAI** | GPT-4o, GPT-4o-mini, o1 | Broad capability, vision, audio | Pay-per-token | Candidate |
| **Google** | Gemini 1.5 Pro, Flash | Large context, multimodal, competitive pricing | Pay-per-token | Candidate |
| **Local (Ollama, LM Studio)** | Llama 3, Qwen, etc. | Free, private, no latency | Hardware-limited | Candidate (future) |
| **OpenRouter** | Access to many via single API | Model switching, fallback | Pay-per-token | Candidate |

**No provider is approved.** Evaluation required considering:
- Capability match for each task type
- Cost per task (tiered: free → cheap → expensive)
- Reliability & uptime
- Rate limits
- Data privacy (local vs. cloud)
- Vendor lock-in avoidance

---

## Tiered Usage Strategy (From Requirements)

| Tier | Purpose | Example Models | Approval |
|------|---------|----------------|----------|
| **Free** | YouTube Data API, local code | N/A | Pre-approved |
| **Cheap** | High-volume, lower-stakes tasks | Gemini Flash, GPT-4o-mini, Haiku | Pre-approved with budget cap |
| **Expensive** | Complex reasoning, vision, high-stakes | GPT-4o, Sonnet, Opus, vision APIs | **Explicit human approval per use** |

**Rule:** Never assume free. Verify current pricing. Paid usage requires explicit approval.

---

## Abstraction Layer (Provider Independence)

```
Application Code
       │
       ▼
┌──────────────────┐
│  AI Provider     │  ← Interface: generate(prompt, schema, options)
│  Adapter         │
└────────┬─────────┘
         │
    ┌────┴────┐
    ▼         ▼
Anthropic  OpenAI  Google  Local  OpenRouter
```

**Interface methods (conceptual):**
- `generate_text(prompt, schema?, options?)`
- `analyze_image(image, prompt, schema?)`
- `transcribe_audio(audio, options?)`
- `embed(texts)`

**Configuration:** `config/ai-providers.json` (model assignments per task type)

---

## Task → Model Assignment (Template — UNDECIDED)

| Task | Tier | Candidate Models | Notes |
|------|------|------------------|-------|
| Metadata collection | Free | Local code | No AI |
| Transcript fetch | Free/Cheap | yt-dlp / Whisper API | Not LLM |
| Hook/structure analysis (Level B) | Cheap | Gemini Flash, GPT-4o-mini | Structured output |
| Pattern synthesis (Level E) | Expensive | Sonnet, GPT-4o | Complex reasoning |
| Visual analysis (Level C) | Expensive | GPT-4o vision, Gemini Pro Vision | Per-frame cost |
| Script drafting (Stage D) | Cheap → Expensive | Flash → Sonnet (iterate) | Human refines |
| Topic evaluation (Stage B) | Cheap | Flash, GPT-4o-mini | Scoring rubric |
| Thumbnail analysis | Expensive | Vision models | Per-image |

**Exact assignments UNDECIDED** — will be tested during implementation.

---

## Structured Output Requirements

All AI calls that produce structured data **must use schema validation** (JSON Schema / Pydantic / Zod).

Example pattern extraction schema:
```json
{
  "type": "object",
  "properties": {
    "hook_type": {"type": "string", "enum": ["question", "statement", "visual", "teaser", "other"]},
    "structure": {"type": "string", "enum": ["three_act", "listicle", "story", "tutorial", "other"]},
    "pacing_notes": {"type": "string"},
    "confidence": {"type": "number", "minimum": 0, "maximum": 1}
  },
  "required": ["hook_type", "structure", "confidence"]
}
```

---

## Cost Tracking

Every AI call logs:
- Provider, model, task type
- Input tokens, output tokens
- Estimated cost (USD)
- Timestamp, success/failure

**Monthly budget cap:** UNDECIDED — set during implementation with alerting.

---

## Local AI Evaluation (Future)

From requirements: Heavy local workloads may be impractical on i7-4th gen/16GB, but **smaller models and other local AI tools can be evaluated through practical testing when justified.**

**Candidates to test:**
- Ollama with quantized models (Q4_K_M: 4-8GB RAM)
- LM Studio (GUI, easy model switching)
- Whisper.cpp (local transcription — very feasible)
- Local vision models (LLaVA, Moondream — test quality vs. API)

**Decision:** Deferred to implementation phase.

---

## Related Documents

- `docs/project/requirements-summary.md` — Sections 7, 8, 11 (AI provider UNDECIDED)
- `docs/research/evidence-and-confidence.md` — How AI analysis feeds evidence
- `docs/production/production-pipeline.md` — AI usage per production stage
- `docs/operations/security-and-approvals.md` — Secrets management, cost approval
- `.env.example` — Environment variable template