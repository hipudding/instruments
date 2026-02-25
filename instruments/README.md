# Instrument Framework

A modular collection of agent protocols. Each instrument defines a specialized working mode with its own protocol, templates, and workflow.

## How It Works

1. On session start, the global `~/.claude/CLAUDE.md` directs the agent to read this file.
2. The agent checks which instrument to activate based on user request or project context.
3. Once activated, the agent reads the instrument's `PROTOCOL.md` and follows it.
4. Runtime state (plans, tasks) is stored in `.tasks/` inside the current project — not in this directory.

## Available Instruments

| Instrument | Directory | Description | Activation |
|------------|-----------|-------------|------------|
| Architect  | `architect/` | Decomposes complex tasks into sub-tasks, coordinates execution (including parallel dispatch of independent tasks), tracks progress via file-based state. | User says "architect mode", or project has `.tasks/plan.md` |

## Adding a New Instrument

1. Create a new directory: `{instruments-root}/{name}/`
2. Add a `PROTOCOL.md` defining the agent's behavior, phases, and conventions.
3. Add any templates under `{instruments-root}/{name}/templates/`.
4. Register the instrument in the table above.

## Persistent Memory

User preferences and context are stored in `~/.claude/memory.md` — **outside** this repo (it's user data, not instrument code).

- **Read** it at session start (handled by `CLAUDE.md`).
- **Write** to it when you learn persistent information: preferences, credentials, environment, conventions.
- If the file doesn't exist, initialize it from the template at `memory.template.md` in this directory.
- See `memory.template.md` for the full format and usage guide.

## Conventions

- Each instrument is self-contained in its own directory.
- `PROTOCOL.md` is the entry point for every instrument.
- Instruments should not depend on each other unless explicitly documented.
- Persistent user data goes in `~/.claude/` (e.g. `memory.md`), not in this repo.
