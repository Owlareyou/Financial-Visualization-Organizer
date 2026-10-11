# 0008 — Technology stack

- **Status:** accepted
- **Date:** 2026-10-11
- **Source:** `docs/plans/03-technical-plan.md` sections 1 and 4

## Decision

- **Language and tooling:** Python 3.12, managed with `uv`.
- **Database:** PostgreSQL 17, a local database server, accessed with `psycopg` 3 and plain SQL. Schema changes are numbered `.sql` migration files. There is no ORM.
- **Web:** a FastAPI server with plain HTML and JavaScript and a downloaded copy of Chart.js. There is no frontend build step.
- **Commands:** `typer`. Import runs through one function that both the command and a later button call.
- **Statements:** `pikepdf` unlocks PDFs and `pdfplumber` reads them; secrets come from the macOS Keychain through `keyring`.
- **AI:** the Anthropic Python SDK with `claude-opus-5-5` and structured outputs. The model is a setting.
- **Quality checks:** `pytest` and `ruff`. Fake test statements are generated with `reportlab`.
- **Data model:** money is stored as `NUMERIC(14,2)` and handled as `Decimal`. Every table holding owner data has a `user_id` column.

## Alternatives considered

- **SQLite:** simpler (one file, no server), but teaches less and would need replacing for multi-user deployment.
- **An ORM:** less SQL to write, but it hides exactly what the owner wants to learn.
- **A frontend framework (React and similar):** a heavier setup with no need at this size.

## Consequences

- PostgreSQL must be running for the app and the tests to work.
- Approved Python packages are the ones listed above. Adding another one needs approval (see 0009).
