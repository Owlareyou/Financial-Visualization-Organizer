# Progress

The source of truth for the autonomous run. The protocol is in `03-technical-plan.md` section 2. Each loop iteration reads this file first, works on the first `todo` task in order, and updates this file before it moves on.

- **Current milestone:** M0 — Setup and foundations
- **Branch:** `m0-setup`. Create it from `main` if it doesn't exist. Never commit to `main` during a run.
- **AI spend this run:** US$0.00 of the US$0.50 cap

## Waiting on owner

Nothing yet.

## Tasks

| ID | Task | Status | Commit | Notes |
|---|---|---|---|---|
| T0.1 | Install uv and PostgreSQL 17, start the service, create `fbo` and `fbo_test` | todo | | |
| T0.2 | Lesson `docs/learn/00-setup.md` | todo | | |
| T0.3 | Project skeleton (uv, `src/fbo`, ruff, pytest, `data/` gitignored) | todo | | |
| T0.4 | Privacy pre-commit hook and `fbo hooks install` | todo | | |
| T0.5 | This file | done | (plan approval) | |
| T0.6 | Database connection and migration runner | todo | | |
| T0.7 | Migration `0001_init.sql` | todo | | |
| T0.8 | `secrets.py` Keychain access | todo | | |
| T0.9 | Lesson `01-database-basics.md` | todo | | |
| T0.10 | **CHECKPOINT** M0 | todo | | |
| T1.1 | Unlock PDFs and extract text and tables | todo | | |
| T1.2 | **Reads real data:** CTBC layout notes | todo | | |
| T1.3 | Synthetic CTBC fixture | todo | | |
| T1.4 | CTBC reader with totals check | todo | | |
| T1.5 | `import_inbox()` | todo | | |
| T1.6 | `fbo import` command | todo | | |
| T1.7 | `GET /api/monthly` | todo | | |
| T1.8 | Web page with monthly bars; `fbo serve` | todo | | |
| T1.9 | Lessons `02-what-is-a-server.md` and `03-reading-pdfs.md` | todo | | |
| T1.10 | **CHECKPOINT** M1 | todo | | |

## Installs

Each system install made during a run, with its command.

## Checkpoint reports

None yet.
