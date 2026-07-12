# TASKS.md

## Active

## Up Next
- Implement Python reference package (`pmp`): `envelope.py`, `producer.py`, `consumer.py`, `spool.py`, `naming.py`, `exceptions.py` per rfcs/0000-pmp-spool.md (repo already created: github.com/pydantic-message-protocol/pmp-python)
- Add `.github/workflows/ci.yml` to pmp-python: build + run tests on push/PR. No publish step for now.
- Manually upload `assets/org-icon.png` as the org avatar via github.com/organizations/pydantic-message-protocol/settings/profile — no API/gh path exists for this, must be done through the web UI.

## Backlog
- README.md, CONTRIBUTING.md, CHANGELOG.md for this repo
- rfcs/template.md for future RFC authoring
- conformance/ test fixtures (valid/ and invalid/ example messages)
- Future transport profiles: PMP/HTTP, PMP/UnixSocket, PMP/TCP
