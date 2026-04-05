# ADR-001: Founding Decisions — Repo Architecture, Stack, and Product Direction

**Status**: Accepted
**Date**: 2026-04-05
**Author**: 6-expert strategic panel + CEO advisory session

> **Full deliberation records** (preserved in Vista repo for historical reference):
> - `vista/docs/product/ridgeline-repo-strategy-panel-briefing.md` (64K — 4-round, 6-expert debate)
> - `vista/docs/product/vista-briefing-engine-concept.md` (32K — multimedia briefing vision)
> - `vista/docs/product/vista-strategic-go-no-go-report.md` (17K — go/no-go panel report)

---

## Context

Vista (a 4D operations theater for national forests) reached Phase 6 completion as
a CesiumJS + Google 3D Tiles globe with narrated fly-throughs. Strategic review
revealed a fundamental pivot: the user cares about the *intelligence briefing*, not
the *3D map*. The question became: how do you build an AI briefing engine that
produces Post-Fire Intelligence Summaries from public federal data?

Key forces:
- Vista's 3D globe is a portfolio showcase, but it's not what practitioners need day-to-day.
- The Director pipeline pattern (LLM plans → deterministic compilation → output) is sound, but the VistaIR types are scene-specific, not document-specific.
- Solo developer. Any architecture must earn its complexity through tangible output.
- Federal data sources (NIFC, NWS, USGS) have real quirks: schema drift, rate limits, temporal gaps.
- BAER practitioners are the target audience. They have strong conventions and zero patience for tools that don't speak their language.

## Decisions

### 1. Separate repos with heritage bridge (Option B)

Ridgeline is a standalone repo on personal GitHub (`jvalenzano/ridgeline`), not a
subdirectory of Vista or a monorepo. Ridgeline inherits *domain knowledge and
architectural patterns* from Vista — not code, not types, not dependencies.

Vista's 3D globe becomes Ridgeline's optional Map tab in Milestone 4+ via iframe embed.

### 2. Python-first stack

Python 3.11+, `uv` for package management, Pydantic for data models. No TypeScript
until a frontend is needed (Milestone 3+). The data pipeline, IR, and document
rendering are all Python.

### 3. New RidgelineIR (not ported VistaIR)

The VistaIR schema (233 lines of TypeScript) describes *scenes* — camera targets,
shot types, layer emphasis. Ridgeline's IR describes *documents* — sections, claims,
citations, temporal tier markers. The architectural *pattern* transfers (LLM as
planner, IR as stable contract, deterministic compilation). The *types* don't.

RidgelineIR is implemented as Python Pydantic dataclasses.

### 4. Design-first methodology

User journeys → document design → wireframes → technical specs → code. No
implementation before the output format is defined.

### 5. Springs Fire as first target

The Springs Fire (SoCal, 2026) is the first data target. One fire, one output
format (Post-Fire Intelligence Summary), one script (`springs_fire.py`).

### 6. Bridge Fire BAER report as design reference

The Bridge Fire BAER Technical Report (Angeles NF, Sept 2024, 54,659 acres) is the
design target. Ridgeline's output should feel like a *precursor* to that document.

## Consequences

- **Positive**: Clean codebase with no Vista technical debt. Python ecosystem is ideal for data pipelines and federal API integration. RidgelineIR can be designed for document output from scratch. Solo developer avoids monorepo coordination overhead.
- **Positive**: Design-first approach prevents building infrastructure before the output format is validated.
- **Negative**: No code reuse from Vista's 600+ commits. Director pipeline logic must be reimplemented.
- **Negative**: Two repos to maintain. Vista integration (Map tab) requires explicit bridge work later.
- **Neutral**: The heritage bridge is *knowledge*, not code — strategic constraints, domain research, and architectural patterns transfer through documentation, not imports.

## Alternatives Considered

1. **Option A — Monorepo (Vista + Ridgeline)**: Rejected because it couples a TypeScript CesiumJS codebase to a Python data pipeline. Shared tooling overhead exceeds benefit for a solo developer. The two products have different deployment targets, test strategies, and dependency chains.

2. **Option C — Fork Vista, refactor**: Rejected because Vista's src/ is 100% TypeScript/CesiumJS scene-planning code. A fork would inherit thousands of lines of irrelevant code and 1,246 tests for a globe renderer Ridgeline doesn't use.

## Panel Composition

| Expert | Domain | Vote |
|--------|--------|------|
| Elena Vasquez | AI Pipelines (ex-Google Brain) | B with schema contract |
| Marcus Chen | Federal Data Systems (ex-USGS) | B |
| Sarah Rodriguez | Gov-Tech Product Management | B |
| James Kato | Platform/DevOps (GCP-native) | B |
| Dr. Andrea Morrison | Former USFS BAER Team Lead | B |
| Tom Reeves | Devil's Advocate / CTO-for-hire | B |

**Confidence**: HIGH (unanimous). Key condition: a real document artifact must exist
within the first week. Architecture is subordinate to tangible output.
