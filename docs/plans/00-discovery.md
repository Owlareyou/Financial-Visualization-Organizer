# Product Discovery

Status: in progress. Answers below are the owner's initial context (2026-10-03). They become decisions only once recorded in `docs/decisions/`. Follow-up questions live in `01-discovery-followups.md`.

## Origin

Banking statements arrive by email from several banks. The goal is one place that automatically shows a clear view of spending. Statements (as screenshots) go in; the app extracts every transaction into a database that can then be queried and visualized freely. This is a cute, personal-use project.

## Product

- **Who is the primary user?**
  The owner, as the sole user for now.
- **What financial-organizing problem should the first release solve?**
  Seeing clearly how much money is earned and spent each month, across all banks, in one place.
- **What is the single most important workflow?**
  Make statements readable (the only manual step) → upload → transactions are extracted, stored and categorized automatically → view monthly charts.
- **What should the product explicitly not do?**
  Open. See follow-ups.

## Data and Trust

- **Which account types and financial records are in scope?**
  Banking statements received by email, from multiple banks. Exact account types are open.
- **How will information enter the product?**
  File import: screenshots of statements, uploaded through a web page. Making the statements readable should be the *only* manual step; everything after that is automatic.
  Note: the owner is open to starting from a local folder of screenshots and wrapping it in a website later, as long as the design doesn't lock the project into local-folder-only intake.
- **What data must be stored, for how long, and where?**
  Initial idea: a database running on the owner's own computer. Retention is open.
- **What are the requirements for privacy, security, export, backup, and deletion?**
  Open.
- **Are any external services allowed to process user data?**
  Open. This matters for how screenshots are read (local text recognition vs. a cloud AI service).

## Features

- **First release**
  - Tag each transaction with a spending category.
  - Bar chart of income and spending per month.
  - Category checklist that filters the chart interactively.
  - Keep visualization simple at first.
- **Subscriptions (added 2026-10-11; release timing open)**
  - Automatically detect recurring payments (subscriptions) from the transactions.
  - Summarize them in one place. What the summary shows is open; see follow-ups section 7.
- **Future (to be discussed)**
  - Budgets.
  - Packaging as a deployable website.

## Experience and Delivery

- **Which regions, languages, currencies, and platforms are required?**
  Languages: English; possibly Traditional Mandarin. Platform: web page, with the database on the owner's computer at first and possible deployment later. Regions and currencies are open.
- **What accessibility requirements apply?**
  None for now.
- **What acceptance criteria demonstrate that the first release is useful?**
  The owner can clearly see how much money they spend in a month.
- **What constraints apply to budget, schedule, hosting, or maintenance?**
  Open. The owner would like a learning session on servers and running databases if the local-database approach needs explaining.

## Next Gate

After these questions are answered, write an approved first-release brief and a small technical plan. Do not begin application scaffolding before that review.
