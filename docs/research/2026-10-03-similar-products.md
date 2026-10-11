# Similar Products Scan

Date: 2026-10-03
Question: Are there existing products that automatically turn bank statements (especially screenshots) into a categorized spending view, like this project?

## Short answer

Parts of the idea exist, but no product found does the whole thing the way the owner described: **screenshots from several banks → the owner's own database → custom monthly charts with category filters.** Existing products fall into four groups, and each one gives up something the owner wants.

## Findings (observed facts)

### 1. Apps that read screenshots or PDFs with AI (cloud)

| Product | What it reads | What you get | Trade-off |
|---|---|---|---|
| [Finny](https://getfinny.app/blog/best-expense-trackers-import-credit-card-transactions) | Screenshots of transactions from your banking app, up to 5 at once | AI extracts merchant, amount and date, and assigns categories on import | iPhone only; subscription with a daily AI request cap; the article doesn't say where data is stored |
| [Skwad](https://skwad.app/bank-statements) | PDFs (digital and scanned), images, screenshots; multiple languages and currencies | Extracted transactions, categories, export to Excel/CSV or Google Sheets | Paid after free trial; files processed on AWS in the USA, uploads deleted after 24 hours |
| [Sortlumo](https://sortlumo.com/) | Statement PDFs | Transactions sorted into 22 categories | PDFs only; iPhone app still in testing. *Needs verification: details came from a search summary, not the site itself.* |
| [MyBankStatementAnalysis](https://mybankstatementanalysis.com/transaction-categorization) | Statement PDFs | 19 categories, CSV/Excel export; one free upload | A converter, not an ongoing tracker. *Needs verification.* |

### 2. Self-hosted, open-source finance apps (runs on your own computer)

| Product | What it reads | Categorizing | Trade-off |
|---|---|---|---|
| [Actual Budget](https://actualbudget.org/docs/transactions/importing) | CSV, QIF, OFX, QFX, CAMT files | Rules; automatic duplicate detection | Cannot read screenshots or PDFs; built around envelope budgeting |
| [Firefly III](https://beancount.io/blog/2026/07/26/firefly-iii-vs-actual-budget-self-hosted-open-source-budgeting-guide) | CSV, CAMT, OFX, QIF; some bank connections (mostly EU/UK services) | Strong rules engine ("description contains X → category Y") | Cannot read screenshots; heavier to run (PHP app plus a database); full double-entry accounting is more than this project needs |
| [Fin](https://github.com/bharat2288/fin) (hobby project, MIT license) | CSV and PDF statements from three Singapore banks | Pattern rules; preview and correct before saving | Local only, no login. Very close to this project's shape: Python + Flask + SQLite + Chart.js, with multi-file upload and spending dashboards |

### 3. Big subscription budgeting apps (US-focused)

[Monarch Money, Copilot Money, YNAB, PocketGuard](https://getfinny.app/blog/best-expense-trackers-import-credit-card-transactions) connect directly to bank logins and categorize with AI. CSV import exists; [importing statement PDFs is still a user feature request in Copilot](https://roadmap.copilot.money/feature-requests/p/import-transactions-via-csv-or-xls), not a shipped feature. They mainly support US banks.

### 4. Taiwan

[麻布記帳 Moneybook](https://apps.apple.com/tw/app/moneybook%E9%BA%BB%E5%B8%83%E8%A8%98%E5%B8%B3-%E8%87%AA%E5%8B%95%E8%A8%98%E5%B8%B3%E8%88%87%E8%B3%87%E7%94%A2%E7%AE%A1%E7%90%86/id1327712235) syncs credit card transactions from 34 Taiwanese banks, plus e-invoices, EasyCard, LINE Pay and JKOPAY, and categorizes them automatically. It is a registered Open Banking service provider in Taiwan. No statement upload is needed, but it connects to your bank accounts and your data lives on its servers.

## What this means (recommendations, not decisions)

- **If the banks are in Taiwan**, Moneybook may already do most of the job, and it's worth trying for a week to see what you'd still miss. The reason to build your own would be owning the data and designing your own charts.
- **The hard, unsolved part is reading statements reliably.** Every product that reads screenshots or PDFs does it with cloud AI. That is the key privacy and cost decision (follow-up question 6).
- **Fin is a useful reference design.** It's a working example of almost exactly the first release described in discovery. We could study how it's structured without copying it.
- **A hybrid is possible:** build only the "statement → clean CSV" reader, then load the CSVs into Actual Budget or Firefly III for storage and charts. Less to build, but their charts and categories are less customizable, and Firefly's bank connections mostly don't cover Asia.

## Open items to verify

- Sortlumo and MyBankStatementAnalysis details came from search summaries; check their sites before relying on them.
- Finny's pricing and where it stores data were not stated on the page reviewed.
- Whether Moneybook covers the owner's specific banks depends on follow-up question 3.
