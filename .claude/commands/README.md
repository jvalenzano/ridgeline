# Claude Code Commands

Custom commands that can be invoked via `/command-name` in Claude Code.

## Available Commands

| Command | File | Purpose |
|---------|------|---------|
| `/start-project` | `start-project.md` | Guide a developer from a rough idea to a concrete implementation plan |

## Adding a New Command

1. Create a new `.md` file in this directory (e.g., `my-command.md`).
2. Follow this structure:
   - **Purpose**: What the command does and when to use it.
   - **Inputs**: What information the command needs from the developer.
   - **Steps**: Ordered workflow with confirmation gates.
   - **Output Format**: What the command produces.
   - **Tone and Style**: How the command should communicate.
3. Reference the root `CLAUDE.md` for project-wide rules.
4. Keep commands focused on a single workflow — compose multiple commands rather than building monoliths.

## Design Principles

- Every command should **pause for confirmation** before making changes.
- Commands should be **self-documenting**: a developer reading the .md file should understand the workflow without external context.
- Prefer **small, reversible steps** over large automatic changes.
- End each command session with a **feedback prompt** to improve the template over time.
