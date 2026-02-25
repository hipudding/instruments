# Instrument Framework

A modular collection of agent protocols. Each instrument defines a specialized working mode with its own protocol, templates, and workflow.

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
