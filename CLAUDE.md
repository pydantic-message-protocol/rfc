# CLAUDE.md

## What this is
RFC repository for **PMP (Protocol for Message Passing)**, org `pydantic-message-protocol`. Docs-only — no executable code. First spec: `rfcs/0000-pmp-spool.md` (PMP RFC 0, the Spool filesystem transport profile). Pydantic is the Python reference implementation's object model, not part of the protocol itself. A separate Python implementation repo (`pmp-python` or `python-pmp`, name TBD) will consume this spec.

## Constraints
- Docs-only. No code, package managers, or build tooling unless a decision in `DECISIONS.md` explicitly approves it.
- Commit convention: Conventional Commits (`feat:`, `fix:`, `docs:`, `chore:`, ...).
- Data location: undecided — see `DECISIONS.md` (BLOCKING).

## Control files
- `TASKS.md` — active to-do, up next, backlog.
- `DECISIONS.md` — decisions with rationale and status (open / blocking / decided).
- `IDEAS.md` — scratch. Everything lands here first, gets promoted to `TASKS.md` or `DECISIONS.md`.

## Cross-project rules
Not here. Cross-project preferences (Python/uv environment, the control-file bootstrap methodology, commit-isolation for control files) live in `~/.claude/CLAUDE.md` (user-level, auto-loaded for every project).
