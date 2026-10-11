# 0007 — Charts, currency and subscriptions

- **Status:** accepted
- **Date:** 2026-10-11
- **Source:** `docs/plans/01-discovery-followups.md` Q4, Q12, Q13, Q23–Q29; clarifications 3 and 8

## Decision

### Charts

- **Monthly bars by calendar month** are the default. A toggle switches between income vs spending side by side and spending stacked by category.
- **A category checklist** filters the chart.
- **Clicking a bar** shows the top 5 transactions in a category by default; a setting shows the full list.
- **A "past 30 days" view** is a later toggle.

### Currency

- **TWD is the main currency.** USD amounts are converted at one fixed rate set in settings. The same rate keeps foreign-currency subscriptions stable from month to month. This is a deliberate rough fix.

### Subscriptions (first release)

- **Anything recurring counts.** Today that means phone bills and online services.
- **The summary shows:**
  - each subscription's name, price and how often it's charged
  - totals per month and per year
  - price increases
  - new subscriptions
  - free trials that turned paid
- **Yearly subscriptions** are spotted from their first charge by matching names against a list of common services and checking whether the amount looks like a yearly price.
- **Confidence-based saving:** a detected subscription is saved only when its confidence is high enough; otherwise it goes to the review area (see 0002).
- **Written summaries:** both a subscription summary and a written monthly spending summary (see 0005 for limits).

## Alternatives considered

- **Live exchange rates:** more accurate, but monthly amounts would drift and it adds another external service. Deferred.
- **Waiting for two charges before spotting a yearly subscription:** rejected by the owner.

## Consequences

- The list of common subscription services has to be maintained.
- Totals in TWD are approximate whenever the real exchange rate moves away from the fixed rate.
