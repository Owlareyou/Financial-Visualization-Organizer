# First-Release Brief

Status: **approved** (2026-10-11). The answers to the review questions are in section 8. This brief sets scope only and doesn't name any tools; the tools are chosen in the technical plan.

## 1. Goal

The owner can clearly see how much money they earn and spend each month across all their bank accounts, and what they pay for subscriptions. The only manual work is supplying statement files and reviewing items the app is unsure about.

## 2. User

The owner, alone, on their Mac. The design must not rule out other users later (0006), but the first release has no sign-in.

## 3. In Scope

### Intake

- Statement files are placed into an intake folder inside the gitignored data folder. Import runs on request: one import function that a command calls now and a button calls later, not a folder watcher. Each file is read once, and every read is logged with its file hash and cost (0001).
- Formats, in build order: password-protected PDF (CTBC, 台新, 匯豐) → PDF (國泰世華) → Bank of America .txt → screenshot (中華郵政). The .txt comes before screenshots because the subscriptions you need to sort out are on that account.
- PDF passwords are read from the macOS Keychain.

### Data quality

- Uncertain transactions wait in a review table. Only accepted ones count (0002).
- Duplicates from overlapping statements and transfers between your own accounts are detected and left out of totals.
- USD is converted to TWD at a fixed rate set in settings (0007).

### Categories

- Ten categories. Each transaction has a main tag and a filter tag. The AI guesses the category, and the app remembers your corrections (0003).

### Charts

- Monthly bars, with a toggle between income vs spending side by side and spending stacked by category.
- A category checklist filters the chart.
- Clicking a bar shows the top 5 transactions by default, with a setting for the full list.

### Subscriptions

- Detection with confidence scores and a review area for low-confidence ones.
- Spotting yearly subscriptions from a list of common service names and their typical prices.
- The summary shows each subscription's name, price and how often it's charged; totals per month and per year; price increases; new subscriptions; and free trials that turned paid (0007).

### Written summaries

- A subscription summary and a monthly spending summary. They describe spending only and never recommend anything (0005).

### Public view

- A way to show the chart on a public web page for a short time, with an obvious switch to turn it off (0006).

### Settings

- The exchange rate, the confidence threshold (starts loose, so fewer items go to review, and can be changed later), and whether clicking a bar shows the top 5 or all transactions.

## 4. Non-goals for the First Release

- Connecting to bank logins, or holding or moving money.
- Recommending financial actions. Summaries only describe spending.
- Budgets.
- A "past 30 days" chart view.
- Sign-up, sign-in, or more than one user.
- A web page for uploading files, and reading the intake folder automatically when a file appears. Intake runs on request for now.
- Live exchange rates.
- Deleting original statements, and backups.
- More than two tags per transaction.
- Treating money moved to the stock account as a transfer. It's tagged Investment instead.
- 國泰世華's partial CSV export. Its PDF is used instead.
- Mobile layout, Traditional Mandarin interface, and accessibility work.

## 5. Milestones

Each milestone ends with something you can see and check yourself. We'll stop for your review after each one.

| # | Milestone | What you can see at the end |
|---|---|---|
| M1 | One bank end to end: CTBC password-protected PDF → database → monthly bar chart | Your real CTBC months in a chart |
| M2 | Trust: review table, duplicate and transfer detection, read log with cost | Uncertain rows waiting for review; re-importing a file changes nothing |
| M3 | Categories: AI guessing, remembered corrections, two tags, stacked toggle, checklist filter, top-5 list | Spending broken down by category |
| M4 | More banks: 台新, 匯豐, 國泰世華, then Bank of America .txt with the fixed exchange rate | All text-based banks in one chart |
| M5 | Subscriptions: detection, review area, yearly-subscription matching, summary | Your subscriptions and their monthly and yearly totals |
| M6 | Written monthly spending summary | A short written summary under each month |
| M7 | 中華郵政 screenshots | All six banks |
| M8 | Public view with an off switch | A link you can show someone |

## 6. Acceptance Criteria

The first release is done when all of these hold:

1. **Accuracy:**
   - For every text-based PDF and the .txt file, the extracted transactions add up exactly to the statement's own totals or the change in balance.
   - For screenshots, any row whose total doesn't match goes to review instead of the main table.
2. **No double reading:** importing the same file again adds 0 transactions and makes 0 AI calls.
3. **No double counting:**
   - A transaction that appears in two overlapping statements counts once.
   - A transfer between two of your own accounts counts as neither spending nor income.
4. **Review gate:** nothing in either review area appears in charts, totals or summaries until you accept it.
5. **Corrections stick:** after you correct a merchant's category once, that merchant's next transaction gets the corrected category with no AI call.
6. **Charts:**
   - Every month with data shows income and spending bars.
   - The stacked view never counts a transaction in two categories.
   - Turning off a category in the checklist updates the chart.
7. **Subscriptions:**
   - Your phone bill and every recurring Bank of America charge are listed, with correct monthly and yearly totals in TWD.
   - A known yearly service is listed after its first charge.
8. **Summaries:** each monthly summary describes changes from the previous month and contains no recommendations.
9. **Cost:** a normal month of statements for all six banks costs under 100 TWD in AI fees, as shown by the read log.
10. **Hands-off:** from dropping files into the intake folder to seeing them in the chart takes no manual steps besides review.
11. **Privacy:** no statement, database file, password or API key is ever committed, and a git check blocks attempts to commit them.

## 7. Risks

- **Investment double counting:** money moving to and from the stock account may inflate totals. Revisit this after M4.
- **LINE Pay:** charges without a merchant name will often land in Other.
- **Screenshot accuracy:** this is the weakest input, which is why it comes last.
- **Public view:** exposes real finances while it's on. You accepted this in 0006.
- **Learning pace:** explaining each tool slows early milestones, which is intended.

## 8. Review Answers (2026-10-11)

1. **Milestone order:** approved, including Bank of America .txt in M4, before screenshots.
2. **Acceptance criteria:** approved as written.
3. **Review threshold:** starts loose, so fewer items go to review. It can be changed later.
4. **Intake trigger:** a command, button or callable function first. Automatic folder watching comes later.
5. **Banks:** six. 國泰世華 offers the partial CSV export, which isn't used in the first release.
