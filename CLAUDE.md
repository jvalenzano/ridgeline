# CLAUDE.md — Ridgeline

## What This Project Is

Ridgeline is an AI-powered intelligence briefing engine for wildfire. It fetches
public federal data (NIFC, NWS, USGS, Census/WUI), structures it through a typed
intermediate representation (RidgelineIR), and compiles analyst-ready Post-Fire
Intelligence Summaries.

**North star:** `docs/product/ridgeline-ceo-strategic-constraints.md` — 5 CEO
constraints that govern all design and implementation decisions. Read this first.

**Heritage:** Spun off from [Vista](https://github.com/jvalenzano/vista) (4D forest
ops theater). Ridgeline inherits domain knowledge and strategic decisions, not code.
Vista's 3D globe becomes Ridgeline's optional Map tab in Milestone 4+.

## Hard Directives

- **AI curates real content, not generates synthetic content** — every data point traces to a federal source
- **Provenance is non-negotiable** — source citations on ALL outputs, "AI-Synthesized" header treatment
- **Temporal honesty** — version tiers: v1 (public data only), v2 (field-enriched), v3 (reviewed). Never present v1 confidence as v3.
- **"Seen, not impressed"** — optimize for practitioner recognition over visual wow
- **BAER-native framing** — use BAER terminology and structure, not generic disaster reporting
- **Two-sided acceptance test** — every briefing must surface at least one new insight AND one correctable error visible to a domain expert
- **NO DEADLINE CONSTRAINTS** — think pure Fingersnap; never hedge vision with budgets or timelines
- **Design-first methodology** — user journeys and visual design before technical specs

## Operating Philosophy: WAT (Workflows, Agents, Tools)

This project follows the WAT framework — a separation of concerns
where probabilistic AI handles reasoning and orchestration while
deterministic code handles execution.

**Workflows** (instructions): Markdown SOPs in `.claude/commands/` and
`.claude/skills/` define objectives, required inputs, which tools to
use, expected outputs, and how to handle edge cases.

**You as Agent** (decision-maker): Read the relevant workflow, run tools
in the correct sequence, handle failures gracefully, and ask clarifying
questions when uncertain. Connect intent to execution without trying to
do everything inline.

**Tools** (execution): Scripts in `tools/scripts/` and application code
in `src/` do the actual work — API calls, data transformations, file
operations. These are consistent, testable, and fast.

## Architecture Overview

High-level boundaries (see `docs/architecture.md` for diagrams):

- `src/ridgeline/pipeline/` — Orchestration: fetch → enrich → compile → render.
- `src/ridgeline/sources/` — Data fetchers, one module per federal API.
- `src/ridgeline/ir/` — RidgelineIR: Pydantic dataclasses for structured briefing data.
- `src/ridgeline/output/` — Renderers: Markdown, PDF, future formats.
- `tools/scripts/` — Automation scripts for data processing and maintenance.

Dependency rules:
- `pipeline` → `sources`, `ir`, `output`. Orchestration layer.
- `sources` → `ir` only. Returns typed IR objects.
- `output` → `ir` only. Consumes typed IR objects.
- `ir` has NO internal dependencies. Pure data definitions.
- No reverse dependencies. No layer skipping.

## Standard Workflow

For any task, follow this sequence. Steps marked **[PAUSE]** require
explicit user confirmation before continuing.

1. **Look for existing tools first.** Check `tools/scripts/` and
   existing modules before building anything new.
2. **Clarify** — Summarize the request. Identify affected modules
   and constraints. If ambiguous, ask 1-2 clarifying questions.
3. **Invariants** — State what must NOT break (tests, contracts, boundaries).
4. **Plan** — List concrete steps at file level. **[PAUSE]** Show the
   plan and wait for approval before proceeding to step 5.
5. **Execute** — Make changes incrementally, one concern per commit.
6. **Validate** — Run tests with `uv run pytest`. Run `uv run ruff check`
   and `uv run mypy src/` on changes.
7. **Document** — Update comments only for non-obvious logic. For
   meaningful architectural changes, update `docs/architecture.md` or
   create an ADR under `docs/decisions/`.
8. **Summarize** — Describe what changed, what was tested, and any
   open items.

## Development Setup

- Copy `.meta.example/` to `.meta/` for process intelligence capture.
- See `docs/developer-workspace.md` for all local workspace files.

```bash
# Install all dependencies (including dev)
uv sync --all-extras

# Run tests
uv run pytest

# Type check
uv run mypy src/

# Lint
uv run ruff check src/ tests/

# Format
uv run ruff format src/ tests/
```

Commits:
- Follow Conventional Commits: `feat:`, `fix:`, `refactor:`, `docs:`, `test:`
- Keep commits logically grouped (one concern per commit).

## Key Documents (Read Order)

1. `docs/product/ridgeline-ceo-strategic-constraints.md` — 5 CEO constraints (north star)
2. `docs/decisions/ADR-001-founding-decisions.md` — architecture decisions from expert panel
3. `docs/architecture.md` — system boundaries and dependency rules
4. `docs/product/ridgeline-ceo-system-prompt.md` — CEO agent prompt (for Claude Project collaboration)
5. `docs/research/` — design research and competitive analysis

## Coding Guidelines

Style:
- Follow ruff configuration in `pyproject.toml`.
- Prefer pure functions and explicit type annotations.
- Use Pydantic models for data structures (especially IR types).
- Avoid introducing new dependencies unless necessary.
- Optimize for readability and maintainability first.

Error handling:
- Use typed exceptions with context, not bare `Exception`.
- Do not swallow errors; log with context and reraise or map to domain errors.
- All HTTP fetches must handle timeouts and non-200 responses explicitly.

Testing:
- Tests in `tests/` using pytest.
- Write unit tests for new features before wiring into pipeline.
- Cache test fixtures — never hit live APIs in unit tests.

File naming conventions:
- Source modules: `src/ridgeline/{layer}/{feature}.py`
- IR models: `src/ridgeline/ir/{entity}.py`
- Tests: `tests/test_{module}.py` or `tests/{layer}/test_{feature}.py`

## Things to Avoid

- Creating new top-level directories without explicit instruction.
- Large, sweeping refactors in a single change.
- Generating synthetic data and presenting it as real federal data.
- Skipping provenance — every output must cite its source.
- Trying to handle execution directly when a deterministic tool exists.

## Available Skills & Commands

**Commands:**
- `/start-project` — Guided workflow from idea to implementation plan.

**Skills:**
- `code-review` — Structured code review with severity-grouped findings.

Discover all available skills: check `.claude/skills/` for SKILL.md files.

## Process Intelligence (.meta/)

This project captures development insights and process observations in `.meta/`
(gitignored — see `.meta.example/` for structure and conventions).

**Insight capture (automatic):** After producing any `★ Insight` block, append
it to `.meta/insights.md` with date, session name, category, context, and
trigger. Also add a one-liner to `.meta/insights-index.csv`.

Categories: `architecture`, `frontend`, `backend`, `workflow`, `tooling`,
`data`, `testing`, `devex`, `strategic`.

**Captain's log (situational):** When the user flags friction, a gap, a win,
or an idea — or when you notice significant process friction — append an entry
to `.meta/captains-log.md` with type, timestamp, and description.

Types: `gap`, `win`, `friction`, `idea`, `template-improvement`.

**Review cycle:** Periodically review `.meta/` to deduplicate, categorize,
and promote recurring patterns to CLAUDE.md, ADRs, or template improvements.
See `docs/developer-workspace.md` for the full review workflow.

## Self-Improvement Loop

When you encounter friction — confusing instructions, missing tools,
repeated manual steps, or unexpected failures:

1. **Identify** what broke or caused friction.
2. **Propose** a specific fix with rationale. Do NOT apply yet.
3. **Ask**: "Should I update [file] with [change]?"
4. **Apply** only after receiving permission.
5. **Log it** — append to `.meta/captains-log.md` if the friction
   is a pattern worth tracking.

Rules:
- Do NOT create or overwrite workflows, skills, or CLAUDE files without
  asking permission first.
- Keep changes minimal and reversible.
- If the fix is structural, create an ADR in `docs/decisions/` first.
