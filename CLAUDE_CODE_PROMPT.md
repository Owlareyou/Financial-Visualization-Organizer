# Project Brief - Financial Banking Organizer

> Project stage: discovery, final step (technical plan). This brief is authoritative together with the approved decisions in `docs/decisions/`.
> Start by reading this file, `docs/plans/00-discovery.md`, and `docs/decisions/`.

## 0. Working Agreement

- Before implementation, propose a short file-by-file plan and wait for approval.
- Work on one scoped task at a time; summarize what changed and stop for review.
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

The first-release brief is approved (`docs/plans/02-first-release-brief.md`). Write a technical plan for approval. Do not scaffold the application before that review.

## 3. Decided

See `docs/decisions/` (0001–0007):

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

## 4. Not Approved Yet

- Technology stack: language, web framework, database engine, charting library, AI provider and model
- Hosting, and how the public graph page is served
- Authentication and multi-user support (deferred; keep the design open to them)
- Release process, testing approach and repository layout

Do not add dependencies until the technical plan is approved.

## 5. Discovery Exit Criteria

| # | Criterion | Status |
|---|---|---|
| 1 | Target user and primary workflow | Approved (`00-discovery.md`) |
| 2 | First-release scope and explicit non-goals | Approved (`02-first-release-brief.md`) |
| 3 | Data sources and treatment of sensitive information | Approved (0001, 0004, 0005) |
| 4 | Platform and measurable acceptance criteria | Approved (0006; brief section 6) |
| 5 | Technical direction and a small, reviewable implementation plan | Not started |

Record durable product or technical rulings in `docs/decisions/`. Keep research findings and sources in `docs/research/`.
