# Hook: Structure Validation

## When to Run

This hook should be checked before committing changes that:
- Create new files or directories
- Move files between modules
- Add new top-level directories

## Rules

1. **No orphan top-level directories.**
   Every top-level directory must be one of: `src/`, `docs/`, `tools/`,
   `.claude/`, `node_modules/`, `dist/`, `.traces/`.
   If a new top-level directory is needed, create an ADR first.

2. **Module boundaries enforced.**
   - `src/api/` must NOT import from `src/persistence/`.
   - `src/persistence/` must NOT import from `src/api/` or `src/workers/`.
   - `src/domain/` must NOT import from `src/api/` or `src/workers/`.
   - `src/workers/` may import from `src/domain/` only.

3. **Required files must exist.**
   Every commit must preserve:
   - `CLAUDE.md` (root)
   - `docs/architecture.md`
   - `.claude/settings.json`

4. **New modules need a CLAUDE.md.**
   If a new directory is created under `src/`, it should include a
   `CLAUDE.md` with at least: purpose (2-3 sentences), 3+ Do rules,
   3+ Don't rules.

5. **Tests accompany new code.**
   New `.ts` files in `src/domain/` or `src/api/` should have a
   corresponding test file in `__tests__/`.

## How to Enforce

These rules can be enforced at different levels:

### Manual (recommended starting point)
Include this checklist in code reviews. The `code-review` skill
already checks architecture and boundaries.

### Automated (when ready)
Add a script to `tools/scripts/` that validates these rules:

```bash
# Example: tools/scripts/check-structure.sh
#!/usr/bin/env bash
set -euo pipefail

# Check required files exist
for f in CLAUDE.md docs/architecture.md .claude/settings.json; do
  if [[ ! -f "$f" ]]; then
    echo "ERROR: Required file missing: $f"
    exit 1
  fi
done

# Check no forbidden imports (basic grep)
if grep -r "from.*src/persistence" src/api/ 2>/dev/null; then
  echo "ERROR: src/api/ must not import from src/persistence/"
  exit 1
fi

echo "Structure check passed."
```

### CI integration
Add the script to your CI pipeline:
```yaml
# .github/workflows/ci.yml
- name: Structure check
  run: bash tools/scripts/check-structure.sh
```
