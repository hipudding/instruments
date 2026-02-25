# Instruments

A modular protocol framework for [Claude Code](https://docs.anthropic.com/en/docs/claude-code). It gives your AI agent structured working modes (instruments) with persistent memory, crash recovery, and multi-agent coordination.

## Quick Start

### 1. Add to your project

```bash
# Clone into your project root
cd your-project
git clone git@github.com:hipudding/instruments.git

# Or add as a submodule
git submodule add git@github.com:hipudding/instruments.git
```

### 2. Configure Claude Code

Add the following to `~/.claude/CLAUDE.md` (create the file if it doesn't exist):

```markdown
# Global Instructions

## Instrument Framework

IMPORTANT: You MUST follow the steps below at the start of EVERY session, BEFORE responding to the user's first message.

1. Check if `instruments/README.md` exists in the current project root.
2. If it exists:
   a. Read `instruments/README.md` — discover available instruments.
   b. Read `instruments/memory.md` if it exists — load persistent user preferences. Apply these preferences throughout the session.
   c. Determine which instrument to activate:
      - If `.tasks/plan.md` exists → MUST activate the **Architect** instrument: read `instruments/architect/PROTOCOL.md` and enter Resume mode.
      - If the user explicitly requests a mode (e.g. "architect mode") → activate the corresponding instrument.
      - If the task contains multiple obviously independent work items (e.g. resolving conflicts in separate files, writing impl + tests) → consider activating the **Architect** instrument to decompose and parallel-dispatch.
      - Otherwise → proceed normally. The user can activate an instrument at any time.
3. If `instruments/README.md` does not exist → proceed normally without instruments.
```

### 3. Use it

Start a Claude Code session in your project:

```
> architect mode, build a user authentication system

# The agent will:
# 1. Decompose the task into sub-tasks
# 2. Create .tasks/plan.md and versioned task files
# 3. Dispatch sub-agents (in parallel when possible)
# 4. Track progress, handle failures, verify results
```

## Project Structure

```
instruments/
  README.md                  # Agent entry point (instrument registry)
  memory.md                  # Persistent user preferences (shared across instruments)
  architect/                 # Architect instrument
    PROTOCOL.md              #   Main protocol (decomposition, execution, evaluation)
    DEVELOPER.md             #   Sub-agent protocol
    templates/               #   Plan and task templates
      plan.md
      task.md
```

Runtime state is stored in `.tasks/` at the project root (not inside `instruments/`):

```
.tasks/
  plan.md                    # Current plan
  plan-v1.md                 # Archived plan (created on re-plan)
  v1-setup-auth/             # Task files for plan v1
    01-init-db.md
    02-create-models.md
  v2-setup-auth/             # Task files for plan v2 (after re-plan)
    03-add-oauth.md
```

## Framework Features

- **Persistent memory** — The agent records your preferences, conventions, and environment details in `instruments/memory.md`, shared across all instruments, so it doesn't ask the same questions twice.
- **Extensible** — Add your own instruments by dropping a `PROTOCOL.md` into a new directory and registering it.

## Instruments

### Architect

Structured mode for complex, multi-step projects. Say `architect mode` or let the agent detect it automatically.

- **Task decomposition** — Breaks a complex objective into atomic, self-contained sub-tasks with explicit dependencies.
- **Parallel execution** — Independent tasks are dispatched to sub-agents concurrently. Each sub-agent has an explicit file boundary to prevent conflicts.
- **Re-planning** — When requirements change mid-project, creates a new plan version while preserving all historical task files. Completed work is never lost or re-executed.
- **Crash recovery** — All state is in files. If a session crashes, the next session reads `.tasks/plan.md` and resumes from where it left off.
- **Versioned history** — Task directories are versioned (`v1-slug/`, `v2-slug/`). Old versions are never deleted.

## Adding a New Instrument

1. Create `instruments/{name}/PROTOCOL.md`
2. Register it in `instruments/README.md`

See `instruments/architect/` for a complete example.

## License

MIT
