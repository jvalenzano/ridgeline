# Developer Workspace Guide

> How to set up your local workspace files. These are personal, gitignored
> files that customize your development environment without affecting the
> shared codebase.

## The `.example` Convention

This template follows the `.env.example` pattern: tracked template files
show the expected structure, and developers copy them to gitignored locations
for personal use.

```
Tracked (in git)              →  Local (gitignored)
─────────────────────────────────────────────────────
.env.example                  →  .env
.meta.example/                →  .meta/
```

When you clone this repo, run these copy commands to set up your workspace:

```bash
# Environment variables
cp .env.example .env
# Edit .env with your actual values

# Process intelligence layer
cp -r .meta.example/ .meta/
# Your insights and observations accumulate here
```

## Local Workspace Files

### `.env` — Environment Variables

**Source template:** `.env.example`

Contains secrets, API keys, and environment-specific configuration.
Never committed to git. See `.env.example` for required variables.

### `.meta/` — Process Intelligence

**Source template:** `.meta.example/`

Captures development process metadata — agent insights, friction points,
workflow observations, and template improvement ideas. See
[`.meta.example/README.md`](../.meta.example/README.md) for full documentation.

| File | Purpose |
|------|---------|
| `insights.md` | Running log of agent insights (auto-appended) |
| `insights-index.csv` | Machine-readable insight index |
| `captains-log.md` | Manual observations: gaps, wins, friction, ideas |
| `review-notes/` | Periodic review and deduplication outputs |

### `.agent/memory/` — Agent Session Memory

The `.agent/memory/` directory is tracked in git for the **condensed memory**
file (`condensed-memory.md`), which provides session handoff context. However,
individual session files (`session-*.md`) are gitignored — they're
developer-local records of specific sessions.

| File | Tracked? | Purpose |
|------|----------|---------|
| `condensed-memory.md` | Yes | Shared session handoff (current project state) |
| `session-*.md` | No (gitignored) | Individual session records (developer-local) |

### `.traces/` — Agent Telemetry

Gitignored. Used by the telemetry system (`src/telemetry/`) to store
session traces in JSONL format. Useful for debugging agent behavior
and measuring session performance.

### `.superpowers/` — Superpowers Plugin State

Gitignored. Working directory for the Superpowers outer-loop tooling
(worktree management, plan state, etc.).

## Future Workspace Files

As the template evolves, additional `.example` files may be added:

| Planned | Purpose |
|---------|---------|
| `.vscode.example/` | Recommended VS Code settings and extensions |
| `.devcontainer.example/` | Dev container configuration for consistent environments |
| `.claude.example/settings.local.json` | Developer-local Claude Code settings overrides |

When new `.example` files appear after a template update, copy them to
their local counterparts to pick up the new conventions.

## Maintaining Your Workspace

### After cloning or pulling template updates

Check for new or updated `.example` files:

```bash
git diff --name-only HEAD~1 -- '*.example*' '.meta.example/'
```

If any changed, diff your local version against the new template and
merge manually.

### Periodic review of .meta/

Every 1-2 weeks:
1. Review `insights.md` — deduplicate, categorize, archive stale entries
2. Review `captains-log.md` — extract `template-improvement` entries into PRs
3. Save review output to `review-notes/YYYY-MM-DD-review.md`
