# Persistent Memory

This is the template for `~/.claude/memory.md`. The actual file lives **outside** this repo at `~/.claude/memory.md` because it contains user-specific data.

When this file does not yet exist at `~/.claude/memory.md`, copy this template there to initialize it.

## How Agents Should Use Memory

- **Read** `~/.claude/memory.md` at the start of every session (handled by `CLAUDE.md`).
- **Write** to it whenever you learn something persistent about the user: preferences, credentials, environment details, conventions, or project notes.
- **Never delete** existing entries unless the user explicitly asks.
- **Do not store raw secrets** — store references (e.g. file path, vault location) instead.

---

## Template

```markdown
# Persistent Memory

## User Preferences
<!-- Coding style, preferred frameworks, language preferences, formatting conventions. -->

## Credentials & Tokens
<!-- Store identifiers and references, NOT raw secrets.
     For raw secrets, note the file path (e.g. .env) or vault location. -->

## Environment
<!-- OS, shell, editor, default branches, CI/CD setup, toolchain versions. -->

## Habits & Conventions
<!-- Commit message style, PR workflow, branch naming, test preferences, review process. -->

## Project Notes
<!-- Architecture decisions, known tech debt, gotchas, important context. -->
```
