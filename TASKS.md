# TASKS.md

## Active

## Up Next
- Confirm license (Apache 2.0 recommended — see DECISIONS.md OPEN) and add LICENSE file
- Pick Python implementation repo name (`pmp-python` vs `python-pmp` — see DECISIONS.md OPEN), create it under github.com/pydantic-message-protocol
- Implement Python reference package (`pmp`): `envelope.py`, `producer.py`, `consumer.py`, `spool.py`, `naming.py`, `exceptions.py` per rfcs/0000-pmp-spool.md

## Backlog
- README.md, CONTRIBUTING.md, CHANGELOG.md for this repo
- rfcs/template.md for future RFC authoring
- conformance/ test fixtures (valid/ and invalid/ example messages)
- Future transport profiles: PMP/HTTP, PMP/UnixSocket, PMP/TCP
