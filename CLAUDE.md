# CLAUDE.md

## What this is
RFC / decision-record repo. Docs-only — no executable code. The documents are the artifact.

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
