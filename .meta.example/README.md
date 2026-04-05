# .meta/ — Process Intelligence Layer

> **This directory is a template.** Copy it to `.meta/` to start capturing
> process intelligence for your development sessions.
>
> ```bash
> cp -r .meta.example/ .meta/
> # .meta/ is already in .gitignore — your captures stay local
> ```

## What This Is

`.meta/` captures **process metadata** — not about the code you're writing, but
about *how* it gets written. Insights, friction points, workflow observations,
and template improvement ideas that arise during development sessions.

This is the developer equivalent of a pilot's logbook: operational intelligence
that improves the craft over time.

## Files

| File | Purpose | Updated by |
|------|---------|------------|
| `insights.md` | Running log of `★ Insight` blocks produced by the agent | Agent (auto-appended after each insight) |
| `insights-index.csv` | Machine-readable index for scanning and filtering insights | Agent (auto-appended) |
| `captains-log.md` | Broader observations: friction, gaps, wins, ideas | Developer or agent |
| `review-notes/` | Outputs from periodic review and deduplication passes | Developer or `/review-meta` skill |

## How It Works

### Automatic Capture (Insights)

The agent is instructed via `CLAUDE.md` to append every `★ Insight` block to
`.meta/insights.md` with metadata (date, session, category, context, trigger).
A one-liner also goes into `insights-index.csv` for quick scanning.

**Categories:** `architecture`, `frontend`, `backend`, `workflow`, `tooling`,
`data`, `testing`, `devex`, `strategic`

### Manual Capture (Captain's Log)

When you notice something — a friction point, a gap in tooling, something that
worked surprisingly well — tell the agent "log that" or "add to captain's log."
Or append directly to `captains-log.md`.

**Types:** `gap`, `win`, `friction`, `idea`, `template-improvement`

### Periodic Review

Every 1-2 weeks, review `.meta/` to:

1. **Deduplicate** insights that say the same thing across sessions
2. **Categorize** any uncategorized entries
3. **Promote** recurring patterns to `CLAUDE.md` or a new ADR in `docs/decisions/`
4. **Extract** template improvements → backlog for the upstream template
5. **Archive** stale observations to `review-notes/`

## Why Gitignored?

Process intelligence is **personal and ephemeral**. Two developers working on the
same repo will have different insights based on their experience level, focus area,
and session history. Committing these to git would create noise and merge conflicts.

The `.meta.example/` directory (tracked in git) shows the structure and conventions.
Each developer copies it to `.meta/` and builds their own capture history.

## Feeding Back to the Template

The most valuable outcome of `.meta/` is **template improvements**. When captain's
log entries tagged `template-improvement` accumulate, they become PRs against the
upstream `claude-code-template`. This is the slow feedback loop:

```
Session insights → .meta/ capture → periodic review → template PR → all future projects benefit
```
