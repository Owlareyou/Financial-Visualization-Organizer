# Discovery Follow-ups

Status: waiting on owner answers. Builds on `00-discovery.md`. Each question says why it matters; write answers under **Answer:** and they will be folded back into the discovery plan.

## 1. The statements themselves

1. **What format do the emailed statements arrive in?** PDF attachment, password-protected PDF, the email body itself, or a link to the bank's site?
   *Why:* if they're PDFs with real text (not images), the app can read them exactly, with no screenshots and no text-recognition errors. Screenshots are the hardest and least reliable input.
   **Answer:**
2. **Can any of your banks export transactions as CSV or Excel** from their website or app?
   *Why:* a CSV is the most accurate input of all and would skip the reading step entirely for that bank.
   **Answer:**
3. **Which banks, and how many?** Are they bank accounts, credit cards, or both? Which country are they in?
   *Why:* each bank lays out statements differently; the first release could start with one or two.
   **Answer:**
4. **Which currencies appear?** If more than one, should totals be converted into one currency?
   **Answer:**
5. **How far back do you want to load?** Just from now on, or past months too?
   **Answer:**

## 2. Reading and trusting the numbers

6. **May the app send statement images to an online AI service** (for example, Claude) to read them? Or must everything stay on your computer?
   *Why:* online AI reads messy screenshots far better, and in both English and Mandarin, but your bank data leaves your machine and each statement costs a little money. On-computer text recognition is private and free but makes more mistakes.
   **Answer:**
7. **When the app isn't sure about a number, what should happen?** Options: show it to you to confirm, save it with a warning flag, or skip it.
   *Why:* you want zero manual steps after upload, but a misread "1,800" vs "18.00" would quietly break your monthly totals.
   **Answer:**
8. **Statements often overlap or repeat** (the same purchase on two screenshots; paying your credit card from your bank account; moving money between your own accounts). Should the app detect these automatically and leave them out of spending totals?
   **Answer:**

## 3. Categories

9. **What categories do you want to start with?** For example: Food, Groceries, Transport, Rent, Shopping, Subscriptions, Entertainment, Income, Other.
   **Answer:**
10. **How should transactions get their category?** Options: rules you set once ("anything from 7-ELEVEN is Food"), AI guessing, or both. When you correct a category, should the app remember that for next time?
    **Answer:**
11. **Can one transaction have more than one tag?** (e.g. Food *and* Travel)
    **Answer:**

## 4. The chart

12. **Bar chart shape:** side-by-side bars for income vs spending each month, or spending bars stacked by category? Or both?
    **Answer:**
13. **Besides the chart, do you want a list of the transactions** behind it (click a bar to see what you spent on)?
    **Answer:**

## 5. Storage and privacy

14. **After the numbers are extracted, should the screenshots be kept or deleted?**
    **Answer:**
15. **Backups:** is a copy on your computer enough, or do you want automatic backups somewhere?
    **Answer:**
16. **Important:** the GitHub repo is currently **public**. Real statements and the database must never be committed. Will you switch it to private, and are you happy for the app to keep its data in a folder that git ignores?
    **Answer:**

## 6. Platform and the future

17. **Which devices will you use it from?** Only this Mac, or also your phone?
    *Why:* phone access means the app has to be reachable from outside your computer, which is the step where security matters.
    **Answer:**
18. **"Deploy in the future" means:** online just for you, or something other people could sign up for?
    *Why:* other users means accounts, passwords and much stricter security; just-for-you is far simpler.
    **Answer:**
19. **Build order:** OK to start with "drop screenshots into a folder → data appears" and add the upload web page as the next step, if the design keeps the two swappable?
    **Answer:**
20. **Is learning part of the goal?** Should we pick tools that teach you servers and databases step by step, or tools that get it working fastest?
    **Answer:**
21. **Budget:** are you willing to pay for anything (AI reading per statement, hosting later), and roughly how much per month?
    **Answer:**
22. **Things the app should never do.** Suggested: never connect to bank logins, never move money, never give financial advice. Anything to add or remove?
    **Answer:**

## 7. Subscriptions

23. **What counts as a subscription?** Only services like Netflix and Spotify, or also phone bills, rent, insurance and loan payments?
    **Answer:**
24. **What should the summary show?** For example: each subscription's name, price and how often it's charged; the total per month and per year; price increases; new subscriptions; a free trial that turned into a paid one.
    **Answer:**
25. **Should the app guess, or ask you to confirm?** Should detected subscriptions be saved automatically, or shown to you to confirm or dismiss?
    *Why:* fully automatic fits "no manual steps", but a regular grocery run can look like a subscription.
    **Answer:**
26. **Yearly subscriptions** only show up once a year, so the app needs at least two charges to spot them. Is it OK if they're missed until then?
    **Answer:**
27. **Foreign-currency subscriptions:** a charge in another currency changes slightly with the exchange rate each month. Should it still count as the same subscription?
    **Answer:**
28. **First release or later?** Is this part of the first version, or does it come after the monthly charts?
    **Answer:**
29. **What does "summarization" cover?** Just a summary of your subscriptions, or also a written summary of each month's spending, like "You spent 20% more on food than last month"?
    **Answer:**
