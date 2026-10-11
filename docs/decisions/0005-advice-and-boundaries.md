# 0005 — Product boundaries and financial advice

- **Status:** accepted
- **Date:** 2026-10-11
- **Source:** `docs/plans/01-discovery-followups.md` Q6, Q22, Q29; clarification 7

## Decision

- **Never connect to bank logins.**
- **Never hold or move money.**
- **Written summaries describe what happened.** Examples: the subscription summary, or "you spent 20% more on food than last month". They don't recommend saving, budgeting or investing actions. This follows the existing rule in `CLAUDE_CODE_PROMPT.md`.
- **Sending statement data to an online AI service is approved** for reading statements, guessing categories and writing summaries (see 0001).

## Later

- More advanced financial advice may become a feature. It needs its own decision before it's built.

## Consequences

- Prompts for writing summaries must ask for descriptions, not recommendations.
