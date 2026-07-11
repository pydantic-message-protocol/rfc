# PMP RFC 0 — PMP Spool Profile

Status: Experimental
Protocol: PMP/Spool 0.0

## 1. Overview

PMP is the **Protocol for Message Passing** — a small, filesystem-first
protocol for exchanging asynchronous, strongly-typed messages between
applications and repositories, without requiring a message broker.

PMP has four layers:

```
Application
    |
Object model        (implementation-specific, e.g. Pydantic in Python)
    |
PMP Envelope         (JSON)
    |
Transport profile    (this document defines the Spool profile)
```

Pydantic is the reference modeling and validation implementation in
Python. Pydantic is **not** part of the protocol and is not required for
implementations written in other languages. The consumer does not need
to know or care how the producer constructed its JSON.

## 2. Terminology

The key words MUST, MUST NOT, SHOULD, and MAY are to be interpreted as
in RFC 2119.

## 3. The PMP Envelope

Every message is a single, complete JSON document.

```json
{
  "pmp": "0.0",
  "profile": "spool",
  "message_type": "build.completed",
  "schema_version": 1,
  "created_at": "2026-07-10T18:42:31.123Z",
  "producer": "repository-a",
  "sequence": "2a",
  "payload": {}
}
```

- `created_at` MUST be a UTC RFC 3339 timestamp.
- `sequence` MUST match the sequence component of the message's
  filename (see §4.2). The filename and envelope MUST agree — this
  makes copied, renamed, or malformed messages detectable.
- `message_type` MUST be a stable dotted identifier matching the
  grammar `[a-z][a-z0-9]*(\.[a-z][a-z0-9_-]*)+` (e.g.
  `build.completed`, `repository.sync.requested`). Python import paths
  MUST NOT be used as message types.
- Application-specific payload schemas are outside PMP core. Producers
  and consumers MUST independently agree on the message type and
  payload schema they exchange.

## 4. The Spool Profile

The Spool profile defines how PMP envelopes are exchanged on a local
Unix-style filesystem. The envelope itself is transport-agnostic; other
profiles (PMP/HTTP, PMP/UnixSocket, PMP/TCP) may carry the same JSON
document over other transports in future RFCs.

### 4.1 Directory layout

```
spool/
├── tmp/          incomplete or unpublished messages
├── pending/      published and available
├── processing/   atomically claimed by a consumer
├── completed/    successfully consumed
└── failed/       processing failed permanently or administratively
```

A deployment MAY delete messages instead of retaining them in
`completed/`, but an implementation MUST support the five directories
above.

### 4.2 Filename format

```
{sequence_hex}-{epoch_ms_hex}-{pid_hex}.pmp
```

Example: `2a-197f50a74c8-9c4.pmp`

- `sequence_hex` is lowercase hexadecimal, no `0x` prefix (e.g. `2a`,
  not `0x2a`). It is the authoritative spool-local identifier.
  Readers MUST parse it numerically — lexical filename sorting is not
  a valid ordering.
- `epoch_ms_hex` and `pid_hex` provide provenance and collision
  resistance. They do not replace correct sequence allocation.
- A producer MUST NOT overwrite an existing destination file. A
  collision MUST fail loudly.

### 4.3 Sequence allocation

PMP Spool 0.0 permits multiple local producers. Sequence allocation
MUST be serialized using an advisory lock around a shared sequence
file: `spool/.sequence`.

A writer locks the file, reads the current hex value, increments it,
persists it, then unlocks it. Sequences start at `0`.

### 4.4 Publication (durability)

To publish a message, a producer MUST:

1. Write the complete message to a temporary file in `tmp/`.
2. Flush and `fsync()` the file.
3. Atomically rename the file into `pending/`.
4. `fsync()` the destination directory.

This distinguishes **atomic** (no reader ever observes a partial file)
from **durable** (the message survives a crash after publication).

### 4.5 Claiming

A consumer claims a message by atomically renaming it from `pending/`
into `processing/`.

### 4.6 Completion and failure

A message is successfully consumed only when the registered handler
returns without error.

- On success: `processing/` → `completed/`
- On failure: `processing/` → `failed/`

Automatic retries are deferred to a later RFC; this keeps v0.0
deterministic.

### 4.7 Crash recovery

PMP Spool 0.0 does not automatically infer that a message left in
`processing/` is abandoned. An operator or implementation-specific
recovery process MAY move it back to `pending/`. Using file
modification time as an automatic lease is deliberately deferred — it
introduces clock, timeout, and duplicate-processing semantics too
early for v0.0.

### 4.8 Filesystem boundary

`tmp/`, `pending/`, and `processing/` MUST reside on the same
filesystem — atomic rename is only guaranteed within one filesystem.
Remote transfer agents MUST upload into the destination's `tmp/` and
then perform the final rename locally at the destination.

## 5. Compatibility

An implementation claiming conformance with PMP Spool 0.0 MUST be able
to publish and consume messages using the directory layout, filename
format, JSON envelope, ordering rules, and atomic filesystem operations
defined above.

Pydantic is the reference modeling and validation implementation.
Pydantic is not required for implementations written in other
programming languages.

## 6. Non-goals for RFC 0

- Application-specific payload schemas.
- Transport profiles other than Spool (candidates for future RFCs:
  PMP/HTTP, PMP/UnixSocket, PMP/TCP).
- Automatic retry and lease-based crash recovery.

## 7. Versioning

Three version references are distinct and MUST NOT be conflated:

- **PMP RFC 0** — this specification document.
- **PMP/Spool 0.0** — the protocol profile and version.
- **pmp 0.0.1** — a Python package release implementing this RFC.
