# Ridgeline

AI-powered intelligence briefing engine for wildfire — generates Post-Fire Intelligence Summaries from public federal data.

Built by **TechTrend Federal Services** using the [Claude Code Project Template](https://github.techtrend.us/USDA-AI-Innovation-Hub/claude-code-template) and the **WAT framework** (Workflows, Agents, Tools).

## What It Does

Ridgeline fetches data from federal wildfire sources (NIFC, NWS, USGS, Census/WUI), structures it through a typed intermediate representation (RidgelineIR), and compiles it into analyst-ready intelligence briefings.

**Target output:** Post-Fire Intelligence Summary — a document that pre-assembles what a BAER team lead, Incident Commander, or congressional staffer would otherwise spend hours gathering manually.

**Design principles:**
- AI curates real content, not generates synthetic content
- Provenance is non-negotiable — source citations on all outputs
- Temporal honesty — explicit confidence tiers (public data / field-enriched / reviewed)
- "Seen, not impressed" — optimize for practitioner recognition over visual wow

## Directory Structure

```
ridgeline/
├── CLAUDE.md                        # Project context and AI agent instructions
├── pyproject.toml                   # Python project config (uv)
│
├── src/ridgeline/                   # Application source
│   ├── pipeline/                    # Orchestration — fetch, enrich, compile, render
│   ├── sources/                     # Data fetchers — one module per federal API
│   ├── ir/                          # RidgelineIR — Pydantic models for briefing data
│   └── output/                      # Renderers — Markdown, PDF, future formats
│
├── tests/                           # pytest test suite
│
├── docs/
│   ├── architecture.md              # System architecture and module boundaries
│   ├── product/                     # Product docs, CEO constraints, strategy
│   ├── research/                    # Design research, competitive analysis
│   ├── design/                      # Visual design, wireframes
│   └── decisions/                   # Architecture Decision Records (ADRs)
│
├── .claude/                         # Claude Code configuration
│   ├── settings.json                # Claude Code settings
│   ├── commands/                    # Custom slash commands
│   ├── hooks/                       # Guardrail checks
│   └── skills/                      # Reusable AI workflow definitions
│
├── .agent/memory/                   # Agent session memory
└── tools/                           # Automation scripts and prompts
```

## Quick Start

```bash
# Install dependencies
uv sync --all-extras

# Run tests
uv run pytest

# Type check
uv run mypy src/

# Lint
uv run ruff check src/ tests/
```

## Heritage

Ridgeline was spun off from [Vista](https://github.com/jvalenzano/vista), a 4D operations theater for national forests. Vista's 3D globe will become Ridgeline's optional Map tab in future milestones. Ridgeline inherits domain knowledge and strategic decisions — not code.

Key founding documents in `docs/`:
- `docs/product/ridgeline-ceo-strategic-constraints.md` — 5 CEO constraints (north star)
- `docs/decisions/ADR-001-founding-decisions.md` — architecture decisions from expert panel

## License

Internal use — TechTrend Federal Services.
