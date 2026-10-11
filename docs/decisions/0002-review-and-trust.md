# 0002 — Review queues, duplicates and transfers

- **Status:** accepted
- **Date:** 2026-10-11
- **Source:** `docs/plans/01-discovery-followups.md` Q7, Q8, Q25

## Context

After upload, the owner wants no manual steps. But a misread amount, a counted duplicate or a wrongly detected subscription would quietly make the totals wrong.

## Decision

- **Uncertain transactions** go to a review table with the same layout as the main transactions table. A transaction moves into the main table only when the owner accepts it. Charts and summaries use the main table only.
- **Uncertain subscriptions** go to a separate review area of the same kind. A detected subscription is saved automatically only when its confidence is above a threshold; otherwise it waits for the owner to confirm or dismiss it.
- **Duplicates and transfers are detected automatically.** That covers the same transaction appearing in overlapping statements, and money moving between the owner's own accounts. Neither counts as spending or income.

## Alternatives considered

- **Save everything with a warning flag:** rejected, because flagged errors would still change the totals.
- **Skip uncertain items:** rejected, because data would be lost silently.

## Consequences

- Every extracted item needs a confidence value, and the threshold must be easy to adjust.
- Transfer detection needs a list of the owner's own accounts.
- Money moved to the stock account is handled by 0003 (the Investment category), not by transfer detection, for now.
