# 0004 — Storage and privacy

- **Status:** accepted
- **Date:** 2026-10-11
- **Source:** `docs/plans/01-discovery-followups.md` Q14, Q15, Q16

## Decision

- **The database runs on the owner's Mac.**
- **All real data stays in one data folder that git ignores.** That covers statements, the database and extracted output. Because of this layout, the repo can be public and other people can use the code without getting anyone's data.
- **Original statement files are kept** after extraction. Deleting them is a later optimization, so the design should leave room for it.
- **No backups for now.**

## Alternatives considered

- **Making the repo private:** not chosen. The gitignored folder protects the data whether the repo is public or private.

## Consequences

- The data folder must be in `.gitignore` before any real statement is added. It's worth adding a check that stops a commit if a statement or database file is staged.
- If the Mac's disk is lost, all extracted data is lost too. This is accepted for now.
- Secrets such as PDF passwords and API keys live in the Keychain or in gitignored files, never in the repo.
