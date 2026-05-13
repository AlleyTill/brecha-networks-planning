# `awe-day` branch — AWE 2026 Demo Operational Branch

*This branch was forked from `main` on 2026-05-12. It is the slimmed copy of the comprehensive planning materials, focused on AWE-day execution. See `03-awe-day-plan.md` as the primary operational document for this branch.*

---

## What this branch is for

This branch holds the planning materials in the form most useful for AWE-day execution. The same Brecha Networks codebase is being built; this branch's planning docs prioritize the subset of features and operational tasks needed for the June 17, 2026 Healthcare Roundtable Table B session.

For the reasoning behind one-codebase-two-plans (instead of two parallel builds), see `01-build-order-reasoning.md`.

## Primary doc on this branch

**`03-awe-day-plan.md`** — narrow operational plan. Covers:

- AWE-day functional requirements (signup, magic link, profile, post a barrier, read others' barriers).
- Pre-AWE checklist.
- Plan A (live working demo), Plan B (polished WIP artifact), Plan C (verbal concept-sell).
- Wrap-up mechanism for the host.
- Co-host pitch to José Ferrer Costa (Table A) and Robin White Owen (Table C).
- AWE-day-of operational logistics.
- What is explicitly cut from AWE-day scope.

## Supporting docs on this branch

- `04-session-recap-2026-05-12.md` — complete audit trail of the 2026-05-12 planning session. Keystone archival artifact. Reference if any context about today's planning is unclear.
- `02-main-planning-supplement.md` — comprehensive supplement covering threat model, security additions, physical archival protocol, stress test scheduling. Background context.
- `01-build-order-reasoning.md` — why one codebase, two planning docs.

## Comprehensive-build docs (also on this branch, secondary focus)

- `decisions-locked.md` — architectural source of truth (full comprehensive build).
- `build-plan.md` — week-by-week execution (full comprehensive build).
- `role-taxonomy.md` — 42 roles + Other taxonomy.
- `research-backlog.md` — parked tactical to-dos.
- `README.md` — overall planning repo overview.

These docs apply on this branch too, but the operational focus is the AWE-day subset.

## What changes on this branch (vs main)

Nothing in the codebase. Nothing in the architectural decisions. The branch difference is in **emphasis**: on this branch, the AWE-day plan is the primary operational document; on `main`, the comprehensive plan is primary.

If decisions need to be made about AWE-day scope (e.g., cutting a feature from AWE-day that remains in the comprehensive build), the change is captured in the AWE-day plan on this branch.

If decisions need to be made about the comprehensive build, those updates happen on `main` and may or may not be reflected in this branch.

## After AWE 2026

Once AWE is complete, this branch's purpose ends. The branch can be:

- Merged back into `main` with any retro notes from AWE-day execution (recommended).
- Archived (deleted) and a new branch created for the next event if relevant.
- Kept as a historical record of the AWE-day execution plan.

The decision can wait until after AWE.

## Branch fork date

Forked from `main` at commit `06a6aba` on 2026-05-12.

---

End of branch README.
