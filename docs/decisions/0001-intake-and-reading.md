# 0001 — Statement intake and reading

- **Status:** accepted
- **Date:** 2026-10-11
- **Source:** `docs/plans/01-discovery-followups.md` Q1, Q2, Q5, Q6, Q19; clarifications 1 and 2

## Context

Six bank accounts send statements in four formats: password-protected PDF (CTBC, 台新, 匯豐), PDF (國泰世華), .txt (Bank of America) and screenshot (中華郵政). CSV export exists for at least one bank but only covers part of what's needed, so CSV can't be the only input.

## Decision

- **Build order for intake:** password-protected PDF → PDF → screenshot. The .txt format will be scheduled in the technical plan.
- **Each format has its own reader.** All readers produce the same transaction shape, so the source of a file (a local folder now, an upload page later) can be swapped without changing anything downstream.
- **An online AI service may read statements** during development. Text-based PDFs and .txt files are parsed directly where possible, and the AI service is used where parsing isn't reliable (screenshots, messy layouts). This keeps costs low.
- **Every input is recorded** with a content hash, when it was read, and what reading it cost. A file whose hash was already read is never sent to the AI service again.
- **PDF passwords are kept in the macOS Keychain** and read automatically (for example with Python's `keyring` library). macOS asks for permission once.
- **Intake loads everything it is given**, with no date limit.

## Alternatives considered

- **Screenshots only, as first imagined:** this is the least reliable input, so it was moved to last.
- **Only text recognition on the computer:** more private, but it makes more mistakes on Mandarin and on messy layouts. Rejected for now.
- **Passwords in the repo:** the owner allowed this as a fallback, but it isn't needed because the Keychain can be read automatically. It would also be unsafe while the repo is public.

## Consequences

- Statement data leaves the Mac whenever the AI service is used. This is accepted for development (see 0005).
- Each bank's layout needs its own parser or prompt. Banks are added one at a time.
- 國泰世華's partial CSV export is not used in the first release; its PDF is used instead.

## Amendment (2026-10-11, brief review)

- **Build order:** password-protected PDF → PDF → Bank of America .txt → screenshot.
- **Import runs on request:** one import function, called by a command now and by a button later. Watching the folder automatically is deferred.
