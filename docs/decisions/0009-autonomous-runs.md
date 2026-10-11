# 0009 — Autonomous runs between checkpoints

- **Status:** accepted
- **Date:** 2026-10-11
- **Source:** `docs/plans/03-technical-plan.md` sections 2 and 4

## Context

The working agreement said to stop for review after every task. The owner wants an AI agent to work through the plan for long stretches and stop only at checkpoints.

## Decision

- **Between checkpoints, the agent completes tasks one after another without stopping for review.** It follows the protocol in `03-technical-plan.md` section 2, and `docs/plans/PROGRESS.md` is the source of truth.
- **Every task must pass its "Done when" checks, `ruff` and `pytest` before it's committed.**
- **Work happens on a milestone branch.** The agent never pushes or merges to `main`; the owner does both at checkpoints.
- **The agent stops at:**
  - every checkpoint
  - a task that still fails after 3 attempts
  - an ambiguous requirement
  - a new Python dependency
  - AI spend over US$0.50 in one run
  - anything touching `docs/decisions/`, `main` or the remote repository
- **The agent may install system software with Homebrew** and records each install in `PROGRESS.md`.
- **Secrets and real statements stay with the owner.** The agent never creates, prints or changes Keychain entries. It reads files in `data/` only in tasks marked "reads real data", and never copies real values into the repo.

## Consequences

- Review happens per milestone, not per task, so the checkpoint reports must be thorough.
- Claude Code permissions for the run are in `.claude/settings.local.json`. Pushing and editing `docs/decisions/` or `data/` still ask first.
