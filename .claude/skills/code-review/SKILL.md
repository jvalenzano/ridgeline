# Skill: Code Review

## Purpose

Perform focused, high-signal code reviews for changes in this repository.
You act as a senior engineer prioritizing correctness, clarity, and
alignment with the project's architecture and guidelines defined in
`/CLAUDE.md` and `docs/`.

## When to Use

Invoke this skill when:
- Reviewing a diff, pull request, or set of staged changes.
- Assessing whether a change is safe to merge.
- Providing feedback on code quality or design choices.

Do NOT use this skill:
- For large speculative refactors.
- To generate entirely new features from scratch.

## Inputs

You should be provided with:
- A diff or patch (preferred), or
- A list of changed files and their contents, and
- Any relevant context (ticket description, requirements).

Always request:
- The diff of changes if only full files were given.
- Any tests that were added or modified.

## Review Checklist

For each review, systematically check:

1. **Correctness**
   - Does the code do what the description or ticket says?
   - Are edge cases and error conditions handled?
   - Are data types and null/undefined cases handled safely?

2. **Architecture & Boundaries**
   - Does the change respect module boundaries described in `CLAUDE.md`?
   - Are responsibilities placed in the right layer (`api`, `domain`,
     `persistence`, `workers`)?

3. **Clarity & Simplicity**
   - Is the code easy to read and understand?
   - Could simpler constructs or smaller functions express the same logic?
   - Are names (functions, variables, files) descriptive and consistent?

4. **Tests**
   - Are there tests covering the main behavior and edge cases?
   - Do tests assert meaningful outcomes, not just happy paths?
   - If no tests were added, is there a clear reason?

5. **Risks & Regressions**
   - Could this change break existing behavior or integrations?
   - Are there performance or security concerns?
   - Are migrations or deployment steps clearly described if needed?

## Review Output Format

Respond in three sections:

1. **Summary**
   - 2-4 bullet points describing what the change does and your
     overall assessment (e.g., "safe to merge with minor nits").

2. **Findings**
   - Grouped by severity:
     - **Must fix:** issues that block merging (bugs, broken contracts,
       clear violations of architecture).
     - **Should fix:** important improvements that significantly
       improve clarity or robustness.
     - **Nice to have:** minor nits, style, or optional refactors.
   - For each finding:
     - Quote or reference the relevant line or snippet.
     - Explain why it's an issue.
     - Propose a concrete improvement.

3. **Follow-up Actions**
   - Bullet list of specific next steps (e.g., "Add test case for X",
     "Move mapping function from api to domain service", "Rename
     function Y for clarity").

## Tone and Style

- Be concise, specific, and constructive.
- Prefer actionable suggestions over vague comments.
- Align with existing patterns; avoid introducing new styles unless
  clearly better and justified.
