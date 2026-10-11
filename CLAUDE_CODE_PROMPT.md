# Project Brief - Financial Banking Organizer

> Project stage: implementation of the first release. This brief is authoritative together with the approved decisions in `docs/decisions/`.
> Start by reading this file, `docs/plans/PROGRESS.md`, and `docs/plans/03-technical-plan.md`.

## 0. Working Agreement

- Implementation follows the approved technical plan (`docs/plans/03-technical-plan.md`). Each later milestone's detailed tasks are approved at the checkpoint before it.
- During an autonomous run, complete tasks one after another and stop only at checkpoints or stop rules (decision 0009). Outside a run, work on one scoped task at a time and stop for review.
- Ask when requirements are ambiguous. Do not silently choose product or technical behavior.
- Keep scope to the approved brief. Record proposed additions before building them.
- This is a learning project: explain new server, database and tooling ideas as they are introduced.
- Real statements and extracted data live only in the gitignored data folder. Use synthetic data in anything committed (fixtures, tests, examples). Never commit statements, databases, account numbers, passwords or API keys.
- Never ask for or store bank login credentials. Statement PDF passwords go in the macOS Keychain only.
- Do not connect to banks or hold or move money.
- Statement data may be sent to an online AI service only to read statements, guess categories and write summaries (decisions 0001 and 0005). Track every call and never re-read a file that was already read. Any other external service needs approval.
- Written summaries describe spending; they do not recommend financial actions (decision 0005).
- Keep comments focused on non-obvious reasons, not narration.

## 1. Context

A personal organizer for the owner's bank statements. Six bank accounts (five in Taiwan and Bank of America) send statements as password-protected PDFs, PDFs, a .txt file and screenshots. The app extracts every transaction into a local database, categorizes it, keeps uncertain items for review, finds subscriptions, and shows monthly income and spending charts and written summaries. TWD is the main currency. The full picture is in `docs/plans/00-discovery.md`.

## 2. Current Goal

Build the first release milestone by milestone (M0–M8 in `docs/plans/03-technical-plan.md`), tracking state in `docs/plans/PROGRESS.md`.

## 3. Decided

See `docs/decisions/` (0001–0009):

- intake formats and build order
- online AI reading
- review queues
- duplicate and transfer detection
- categories and tags
- local storage in a gitignored folder
- boundaries and advice
- platform and future users
- learning goal and budget
- charts, currency and subscriptions
- technology stack (0008)
- autonomous runs between checkpoints (0009)

## 4. Not Approved Yet

- Hosting, and how the public graph page is served (planned for M8)
- Authentication and multi-user support (deferred; keep the design open to them)
- Any Python dependency not listed in decision 0008

## 5. Discovery Exit Criteria

| # | Criterion | Status |
|---|---|---|
| 1 | Target user and primary workflow | Approved (`00-discovery.md`) |
| 2 | First-release scope and explicit non-goals | Approved (`02-first-release-brief.md`) |
| 3 | Data sources and treatment of sensitive information | Approved (0001, 0004, 0005) |
| 4 | Platform and measurable acceptance criteria | Approved (0006; brief section 6) |
| 5 | Technical direction and a small, reviewable implementation plan | Approved (0008, 0009, `03-technical-plan.md`) |

Record durable product or technical rulings in `docs/decisions/`. Keep research findings and sources in `docs/research/`.
