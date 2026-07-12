# DECISIONS.md

Entry format:
```
## [STATUS] Title
- Date:
- Rationale:
- Status: OPEN | BLOCKING | DECIDED
```

## [DECIDED] Data location
- Date: 2026-07-10
- Rationale: Resolved by the PMP design chat — messages live as JSON files in filesystem spool directories (`tmp/`, `pending/`, `processing/`, `completed/`, `failed/`). See `rfcs/0000-pmp-spool.md` §4.1.
- Status: DECIDED

## [DECIDED] Protocol scope and naming
- Date: 2026-07-10
- Rationale: PMP = "Protocol for Message Passing," not "Pydantic Message Protocol." Pydantic is the Python reference implementation's object model, not part of the protocol — consumers must not need to know or care what the producer used to build the JSON. Layers: Application → Object model → PMP Envelope (JSON) → Transport profile (Spool is the first profile).
- Status: DECIDED

## [DECIDED] Sequence allocation
- Date: 2026-07-10
- Rationale: Multiple local producers are permitted. Sequence numbers are serialized via an advisory lock on a shared `spool/.sequence` file (lowercase hex, starting at 0) rather than restricting to a single producer.
- Status: DECIDED

## [DECIDED] Filename format and durability
- Date: 2026-07-10
- Rationale: `{sequence_hex}-{epoch_ms_hex}-{pid_hex}.pmp`; sequence is the authoritative spool-local id and must match the envelope's `sequence` field. Publication: write to `tmp/`, fsync file, atomic rename into `pending/`, fsync directory. Crash recovery in v0.0 is manual only (no automatic lease/timeout) to keep the spec deterministic.
- Status: DECIDED

## [DECIDED] License
- Date: 2026-07-11
- Rationale: Apache 2.0 confirmed — the GitHub repo was initialized with an Apache 2.0 `LICENSE` file, matching the earlier recommendation (explicit patent grant, fits a public protocol). Merged into local history from `origin/main`.
- Status: DECIDED

## [DECIDED] Python implementation repo name
- Date: 2026-07-11
- Rationale: `pmp-python`, under the `pydantic-message-protocol` org — groups consistently with hypothetical future per-language repos (`pmp-go`, `pmp-rust`).
- Status: DECIDED

## [DECIDED] CI scope for pmp-python
- Date: 2026-07-11
- Rationale: `pip install git+https://github.com/pydantic-message-protocol/pmp-python.git` already works with a plain PEP 517 `pyproject.toml` (hatchling) — no GitHub Action is required for that install path. A CI-only GitHub Action (build + run tests on every push/PR) still catches packaging regressions before they reach that install path. No PyPI publishing workflow for now — no code, tests, or release process exist yet; publishing automation is deferred until there's something to release.
- Status: DECIDED

## [DECIDED] Python package dependencies
- Date: 2026-07-11
- Rationale: `pydantic` is the only runtime dependency — the RFC frames Pydantic as the reference implementation's object model, so the envelope/payload types are Pydantic models. Everything else (atomic rename, fsync, advisory locking via `fcntl`, timestamps, filenames) is Python stdlib. No `jsonschema` dependency — the checked-in JSON Schema is for cross-language conformance tooling, not runtime use. Packaging uses a standard PEP 517 backend (hatchling) so `pip install git+https://github.com/pydantic-message-protocol/pmp-python.git` works without requiring `uv`; `uv` remains the local dev-environment tool only.
- Status: DECIDED

## [DECIDED] Org avatar art
- Date: 2026-07-12
- Rationale: Finalized spool-concept icon (concentric rings + core, slate/amber/blue) stored at `assets/org-icon.png`. The originally generated PNG had a checkerboard background baked into opaque pixels (no real alpha channel) rather than true transparency — converted locally to a genuine RGBA alpha channel before committing. First upload attempt was rejected by GitHub (1.07MB, over the 1MB avatar limit); resized to 512x512 (~127KB) which uploaded successfully. GitHub has no REST API endpoint for uploading org/user avatars — done manually via the web UI. Org avatar is now live.
- Status: DECIDED
