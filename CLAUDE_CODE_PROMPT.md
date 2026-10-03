# Project Brief - Financial Banking Organizer

> Project stage: discovery. This brief is authoritative once decisions are approved.
> Start by reading this file and `docs/plans/00-discovery.md`.

## 0. Working Agreement

- Before implementation, propose a short file-by-file plan and wait for approval.
- Work on one scoped task at a time; summarize what changed and stop for review.
- Ask when requirements are ambiguous. Do not silently choose product or technical behavior.
- Keep scope to the approved brief. Record proposed additions before building them.
- Use synthetic data during discovery and development. Do not request or store real bank credentials or account identifiers.
- Do not connect to banks, move money, make financial recommendations, or send financial data to external services unless those capabilities are explicitly specified and approved.
- Keep comments focused on non-obvious reasons, not narration.

## 1. Context

The workspace is for designing and building a financial and banking organizer. The intended users, core problem, workflows, supported regions, and product boundaries have not yet been specified. Treat the project name as a working label, not as a complete requirements document.

## 2. Current Goal

Complete product discovery and agree on a first release before choosing an implementation stack or generating application code.

The discovery checklist in `docs/plans/00-discovery.md` tracks open questions. Do not fill in answers on the user's behalf.

## 3. Not Approved Yet

These are open decisions, not implied requirements:

- Target user and primary job to be done
- Supported financial institutions, account types, regions, and currencies
- Manual entry, file import, bank connections, or other data sources
- Data storage, retention, backup, export, and deletion behavior
- Authentication, authorization, privacy, and security requirements
- Web, mobile, desktop, or other platform
- Product features, visual design, and accessibility requirements
- Technology stack, hosting, integrations, and release process

Do not scaffold an application or add dependencies until the first-release scope and implementation plan are approved.

## 4. Discovery Exit Criteria

Discovery is complete when the user has approved:

1. The target user and primary workflow.
2. The first-release scope and explicit non-goals.
3. Data sources and the expected treatment of sensitive information.
4. Platform and measurable acceptance criteria.
5. A technical direction and a small, reviewable implementation plan.

Record durable product or technical rulings in `docs/decisions/`. Keep research findings and sources in `docs/research/`.