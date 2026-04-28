# Brecha Networks — Planning

**Brecha Networks** is a coordination platform for healthcare professionals and developers working on cross-jurisdictional access barriers. Verified-pseudonymous identity, native-language posts with required English summaries, geocoded foundation, working-group formation around shared barriers.

This repository holds planning and decision documents. The codebase lives in a separate repository (set up in week 1 of the build).

## Files

- **`decisions-locked.md`** — finalized architectural and product decisions. Tech stack, privacy/identity model, content language, forum features, legal posture, hosting, branding, build sequence, budget. Append-only change log at the bottom.
- **`build-plan.md`** — week-by-week unified build plan with embedded learning topics, AI-assistance gradient, quality gates, and post-AWE roadmap. Derived from `decisions-locked.md`.
- **`open-questions.md`** — remaining unresolved items (mostly desktop research tasks + parked tactical follow-ups). Most prior questions have been resolved and moved to `decisions-locked.md`.

## Working with Claude on this project

When starting a new conversation about Brecha Networks, share the relevant subset of these files to restore context:

- For architectural questions: `decisions-locked.md`
- For build-step questions: `build-plan.md`
- For "what's still undecided?": `open-questions.md`

When decisions change or new ones get locked:

1. Discuss the decision fully before updating files
2. Update `decisions-locked.md` and add a line to its change log
3. Remove the corresponding question from `open-questions.md` if applicable
4. Commit with a clear message describing what changed

## Project status

| Aspect | Status |
|---|---|
| Project name | **Locked: Brecha Networks** |
| Tech stack | **Locked: Django + HTMX + Alpine.js + Tailwind + PostgreSQL/PostGIS** |
| Identity model | **Locked: Model D — Verified-Pseudonymous Identity** |
| Build start | ~Late April 2026 |
| AWE 2026 unveiling | June 15-18, 2026 |
| AWE deadline status | **Non-sacred** — quality gate is "load-bearing platform," not "demo by AWE" |

## Predecessor projects

- **Barrier Registry** (`AlleyTill/barrier-registry`) — predecessor research project, top-down state-by-state policy scraping. Archived. Pivoted to Brecha Networks after legal infeasibility for solo builder. Public for cohort/portfolio reference.
- **Community Registry (working name)** — interim working name during planning phase. Rolled into Brecha Networks. Old planning repo deleted; this repo supersedes it.

## License

Planning documents, no code license needed. Codebase repository (separate) will use MIT or similar permissive license at first push.
