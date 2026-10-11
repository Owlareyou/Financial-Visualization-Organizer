# Product Discovery

Status: complete (2026-10-11). Owner answers are in `01-discovery-followups.md`; settled rulings are in `docs/decisions/`. Next step: the first-release brief.

## Origin

Banking statements arrive by email from several banks. The goal is one place that automatically shows a clear view of spending. Statements go in; the app extracts every transaction into a database that can then be queried and visualized freely. This is a personal project, built quick and fail fast, and it is a learning opportunity for the owner.

## Product

- **Primary user:** the owner, as the sole user for now. The design should allow other people to sign up later (see `docs/decisions/0006-platform-and-future.md`).
- **Problem:** seeing clearly how much money is earned and spent each month, across all banks, in one place.
- **Core workflow:** statement files go in → transactions are extracted, stored and categorized automatically → uncertain items wait for review → monthly charts and summaries.
- **Never:** connect to bank logins, or hold or move money.

## Data and Trust

- **Banks (all are bank accounts; there are no credit cards yet):**

  | Bank | Role | Statement format | Currency |
  |---|---|---|---|
  | CTBC | Main salary account; most LINE Pay spending | Password-protected PDF | TWD |
  | 台新 | Old salary account; some use | Password-protected PDF | TWD |
  | 匯豐 | Almost never used | Password-protected PDF | TWD |
  | 國泰世華 | Main stock account | PDF | TWD |
  | Bank of America | Barely used; holds subscriptions | .txt | USD |
  | 中華郵政 | Almost never used | Screenshot | TWD |

- **Intake order:** password-protected PDF → PDF → .txt → screenshot. Import runs on request (a command now, a button later), and it loads everything it is given, with no date limit.
- **Reading:** an online AI service may read statements during development. Every input is tracked, and a file that was already read is never read again.
- **PDF passwords:** stored in the macOS Keychain.
- **Uncertain numbers:** they go to a separate review table with the same layout as the main one. They move into the main table only after the owner accepts them.
- **Duplicates and transfers:** detecting overlapping statements and transfers between the owner's own accounts is important. These must not count as spending.
- **Storage:** a database on the owner's Mac, in a data folder that git ignores. Original statement files are kept for now; whether to delete them later is an optimization to revisit. No backups for now.
- **Currency:** TWD is the main currency. USD is converted to TWD at one fixed rate set in settings.

## Features (first release)

- **Categories:** Food, Groceries, Transport, Rent, Shopping, Subscriptions, Entertainment, Investment, Income, Other.
  - The AI guesses the category. When the owner corrects one, the app remembers the correction for next time.
  - Each transaction has a main tag (used by the chart) and a filter tag. The design should allow more tags later.
  - Money moved to the stock account is tagged Investment. This is a quick fix that may double count.
- **Chart:** monthly bars, with a toggle between income vs spending side by side and spending stacked by category. A category checklist filters the chart. A "past 30 days" view is a later toggle.
- **Transactions behind the chart:** shows the top 5 transactions in a category by default; a setting shows the full list.
- **Subscriptions:**
  - Anything recurring counts; today that means phone bills and online services.
  - The summary shows each subscription's name, price and how often it's charged; the totals per month and per year; price increases; new subscriptions; and free trials that turned paid.
  - Each detected subscription gets a confidence score. Low-confidence ones go to their own review area to be confirmed.
  - Yearly subscriptions are spotted from their first charge by matching names against a list of common services and checking whether the amount looks like a yearly price.
  - Foreign-currency charges count as the same subscription; the fixed exchange rate keeps the amounts stable.
- **Written summaries:** a subscription summary and a written monthly spending summary (for example, "food is up 20% from last month"). These only describe spending; they don't recommend anything (see `docs/decisions/0005-advice-and-boundaries.md`).

## Later

- Budgets.
- A "past 30 days" chart toggle.
- More advanced financial advice.
- Deleting original statement files after extraction.
- More than two tags per transaction.
- Signing up other users.
- A web upload page, and hosting the app properly.

## Experience and Delivery

- **Languages:** English, and possibly Traditional Mandarin.
- **Devices:** mainly this Mac. Briefly showing the real graph on a public website is accepted as a deliberate, short-lived risk.
- **Accessibility:** no requirements for now.
- **Learning:** this is a learning project. Pick tools that teach servers and databases step by step, and explain each step.
- **Budget:** about 100 TWD per month for personal use. Expanding to other users means another conversation about cost.
- **Acceptance:** the owner can clearly see how much money they spend in a month.

## Resolved

- There are six banks.
- 國泰世華 offers a partial CSV export. It isn't used in the first release; the PDF is used instead.

## Next Gate

The first-release brief is approved (`02-first-release-brief.md`). Next is the technical plan. Do not begin application scaffolding before that plan is approved.
