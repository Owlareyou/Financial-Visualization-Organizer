# 0003 — Categories and tags

- **Status:** accepted
- **Date:** 2026-10-11
- **Source:** `docs/plans/01-discovery-followups.md` Q9, Q10, Q11; clarifications 4 and 5

## Decision

- **Starting categories:** Food, Groceries, Transport, Rent, Shopping, Subscriptions, Entertainment, Investment, Income, Other.
- **Each transaction has a main tag and a filter tag.** The chart uses only the main tag, so stacked bars never count a transaction twice. The filter tag is used for filtering. Store tags so that more can be added later without changing the database layout.
- **The AI guesses each category.** When the owner corrects one, the app saves the correction as a rule for that merchant and applies it the next time.
- **Investment:** money moved to the stock account (國泰世華) is tagged Investment. This is a quick fix.
- **LINE Pay:** charges that show only "LINE Pay", with no merchant name, are acceptable. They will often land in Other.

## Alternatives considered

- **Rules only:** too much setup.
- **Unlimited tags with no main tag:** stacked charts would double count.
- **Treating money to the stock account as a transfer:** cleaner, but deferred.

## Consequences

- Investment may double count, for example when money moves into the stock account and later back out. Revisit this once real data shows how it behaves.
- Categorization calls the AI service, which adds to the monthly cost (see 0006).
