# Condensed Memory — Ridgeline

> Last updated: 2026-04-05 (Session 0 — repo standup from Vista heritage bridge)

## Project Identity
- **Ridgeline:** AI-powered intelligence briefing engine for wildfire
- Produces Post-Fire Intelligence Summaries from public federal data
- Human orchestrator: Jason Valenzano (senior AI technologist at TechTrend)
- GitHub: `jvalenzano/ridgeline` (personal GitHub)
- Heritage: spun off from Vista. Inherits domain knowledge and architectural patterns, not code.

## Current State
- **Branch:** `main` @ initial commit
- **Status:** success | **Quality:** prototype (scaffold complete, no application logic yet)
- Repo scaffolded from GHE `claude-code-template`, adapted for Python-first
- uv sync works, 1 smoke test passing
- CEO constraints document bridged from Vista
- Founding decisions ADR written (condenses 113K of panel deliberation)
- Design research bridged from Vista

## Key Files
- `CLAUDE.md` — project context, WAT framework, coding guidelines
- `docs/product/ridgeline-ceo-strategic-constraints.md` — 5 CEO constraints (north star)
- `docs/product/ridgeline-ceo-system-prompt.md` — CEO agent prompt
- `docs/decisions/ADR-001-founding-decisions.md` — founding architecture decisions
- `docs/architecture.md` — system boundaries (pipeline → sources/ir/output)
- `docs/research/2026-04-05-perp-ridgeline-ui-design-research.md` — UI design research
- `src/ridgeline/` — Python package (pipeline, sources, ir, output)
- `pyproject.toml` — project config (uv, pytest, ruff, mypy)

## Key Conventions
- `uv` for Python tooling. `pyproject.toml` for deps.
- Conventional Commits: `feat:`, `fix:`, `docs:`, `test:`
- Design-first: user journeys → wireframes → specs → code
- NO DEADLINE CONSTRAINTS — think pure Fingersnap
- Provenance non-negotiable. "AI-Synthesized" header on all outputs.
- RidgelineIR is the stable contract — Pydantic dataclasses

## Next Steps
1. **Design research sprint** — mood board from UI research + reference products. Define Ridgeline's visual language.
2. **Download + analyze Bridge Fire BAER report** — section-by-section: "what could Ridgeline pre-assemble?" → v1 document template.
3. **Design user journeys for 5 personas** — BAER team lead, congressional staffer, IC, Forest Supervisor, GIS specialist.
4. **Design the Post-Fire Intelligence Summary** — document layout, typography, inline provenance, temporal tiers.
5. **Write `springs_fire.py`** — v0 pipeline script: NIFC + NWS + USGS + Census/WUI → Markdown document.
6. **Hand-curate 3-5 historical fire analogs** — SoCal chaparral fires for the differentiator section.
