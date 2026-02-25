# Instruments

A modular protocol framework for [Claude Code](https://docs.anthropic.com/en/docs/claude-code). It gives your AI agent structured working modes (instruments) with persistent memory, crash recovery, and multi-agent coordination.

## Quick Start

### 1. Install (one-time)

```bash
git clone git@github.com:hipudding/instruments.git ~/.claude/instruments
```

That's it. The instruments are now globally available to all projects.

### 2. Configure Claude Code

Add the following to `~/.claude/CLAUDE.md` (create the file if it doesn't exist):

```markdown
# Global Instructions

## Instrument Framework

IMPORTANT: You MUST follow the steps below at the start of EVERY session, BEFORE responding to the user's first message.

1. Check if `~/.claude/instruments/README.md` exists.
2. If it exists:
   a. Run `git -C ~/.claude/instruments pull --ff-only -q` to auto-update from remote (ignore failures silently — network may be unavailable).
   b. Read `~/.claude/instruments/README.md` — discover available instruments.
   c. Read `~/.claude/memory.md` if it exists — load persistent user preferences. Apply these preferences throughout the session.
   d. Determine which instrument to activate:
      - If `.tasks/plan.md` exists → MUST activate the **Architect** instrument: read `~/.claude/instruments/architect/PROTOCOL.md` and enter Resume mode.
      - If the user explicitly requests a mode (e.g. "architect mode") → activate the corresponding instrument.
      - If the task contains multiple obviously independent work items (e.g. resolving conflicts in separate files, writing impl + tests) → consider activating the **Architect** instrument to decompose and parallel-dispatch.
      - Otherwise → proceed normally. The user can activate an instrument at any time.
3. If `~/.claude/instruments/README.md` does not exist → run `git clone git@github.com:hipudding/instruments.git ~/.claude/instruments` to install, then go to step 2.
```

### 3. Use it

Start a Claude Code session in any project:

```
> architect mode, build a user authentication system

# The agent will:
# 1. Decompose the task into sub-tasks
# 2. Create .tasks/plan.md and versioned task files
# 3. Dispatch sub-agents (in parallel when possible)
# 4. Track progress, handle failures, verify results
```

## How It Works

```
~/.claude/
  CLAUDE.md                    # Global config (points to instruments)
  memory.md                    # Persistent user preferences (not in repo)
  instruments/                 # Installed once, available to all projects
    README.md                  #   Instrument registry (agent entry point)
    architect/                 #   Architect instrument
      PROTOCOL.md              #     Main protocol
      DEVELOPER.md             #     Sub-agent protocol
      templates/               #     Plan and task templates
        plan.md
        task.md

{any-project}/
  .tasks/                      # Runtime state (per project, gitignore-able)
    plan.md                    #   Current plan
    plan-v1.md                 #   Archived plan (created on re-plan)
    v1-setup-auth/             #   Task files for plan v1
      01-init-db.md
      02-create-models.md
```

## Framework Features

- **Global install** — Install once in `~/.claude/instruments/`, use in any project. No per-project setup needed.
- **Persistent memory** — The agent records your preferences, conventions, and environment details in `~/.claude/memory.md`, separate from instrument code and shared across all projects.
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

1. Create `~/.claude/instruments/{name}/PROTOCOL.md`
2. Register it in `~/.claude/instruments/README.md`

See `architect/` for a complete example.

## Updating

```bash
cd ~/.claude/instruments && git pull
```

## License

MIT
