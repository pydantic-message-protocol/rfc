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

## [OPEN] License
- Date: 2026-07-10
- Rationale: Apache 2.0 was recommended in the design chat (explicit patent grant, fits a public protocol) but not yet explicitly confirmed. No LICENSE file has been added.
- Status: OPEN

## [OPEN] Python implementation repo name
- Date: 2026-07-10
- Rationale: Candidates are `pmp-python` vs `python-pmp` under the `pydantic-message-protocol` org. Not yet picked.
- Status: OPEN
