# Command: /start-project

## When to use this vs. the Standard Workflow

Use `/start-project` when you're starting something new — a new feature,
a new project from the template, or onboarding to an unfamiliar codebase.
It provides guided prompts and confirmation gates at every step.

Use the **Standard Workflow** (in root `CLAUDE.md`) for ongoing work
where you already know the codebase and just need the
Clarify → Plan → Execute → Validate → Summarize sequence.

## Purpose

Guide a developer from a rough idea to a concrete, organized plan
within this template repository. This command should:

- Clarify the developer's objective in plain language.
- Decide where the work belongs in the project structure.
- Initialize or update key docs (CLAUDE.md, architecture, decisions).
- Produce a small, ordered implementation plan.
- Suggest next commands/skills (e.g., code review) to continue.

Use concise language and small, reversible steps.

---

## How to Run

Developer runs:

> /start-project

Then answers your questions in natural language.

---

## Step 1 – Gather Inputs

Ask the developer, one section at a time:

1. **Objective**
   - "In 2-3 sentences, what are you trying to build or change?"

2. **Context**
   - "Is this a new project or an existing one?"
   - "Which parts of the system are likely involved? (api, domain, persistence, workers, tools, other)"

3. **Constraints**
   - "Any hard constraints? (deadlines, performance, security, compliance, must-use technologies)"
   - "Any parts of the codebase you must NOT touch?"

4. **Current state**
   - "Are there any existing tickets, docs, or prior attempts I should be aware of?"

### Examples to help the developer

If the developer is unsure how to answer, share these worked examples:

> **Example A — New API feature:**
> - Objective: "Add a /invoices endpoint that lets clients create and retrieve invoices."
> - Context: "New project, touches api, domain, and persistence."
> - Constraints: "Must use Zod for validation. No new dependencies beyond what's in package.json."
> - Current state: "No prior attempts. There's a rough spec in the ticket linked in Jira."

> **Example B — Refactoring existing code:**
> - Objective: "Extract the email notification logic from the order handler into a standalone domain service."
> - Context: "Existing project. Mainly domain and workers."
> - Constraints: "Don't change the API contract. Must keep existing tests passing."
> - Current state: "The current implementation is in src/api/orders.ts lines 120-180. It works but it's in the wrong layer."

> **Example C — Adding observability:**
> - Objective: "Wire up OpenTelemetry tracing for all database queries so we can see slow queries in our dashboard."
> - Context: "Existing project. Persistence layer and tools/scripts."
> - Constraints: "Must be zero-overhead when tracing is disabled. FedRAMP-compliant exporters only."
> - Current state: "We have a basic logger but no structured tracing yet."

Summarize their answers back in 3-5 bullet points and confirm:

> "Here's my understanding of your goal and constraints. Is this correct?
> If not, please correct anything that's wrong or missing."

Do not proceed until they confirm or correct.

### If they say "no" or want changes

At any confirmation gate in this workflow, if the developer disagrees:
1. Ask what specifically is wrong or missing.
2. Revise only the part they flagged.
3. Show the revised version and confirm again.
4. If they want to abandon this step entirely, skip it and note it
   as an open item. Never force a step.

---

## Step 2 – Map to project structure

Using the template's conventions and root `CLAUDE.md` (also read the
module-local `CLAUDE.md` files in `src/api/`, `src/domain/`, and
`src/persistence/` for module-specific rules):

1. Identify relevant modules:
   - Suggest where the work belongs (e.g., `src/api`, `src/domain`, `src/persistence`, `src/workers`, `tools/scripts`).
   - If you think a new module or folder is needed, propose it and explain why.

2. Output a short mapping like:

- Affected modules:
  - `src/domain/...` — core logic for X.
  - `src/api/...` — expose endpoint Y.
  - `src/persistence/...` — data access for Z.

Ask for confirmation:

> "Does this module mapping look right for what you have in mind?"

Adjust if they disagree.

---

## Step 3 – Update or initialize documentation

Work with these docs:

- `CLAUDE.md` (root)
- `docs/architecture.md`
- `docs/decisions/` (ADRs as needed)

Follow this sequence:

1. **Root CLAUDE.md**
   - If this is a brand-new project, fill in any obvious placeholders
     (e.g., PROJECT_NAME, primary frameworks) using their answers.
   - If it's an existing project, propose small edits, such as:
     - Adding the new feature to the "Architecture Overview".
   - Show a diff-style or clearly marked proposed change and ask:

     > "Here is how I propose to update CLAUDE.md. Do you want me to apply these changes?"

2. **Architecture doc**
   - In `docs/architecture.md`, add or update a short section describing
     the new component/feature and how it fits existing boundaries.
   - Keep it high-level and bullet-pointed; do not over-document.

3. **Decisions (optional)**
   - If the change introduces a significant architectural decision
     (new dependency, new service, new persistence model), propose an ADR
     in `docs/decisions/` using the template in `docs/decisions/TEMPLATE.md`.
   - Ask if they want the ADR created now or later.

Always show the proposed content before editing files, and only apply
changes once they approve.

---

## Step 4 – Create an implementation plan

Produce an ordered, minimal plan of 5-10 steps, each aimed at a small,
reversible change. Use this structure:

- **Step 1 – Setup / discovery**
  - e.g., "Inspect existing modules A and B related to feature X."

- **Step 2 – Domain changes**
  - e.g., "Add new domain model or use case in src/domain/... with tests."

- **Step 3 – Persistence changes**
  - e.g., "Extend repository in src/persistence/... to store/retrieve Y."

- **Step 4 – API changes**
  - e.g., "Add handler in src/api/... to expose endpoint Z."

- **Step 5 – Validation**
  - e.g., "Add/extend tests; run them or describe how to run them."

- **Step 6 – Cleanup & docs**
  - e.g., "Update docs/architecture.md if needed; ensure comments are accurate."

For each step:
- Specify which files or directories are likely to be touched.
- Note whether you expect to use other skills (e.g., `code-review`).
- Identify existing tools in `tools/scripts/` that could be used.

Present the plan and ask:

> "Here is a proposed plan. Which step would you like to start with?
> We can adjust the plan before we begin."

Do not change any code until they select a step.

---

## Step 5 – Execute the chosen step

When the developer picks the first step:

1. Restate the step in your own words.
2. Check `tools/scripts/` for existing tools that can help.
3. Identify the exact files you will open or create.
4. Perform minimal, focused edits required for that step.
5. Summarize changes done in 2-3 bullets.

If tests are relevant for this step, either:
- Run them and summarize results, or
- If you cannot run them, clearly state how they should be run.

End each step with:

> "Would you like to proceed to the next step, revise this step, or stop here?"

### When something fails

Follow the self-improvement loop from CLAUDE.md:
1. Identify what broke — read the full error message and trace.
2. Fix the tool or code and verify the fix works.
3. Update the relevant workflow or doc with what you learned.
4. Move on with a more robust system.

If using paid APIs or credits, check with the developer before retrying.

---

## Step 6 – Code review and next actions

After at least one implementation step:

1. Run the `code-review` skill on the changes:

   > "I'll now review the changes we've made using the code-review skill
   > to catch issues before we continue."

2. Present the review findings and address any "Must fix" items.

3. Offer clear next actions:

   > "Your options:
   > (a) Fix the issues found in the review and re-review.
   > (b) Proceed to the next implementation step.
   > (c) Stop here and commit what we have.
   > Which would you like?"

---

## Step 7 – Capture template feedback

At the end of the `/start-project` session, ask:

- "What, if anything, felt confusing or inconvenient about this workflow?"
- "Is there anything you wish this template or command had done for you?"

If they provide feedback:
1. Summarize it.
2. Propose a specific, minimal change to the relevant file
   (CLAUDE.md, this command, or a skill).
3. Ask permission before editing any meta files.

---

## Tone and Style

- Be direct, organized, and concise.
- Avoid long monologues; pause for confirmation at each major step.
- Favor simple language that a new teammate can understand quickly.
- Default to small, incremental changes over big refactors unless the
  developer explicitly requests otherwise.
- Look for existing tools before building new ones.
