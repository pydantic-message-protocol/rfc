# PMP — Protocol for Message Passing

PMP is a small, filesystem-first protocol for exchanging asynchronous,
strongly-typed JSON messages between applications and repositories,
without requiring a message broker.

- **Spec:** [`rfcs/0000-pmp-spool.md`](rfcs/0000-pmp-spool.md) — PMP RFC 0,
  defining the envelope format and the Spool transport profile
  (`PMP/Spool 0.0`).
- **Schema:** [`schemas/pmp-spool-0.0.schema.json`](schemas/pmp-spool-0.0.schema.json)
  — JSON Schema for the envelope, for cross-language conformance checks.
- **Example:** [`examples/build-completed.pmp`](examples/build-completed.pmp)
  — a sample message.

Pydantic is the modeling and validation library used by the Python
reference implementation. It is **not** part of the protocol itself —
consumers written in other languages don't need it, and don't need to
know or care what the producer used to build the JSON.

## Implementations

- Python: [`pmp-python`](https://github.com/pydantic-message-protocol/pmp-python)

## Status

Experimental. `PMP/Spool 0.0` is the only defined profile; future RFCs
may add others (`PMP/HTTP`, `PMP/UnixSocket`, `PMP/TCP`).

## License

Apache 2.0 — see [`LICENSE`](LICENSE).
