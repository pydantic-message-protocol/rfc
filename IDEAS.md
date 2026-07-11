# IDEAS.md

Scratch space. Everything lands here first. Promote to `TASKS.md` or `DECISIONS.md` when ready.

- Org layout: `github.com/pydantic-message-protocol/{rfc, pmp-python, pmp-go, pmp-rust, pmp-http, examples}` — one repo per implementation/transport profile, org name signals PMP is the protocol.
- `example-project-messages`-style shared payload packages: projects that want a shared message contract can publish their own package depending on `pmp`, without `pmp` depending on them.
- Version layering to keep straight: PMP RFC 0 (spec) vs PMP/Spool 0.0 (protocol profile+version) vs `pmp` 0.0.1 (Python package release).
