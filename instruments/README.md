# Instrument Framework

A modular collection of agent protocols for Claude Code. Each instrument defines a specialized working mode with its own protocol, templates, and workflow.

## Setup

### 1. Add instruments to your project

Copy or symlink the `instruments/` directory into your project root:

```bash
# Option A: clone directly into your project
git clone git@github.com:hipudding/instruments.git

# Option B: add as a git submodule
git submodule add git@github.com:hipudding/instruments.git
```

### 2. Configure `~/.claude/CLAUDE.md`

Add the following to your global `~/.claude/CLAUDE.md` (create the file if it doesn't exist):

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

### 3. Done

Start a new Claude Code session in your project. The agent will automatically detect `instruments/README.md` and load the framework.

## How It Works

1. On session start, the global `~/.claude/CLAUDE.md` directs the agent to read this file.
2. The agent checks which instrument to activate based on user request or project context.
3. Once activated, the agent reads the instrument's `PROTOCOL.md` and follows it.

## Available Instruments

| Instrument | Directory | Description | Activation |
|------------|-----------|-------------|------------|
| Architect  | `architect/` | Decomposes complex tasks into sub-tasks, coordinates execution (including parallel dispatch of independent tasks), tracks progress via file-based state. | User says "architect mode", or project has `.tasks/plan.md` |

## Shared Resources

| File | Purpose |
|------|---------|
| `memory.md` | Persistent user preferences, credentials, habits — shared across all instruments. Loaded on every session start. |

## Adding a New Instrument

1. Create a new directory under `instruments/`: `instruments/{name}/`
2. Add a `PROTOCOL.md` defining the agent's behavior, phases, and conventions.
3. Add any templates under `instruments/{name}/templates/`.
4. Register the instrument in the table above.

## Conventions

- Each instrument is self-contained in its own directory.
- `PROTOCOL.md` is the entry point for every instrument.
- `memory.md` at the top level is shared — all instruments read and write to it.
- Instruments should not depend on each other unless explicitly documented.
