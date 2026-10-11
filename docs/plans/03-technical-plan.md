# Technical Plan

Status: **approved** (2026-10-11); review answers are in section 4. Builds on the approved brief (`02-first-release-brief.md`) and decisions 0001–0007. The stack is decision 0008 and the autonomy rules are decision 0009.

The plan has three parts:

1. **Stack and layout:** the tools, and why each one teaches something.
2. **Autonomous run protocol:** how an AI agent works through the tasks on its own and stops at checkpoints.
3. **Task list:** detailed for M0–M1, outlined for M2–M8. Each later milestone is detailed at the checkpoint before it.

---

## 1. Stack

| Part | Choice | Why |
|---|---|---|
| Language | Python 3.12 (already installed) | Best libraries for PDFs, the Keychain and the Claude SDK; easy to read while learning |
| Project tool | `uv` | One tool for the Python version, dependencies and running commands |
| Database | **PostgreSQL 17**, a real database server, installed with Homebrew | Teaches what a database server is: it runs in the background, you connect to it, and it handles users and permissions. It's the same engine you'd use when deploying for other users. SQLite (one file, no server) is simpler but teaches less, and it would need replacing for multi-user use |
| Database access | `psycopg` 3 with plain SQL files, and no ORM (a library that writes SQL for you) | You learn SQL itself; every table and query is visible |
| Schema changes | Numbered `.sql` migration files plus a ~40-line runner | Shows how databases change safely over time without hiding it behind a framework |
| Web server | `FastAPI` + `uvicorn` | A small, explicit backend server: you see each URL, request and JSON response |
| Web page | Plain HTML and JavaScript served by FastAPI, with a downloaded copy of **Chart.js** | No frontend build step to learn yet; the page just asks the server for JSON and draws it |
| Commands | `typer` | `fbo import`, `fbo serve` and so on, the "command now, button later" from the brief. The button calls the same function |
| Secrets | `keyring` (macOS Keychain) | PDF passwords and the API key, read automatically (0001) |
| PDFs | `pikepdf` (unlock) + `pdfplumber` (read text and tables) | Reads real-text PDFs exactly, with no AI cost |
| AI | Anthropic Python SDK, model `claude-opus-5-5`, structured outputs | Reads screenshots, guesses categories, finds subscriptions, writes summaries. Responses are checked against a schema |
| Tests and lint | `pytest`, `ruff` | Every task has automated checks, which is what makes an autonomous run safe |
| Test PDFs | `reportlab` (dev only) | Generates fake statements that copy a bank's layout with made-up numbers, so tests never need real data |

Money is stored as `NUMERIC(14,2)` in the database and as `Decimal` in Python, never as floating-point numbers, which can introduce rounding errors.

Every table that holds owner data has a `user_id` column, and there is one default user for now. This keeps the door open for other users later (0006) at almost no cost.

### AI cost estimate

Prices are for `claude-opus-5-5` at $4 per million input tokens and $20 per million output tokens. A token is roughly a word or a short piece of one.

The text-based PDFs (five banks) and the .txt file are read **without AI**. AI is used only for:

| Use | Rough monthly amount | Rough cost |
|---|---|---|
| 中華郵政 screenshots (a few images) | ~10k input / ~5k output | ~US$0.15 |
| Categorizing new merchants (corrections are remembered, so this shrinks over time) | ~20k / ~10k | ~US$0.30 |
| Subscription check and written summaries | ~15k / ~5k | ~US$0.15 |
| **Total** | | **~US$0.60 ≈ 20 TWD** |

That's well under the 100 TWD budget. These are estimates; the real numbers will come from the read log (acceptance criterion 9). Loading past months the first time is a one-off cost on top.

Two related settings:
- **Model:** the model is a setting. You could switch to a cheaper one later, but that's your call; the plan doesn't assume it.
- **Automatic fallback:** requests use the API's server-side fallback, so if the main model declines a request, another model handles it.

### Repository layout

```
src/fbo/
  cli.py              commands: import, serve, migrate, review …
  config.py           settings (exchange rate, review threshold, top-N …)
  secrets.py          Keychain access
  db/
    connect.py
    migrate.py
    migrations/       0001_init.sql, 0002_…
  intake/
    service.py        import_inbox(): the one function the command and the button both call
  readers/
    base.py           the shared transaction shape every reader returns
    ctbc.py, taishin.py, hsbc.py, cathay.py, boa_txt.py, post_screenshot.py
  ai/                 from M3: client, prompts, call log
  analysis/           from M2: duplicates, transfers, categories, subscriptions
  web/
    app.py            FastAPI routes
    static/           index.html, app.js, chart.umd.js
tests/
  fixtures/synthetic/ fake statements only
scripts/hooks/pre-commit   privacy check
docs/learn/           short lessons written as each new idea appears
data/                 gitignored: inbox/, originals/, exports/
```

---

## 2. Autonomous Run Protocol

This changes the working agreement. Today it says to stop after every task. Under this protocol, **between checkpoints the agent completes tasks one after another without stopping**, and it stops only at a checkpoint or when a stop rule triggers.

### Source of truth

`docs/plans/PROGRESS.md` (created in M0) lists every task with its status (`todo`, `doing`, `done`, `blocked`), the commit it landed in, and short notes. Each loop iteration starts by reading it, so a fresh or restarted session can pick up exactly where the last one left off.

### One iteration

1. **Orient:** read `CLAUDE_CODE_PROMPT.md`, `PROGRESS.md`, and this plan's entry for the next `todo` task.
2. **Check the task type:**
   - **CHECKPOINT task:** write the checkpoint report (see below) and stop the loop.
   - **OWNER task** (something only you can do): list it in `PROGRESS.md` under "Waiting on owner" and stop.
3. **Build:** implement only that task, including its tests.
4. **Verify:** run every command in the task's **Done when** line, plus `uv run ruff check` and `uv run pytest`.
5. **If verification fails:**
   - fix it and run verification again, up to **3 attempts**
   - if it still fails, mark the task `blocked` with the error output and stop
6. **Record:**
   - commit on the milestone branch (`m1-ctbc`, etc.) with the message `T1.4: <summary>`
   - mark the task `done` in `PROGRESS.md` with the commit hash
   - add or update a lesson in `docs/learn/` if the task introduced a new idea
7. **Continue** with the next task.

### Stop rules (stop immediately and explain in `PROGRESS.md`)

- A requirement is ambiguous, or the task would need a product decision.
- The task needs a Python dependency not on the approved list in section 1. (System installs with Homebrew are allowed; record each one in `PROGRESS.md`.)
- A test can only pass by weakening or deleting it.
- AI spend in this run would exceed **US$0.50** (tracked in the AI call log from M3; reading statements during development counts too).
- Anything would touch `docs/decisions/`, `main`, or the remote repository.

### Never, during an autonomous run

- Push, merge to `main`, or rewrite git history.
- Commit anything from `data/`, any real statement, password or key. The pre-commit hook enforces this.
- Read files in `data/` except in tasks marked **reads real data**. Those tasks may print layout notes but never copy real values into the repo.
- Change Claude Code settings, or create, read out or change Keychain entries. The app reads secrets through `keyring` at runtime; the agent never prints them.

### Checkpoints

Each milestone ends with a CHECKPOINT task. The agent writes a short report in `PROGRESS.md`:
- what was built
- how to try it (exact commands)
- what to check by hand against a real statement
- costs so far
- which new lessons were added
- the detailed task list it proposes for the next milestone

You review, merge the branch and push. The next milestone's tasks are approved before the loop restarts.

### How to run it

1. In Claude Code, inside this repo, start a self-paced loop: `/loop Continue the plan: follow the autonomous run protocol in docs/plans/03-technical-plan.md section 2.`
2. The loop works through tasks and stops itself at the next checkpoint or stop rule: it writes the reason in `PROGRESS.md` and ends the loop.
3. Permissions for the run are in `.claude/settings.local.json` (added 2026-10-11): uv, the PostgreSQL command-line tools, Homebrew, git, and editing source, tests, scripts and docs. Pushing, deleting files, and editing `docs/decisions/` or `data/` still ask first.

---

## 3. Tasks

**Done when** lists the checks that must pass. Tags:
- **OWNER:** you do it.
- **CHECKPOINT:** the run stops for your review.
- **reads real data:** the task may look at files in `data/`, following the rules above.

### M0 — Setup and foundations

| ID | Task | Done when |
|---|---|---|
| T0.1 | Install the tools: `brew install uv postgresql@17`, `brew services start postgresql@17`, then create the databases `fbo` and `fbo_test` | `uv --version` works; `psql fbo -c 'select 1'` and `psql fbo_test -c 'select 1'` work |
| T0.2 | Write `docs/learn/00-setup.md`. It explains what Homebrew, a database server, a background service, the Keychain and `uv` are, shows every command T0.1 ran, and gives the commands **you** run before M1 (storing the API key and the CTBC password in the Keychain, and where to put statements) | The file exists and every command in it is copy-pasteable |
| T0.3 | Project skeleton: `pyproject.toml` (uv), the `src/fbo` package, ruff and pytest settings, `data/` folders, `.gitignore` entries for `data/` | `uv run pytest` passes (an empty test); `git check-ignore data/inbox/x.pdf` matches |
| T0.4 | Privacy pre-commit hook. It blocks `data/`, `*.pdf`/`*.png`/`*.jpg`/`*.txt` outside `tests/fixtures/synthetic/`, database dumps, and lines that look like API keys. Install it with `fbo hooks install` | A test commits a fake `data/x.pdf` in a temporary repo and is rejected |
| T0.5 | `PROGRESS.md` with every M0–M1 task *(done at plan approval)* | The file lists T0.1–T1.10 |
| T0.6 | Database connection and migration runner (`fbo migrate`), with a `schema_migrations` table | Migrating `fbo_test` twice is a no-op the second time (tested) |
| T0.7 | Migration `0001_init.sql`: `users`, `accounts` (bank, currency, kind), `source_files` (sha256, bank, status, imported_at), `transactions` (user_id, account_id, source_file_id, posted_on, description, amount, currency, direction) | Tests insert and read a transaction in `fbo_test` |
| T0.8 | `secrets.py`: `get_secret(name)` reads from the Keychain, with a clear error if the secret is missing | A test with a fake keyring backend passes |
| T0.9 | Lesson `docs/learn/01-database-basics.md`: tables, rows, SQL, migrations, using this project's own schema | The file exists |
| T0.10 | **CHECKPOINT** M0. Before M1 starts, you store the API key and the CTBC password in the Keychain and put 1–2 real CTBC statements in `data/inbox/`, following `docs/learn/00-setup.md` | |

### M1 — CTBC from statement to chart

| ID | Task | Done when |
|---|---|---|
| T1.1 | Unlock a PDF with the Keychain password and extract its text and tables (`readers/pdf.py`) | Tests pass on a synthetic password-protected PDF |
| T1.2 | **Reads real data:** look at the layout of the real CTBC statement and record it in `docs/learn/banks/ctbc-layout.md`. That covers the column names, date format, totals lines, and how page breaks look, **with no real values** | The file exists, and a grep finds no real amounts or account numbers in it |
| T1.3 | Generate a synthetic CTBC statement (with reportlab) that copies that layout, plus the expected parse result as JSON | The fixture and expected JSON are in `tests/fixtures/synthetic/ctbc/` |
| T1.4 | CTBC reader: produces transactions in the shared shape and checks that they add up to the statement's own totals; if they don't, the reader fails loudly | Tests: the parse matches the expected JSON, and a fixture with one row removed fails the total check |
| T1.5 | `import_inbox()`: hash each file, skip files already imported, choose a reader by bank, store everything in one database transaction, move the original to `data/originals/` | Tests: importing the same fixture twice adds rows only once; a file that fails leaves no partial rows |
| T1.6 | `fbo import` command that calls `import_inbox()` and prints how many files it read and how many rows it added | A CLI test on the fixture passes |
| T1.7 | API route `GET /api/monthly` returns `[{month, income, spending}]` | A test with known fixture rows checks the exact numbers |
| T1.8 | Web page: side-by-side monthly bars from `/api/monthly`; `fbo serve` starts it on `localhost:8000` | A test fetches the page; `/api/monthly` returns 200. A manual check is listed in the checkpoint report |
| T1.9 | Lessons: `02-what-is-a-server.md` (requests, ports, JSON, using `/api/monthly` as the example) and `03-reading-pdfs.md` | The files exist |
| T1.10 | **CHECKPOINT** M1: you run `fbo import` on the real CTBC statements and `fbo serve`, then compare one month's totals with the statement | |

### M2–M8 — outline (detailed at each checkpoint)

- **M2 Trust:**
  - `transactions_review` table and `fbo review`
  - confidence per row
  - duplicate check (same account, date, amount and description)
  - transfers between your own accounts (matching amounts on both sides within a few days)
  - AI call log table
  - criteria 2, 3 and 4
- **M3 Categories:**
  - categories and `transaction_tags` (role: main or filter; designed for more roles later)
  - the first AI step: structured-output category guesses
  - `category_rules` learned from your corrections
  - stacked-bar toggle, category checklist, top-5 list and a settings page
  - criteria 5 and 6
- **M4 More banks:**
  - 台新, 匯豐, 國泰世華 readers (each with a "reads real data" layout task and a synthetic fixture)
  - Bank of America .txt reader
  - fixed exchange-rate setting
- **M5 Subscriptions:**
  - recurring-charge detection with confidence
  - a review area
  - the list of common services for spotting yearly subscriptions
  - the summary page
  - criterion 7
- **M6 Written summaries:**
  - the monthly summary through AI
  - a prompt and an automated check that it only describes spending (0005)
  - criterion 8
- **M7 Screenshots:**
  - the 中華郵政 reader through AI vision
  - rows whose totals don't match go to review
  - criterion 1 for screenshots
- **M8 Public view:**
  - a read-only public page with an on/off switch
  - a lesson on what "exposed to the internet" means
  - criterion 11 rechecked; final acceptance run of all 11 criteria

---

## 4. Review Answers (2026-10-11)

1. **Database:** PostgreSQL.
2. **Autonomy rules:** approved as written in section 2.
3. **Installs:** the agent may install anything it needs with Homebrew. Secrets and real statements stay with you.
4. **Permissions:** added to `.claude/settings.local.json`.
