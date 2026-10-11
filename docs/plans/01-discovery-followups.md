# Discovery Follow-ups

Status: answered 2026-10-11, with clarifications in section 8. Folded into `00-discovery.md`; settled rulings are in `docs/decisions/`. Builds on `00-discovery.md`.

## 1. The statements themselves

1. **What format do the emailed statements arrive in?** PDF attachment, password-protected PDF, the email body itself, or a link to the bank's site?
   *Why:* if they're PDFs with real text (not images), the app can read them exactly, with no screenshots and no text-recognition errors. Screenshots are the hardest and least reliable input.
   **Answer:**
    after some digging, i have four banks in taiwan, one in usa. out of the five, two has password pdf, one can do csv, one can to .txt, and one is a screenshot.

2. **Can any of your banks export transactions as CSV or Excel** from their website or app?
   *Why:* a CSV is the most accurate input of all and would skip the reading step entirely for that bank.
   **Answer:**
    yes, but only partial. so csv can not be the only input

3. **Which banks, and how many?** Are they bank accounts, credit cards, or both? Which country are they in?
   *Why:* each bank lays out statements differently; the first release could start with one or two.
   **Answer:**
    Bank of America: I barely use, but there are subscription things on it i need to work on.
    匯豐 與 中華郵政 both almost never use.
    CTBC: My main salary account. most of my line pay transaction is on it.
    國泰世華: my main stock account.
    台新：Old salary account. sometime use, has a decent amount of money.

    i believe all of them are accounts. i don't have credti cards, yet.


4. **Which currencies appear?** If more than one, should totals be converted into one currency?
   **Answer:**
    TWD is the main. BOA should convert to TWD

5. **How far back do you want to load?** Just from now on, or past months too?
   **Answer:**
    i want past 30 days as default, but ablity to change past months too.


## 2. Reading and trusting the numbers

6. **May the app send statement images to an online AI service** (for example, Claude) to read them? Or must everything stay on your computer?
   *Why:* online AI reads messy screenshots far better, and in both English and Mandarin, but your bank data leaves your machine and each statement costs a little money. On-computer text recognition is private and free but makes more mistakes.
   **Answer:**
    I think i am willing to let it leave my computer as this stage of development. causeing money is not an issue per input as long as we have a way to keep track of the inputs and not do any unnesscary re-reading images.


7. **When the app isn't sure about a number, what should happen?** Options: show it to you to confirm, save it with a warning flag, or skip it.
   *Why:* you want zero manual steps after upload, but a misread "1,800" vs "18.00" would quietly break your monthly totals.
   **Answer:**
    i want a seperate identical place where any uncertainty number is saved. and user can manually check, if they are fine with thre reading it will be added into the formal table of numbers.

8. **Statements often overlap or repeat** (the same purchase on two screenshots; paying your credit card from your bank account; moving money between your own accounts). Should the app detect these automatically and leave them out of spending totals?
   **Answer:**
yes. this is important


## 3. Categories

9. **What categories do you want to start with?** For example: Food, Groceries, Transport, Rent, Shopping, Subscriptions, Entertainment, Income, Other.
   **Answer:**
    the list you gave is perfect.Food, Groceries, Transport, Rent, Shopping, Subscriptions, Entertainment, Income, Other.


10. **How should transactions get their category?** Options: rules you set once ("anything from 7-ELEVEN is Food"), AI guessing, or both. When you correct a category, should the app remember that for next time?
    **Answer:**
    ai guessing + remmebering when corrected

11. **Can one transaction have more than one tag?** (e.g. Food *and* Travel)
    **Answer:**
    yes

## 4. The chart

12. **Bar chart shape:** side-by-side bars for income vs spending each month, or spending bars stacked by category? Or both?
    **Answer:**
    both with toggle options

13. **Besides the chart, do you want a list of the transactions** behind it (click a bar to see what you spent on)?
    **Answer:**
    yes. in settings this will defaul to biggest top 5 spending in a category. and with optoins of seeing a list of all tranactions


## 5. Storage and privacy

14. **After the numbers are extracted, should the screenshots be kept or deleted?**
    **Answer:**
    kept for now. but keep in mind of this desgn for future optimization

15. **Backups:** is a copy on your computer enough, or do you want automatic backups somewhere?
    **Answer:**
    no backups for now.

16. **Important:** the GitHub repo is currently **public**. Real statements and the database must never be committed. Will you switch it to private, and are you happy for the app to keep its data in a folder that git ignores?
    **Answer:**
    if i want to share this to other to use, i think its best to deisgn it so there is a folder for git to ignore.

## 6. Platform and the future

17. **Which devices will you use it from?** Only this Mac, or also your phone?
    *Why:* phone access means the app has to be reachable from outside your computer, which is the step where security matters.
    **Answer:**
    mainly on mac. but having a not safe feature of being able to read the graph on a website is kinda cool as a flex. for a small duration.


18. **"Deploy in the future" means:** online just for you, or something other people could sign up for?
    *Why:* other users means accounts, passwords and much stricter security; just-for-you is far simpler.
    **Answer:**
    design the project in a way that it can be modulated into a app for others to sign up for.


19. **Build order:** OK to start with "drop screenshots into a folder → data appears" and add the upload web page as the next step, if the design keeps the two swappable?
    **Answer:**
lets start with "password protected pdf" and then to "pdf" and then to screenshots


20. **Is learning part of the goal?** Should we pick tools that teach you servers and databases step by step, or tools that get it working fastest?
    **Answer:**
this is a learnign opurtunity and i need to be taught

21. **Budget:** are you willing to pay for anything (AI reading per statement, hosting later), and roughly how much per month?
    **Answer:**
    yes. for my personal use maybe 100 twd is acceptable. if expand to others then well need to talk then.


22. **Things the app should never do.** Suggested: never connect to bank logins, never move money, never give financial advice. Anything to add or remove?
    **Answer:**
never connect ot bank logins.
never have money.
we could have financial advice that is fine


## 7. Subscriptions

23. **What counts as a subscription?** Only services like Netflix and Spotify, or also phone bills, rent, insurance and loan payments?
    **Answer:**
anything reaccuring counts. wlhtouh i do not have insureance and rent. so only phone bills for now.


24. **What should the summary show?** For example: each subscription's name, price and how often it's charged; the total per month and per year; price increases; new subscriptions; a free trial that turned into a paid one.
    **Answer:**
each subscription's name, price and how often it's charged; the total per month and per year; price increases; new subscriptions; a free trial that turned into a paid one.

25. **Should the app guess, or ask you to confirm?** Should detected subscriptions be saved automatically, or shown to you to confirm or dismiss?
    *Why:* fully automatic fits "no manual steps", but a regular grocery run can look like a subscription.
    **Answer:**
have a confidence system. that only ask for confrimation, such as the "7. **When the app isn't sure about a number, what should happen?**" probelm as before. a similar but seperate place for manual confirm if confidence is lower then a threshold.


26. **Yearly subscriptions** only show up once a year, so the app needs at least two charges to spot them. Is it OK if they're missed until then?
    **Answer:**
no. have a way of spoting them. one suggestion is base on names. have a list of most common subscriptions to macth and use the dollar ammount to guesstimate if tis a yearly subscription.

27. **Foreign-currency subscriptions:** a charge in another currency changes slightly with the exchange rate each month. Should it still count as the same subscription?
    **Answer:**
yes. and the fix for this is to have a fix exchange rate decided be fore in advacne. or in settings.

28. **First release or later?** Is this part of the first version, or does it come after the monthly charts?
    **Answer:**
yes this is aprt of the first version.

29. **What does "summarization" cover?** Just a summary of your subscriptions, or also a written summary of each month's spending, like "You spent 20% more on food than last month"?
    **Answer:**

both.

## 8. Clarifications (2026-10-11)

Asked after reviewing the answers above. Numbers in brackets point to the original questions.

1. **Format per bank** [Q1, Q19]
   - Password-protected PDF: CTBC, 匯豐, 台新
   - PDF: 國泰世華
   - .txt: Bank of America
   - Screenshot: 中華郵政
2. **PDF passwords** [Q19]: store them in the macOS Keychain if the app can read them from there without manual steps. Otherwise, keep them in the repo for faster development and come back to it later.
3. **"Past 30 days"** [Q5]: this was only about the chart. Default to calendar months; add a "past 30 days" toggle later. Intake loads everything it is given.
4. **Tags** [Q11, Q12]: each transaction has a main tag (used by the chart) and a filter tag. Two tags for now, but keep the design open to more later.
5. **Stock account transfers** [Q3, Q8]: add an "Investment" category as a quick fix, accepting some risk of double counting. LINE Pay charges without merchant detail are acceptable.
6. **Public graph** [Q17]: showing real finances publicly is fine for now. Build quick, fail fast.
7. **Financial advice** [Q22, Q29]: stay within the existing policy for now. Note that more advanced advice may be added as a feature later. The app never holds or moves money.
8. **Exchange rate** [Q4, Q27]: one fixed USD→TWD rate in settings covers both Bank of America totals and foreign-currency subscriptions. This is a deliberate rough fix.

### Still open

- Q1 said five banks, but Q3 and clarification 1 list six. Six is assumed.
- Q1 and Q2 mention a bank with partial CSV export, but no bank is listed with CSV above. Which bank offers it, and what does "partial" cover?

