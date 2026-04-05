# Architecture Overview

> Update this document as the system evolves. Keep it high-level —
> detailed design belongs in module-local CLAUDE.md files and ADRs.

## System Boundaries

```
                    ┌─────────────────────┐
                    │      Pipeline       │
                    │  (orchestration)    │
                    └──────┬──────────────┘
                           │
              ┌────────────┼────────────────┐
              ▼            ▼                ▼
        ┌──────────┐ ┌──────────┐    ┌───────────┐
        │ Sources  │ │    IR    │    │  Output   │
        │          │ │          │    │           │
        │ NIFC     │ │ Pydantic │    │ Markdown  │
        │ NWS      │ │ models   │    │ PDF       │
        │ USGS     │ │ (data)   │    │ (future)  │
        │ Census   │ │          │    │           │
        └──────────┘ └──────────┘    └───────────┘
```

## Package Layout

```
src/ridgeline/
├── pipeline/     # Orchestration — fetch, enrich, compile, render
├── sources/      # Data fetchers — one module per federal API
├── ir/           # RidgelineIR — Pydantic dataclasses for structured briefing data
└── output/       # Renderers — Markdown, PDF, future formats
```

## Dependency Rules

- `pipeline` depends on `sources`, `ir`, and `output`.
- `sources` depends on `ir` only (returns typed IR objects).
- `output` depends on `ir` only (consumes typed IR objects).
- `ir` has no internal dependencies — pure data definitions.
- No reverse dependencies. No layer skipping.

## Key Design Principles

1. **AI curates real content, not generates synthetic content** — every data point traces to a federal source.
2. **Provenance is non-negotiable** — source citations on all outputs, "AI-Synthesized" header treatment.
3. **Temporal honesty** — version tiers (v1: public data, v2: field-enriched, v3: reviewed) with explicit confidence.
4. **Cache everything** — all raw API responses cached locally for reproducibility and offline dev.
5. **IR is the contract** — RidgelineIR is the stable interface between sources, pipeline, and output.

## Infrastructure

- **Language**: Python 3.11+
- **Package manager**: uv
- **Compute**: Local CLI first; GCP Cloud Run later
- **CI/CD**: GitHub Actions
- **LLM**: Gemini (via Vertex AI) for enrichment and synthesis
