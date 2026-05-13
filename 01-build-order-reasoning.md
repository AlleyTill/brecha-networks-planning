# Build Order Reasoning — Brecha Networks

*Generated: 2026-05-12. Self-contained reasoning doc covering whether to build the comprehensive feature set first or the AWE-day demo first.*

---

## The question

The build has two visible deliverables:

1. **Comprehensive Brecha Networks platform** — full feature set per `decisions-locked.md` and `build-plan.md` (verified-pseudonymous identity, working groups, threaded comments, @mentions, follow issues, person search, geocoded barriers, status markers, GDPR data export, etc.).
2. **AWE-day demo** — narrow operational tool for the June 17, 2026 Healthcare Roundtable Table B session. Requirements: mobile-friendly signup via QR code, magic-link auth, minimal profile (username + role + region + bio), post a barrier (title + English summary + description), read other attendees' barriers from the event, host-side filter view for wrap-up.

The question: should the comprehensive build be started first, or should the AWE-day demo be built first?

---

## The answer

**Start the comprehensive build. The AWE-day demo is a checkpoint inside it, not a separate build.**

One codebase. Two planning documents reflecting different scope cutoffs.

---

## Why this is the right answer

### 1. AWE-day requirements are a strict subset of the comprehensive feature set.

There is no AWE-day functionality that does not also exist in the comprehensive plan. Specifically:

| AWE-day requirement | Comprehensive plan equivalent |
|---|---|
| Mobile signup with magic link | Already locked: Flow C code-based hybrid + magic link return, via `django-allauth` |
| Minimal user profile | Already locked: username + verified role + verified region + bio + their posts |
| Post a barrier | Already locked: `Barrier` model with title, English summary, full description, author, optional locations |
| Read others' barriers from the event | Already locked: barrier list + detail pages |
| Host-side filter for wrap-up | Trivially supported: filter list by `created_at` >= event start |

Nothing in the AWE-day scope adds new data models, new packages, or new architectural decisions that are absent from the comprehensive plan.

### 2. The architectural foundations are identical between the two.

Week 1 work in the locked `build-plan.md`:

- Django project initialized with `django-allauth`, `django-tailwind`, `django-htmx`, `django-environ`
- Custom user model with verified-pseudonymous fields (username + role + region + bio + email)
- URL conventions locked from day 1 (plural collections, slug-based detail URLs)
- Base template structure (Tailwind + HTMX + Alpine.js loaded)
- Basic landing page + About page + signup page skeleton

This work serves both the AWE-day demo and the comprehensive build. There is no decision where "for AWE we use X but for the comprehensive build we use Y" — the stack is locked at the architecture level.

Week 2 of the locked build adds the `Barrier` model, the `WorkingGroup` model, the `Comment` model, the `ConnectRequest` model, and the `Follow` model. The AWE-day demo requires only the `Barrier` model and the views around it. The other models can be deferred to weeks 3+ without affecting AWE-day readiness.

### 3. The differences between "AWE-day-ready" and "comprehensive-ready" are at the route, template, and UI layer — not the data model layer.

This means: with one codebase, the AWE-day demo is achieved by exposing a smaller surface area of the same models. A feature flag, conditional template, or simply unbuilt routes/views can hide functionality that's not ready while leaving the architecture intact.

There is no scenario where the AWE-day demo and the comprehensive build need to diverge at the codebase level.

### 4. The user's mental-model preference favors one codebase.

The user has explicitly stated: *"I need to stay in a space where I understand what layer I'm on, and what's happening with those layers."* (Quoted from 2026-05-12 conversation.)

Two parallel codebases — one for AWE, one for comprehensive — fragment the mental model. The user would have to track "which version has feature X" and risk drift between the two. The discipline of one codebase preserves the layered mental model.

### 5. The original locked `build-plan.md` already anticipates this.

The locked plan's week 7 "quality gate decision" reads: *"Is the foundation load-bearing? YES → outreach push for real verified users. NO → final polish only. AWE demo is the architecture itself."*

The build plan already treats AWE-day as a checkpoint inside the comprehensive build, not a separate artifact. The user's recent reframing ("plan-quality > completion-speed; AWE-day is narrow MVP if code lands at all") is fully consistent with this.

---

## What the "fork" therefore means

Earlier conversation referenced a "fork" of the planning doc for the demo, slimmed down with AI oversight. The correct interpretation:

- **The fork is a PLANNING FORK, not a codebase fork.** Two planning documents that prioritize the same build differently.
- **Comprehensive Planning Doc** lists all features in priority order, all weeks.
- **AWE-Day Planning Doc** identifies which subset of the comprehensive priority list must be functional by June 15, 2026 (flight day), plus pre-AWE operational tasks (printed QR codes, mobile signup test, wrap-up filter view, demo Plan B and Plan C, co-host pitch).
- Both planning docs reference the same `decisions-locked.md` for architectural source of truth.
- Same GitHub planning repository, but on different branches (`main` for comprehensive, `awe-day` for the slimmed demo plan) so the difference between "ship eventually" and "ship for AWE" is visible as a branch diff.

---

## Implications for execution

### Build sequence (one codebase, prioritized)

| Week | Work | AWE-day coverage |
|---|---|---|
| 1 | Django bootstrap + allauth + Tailwind + HTMX + base templates + custom user model + URL conventions + admin MFA | Foundation for both |
| 2 (early) | `Barrier` model + Django admin + minimal post form + list view + detail view | **AWE-day demo functional from this point** |
| 2 (late) — pre-AWE checkpoint | Mobile signup flow polish + QR code generation + magic link delivery tested on mobile + wrap-up filter view | AWE-day ready |
| 3-onward | `WorkingGroup` + `Comment` + `Friendship` + @mentions parser + search + Mapbox geocoding + status markers + etc. | Comprehensive build continues |
| 5 | Hardening (rate limiting, moderation, SPF/DKIM/DMARC, deliberate logging, backup key custody, GDPR data export, lawyer consult) | Same week 5 as locked plan |
| 6 | Eco-sphere + branding + analytics | Same week 6 as locked plan |
| 7 | Pre-AWE polish + final mobile testing + demo dry runs + printed QR codes + pre-AWE checklist | AWE-day final prep |
| AWE day (June 17) | Roundtable Table B session 3:30-4:30 PM | Operational use |
| Post-AWE | Continue comprehensive build per original plan; $2-3K enterprise stress test scheduled here | Full feature set |

### Demo Plan B and Plan C

Per the user's 2026-05-12 decision: plan for both code-readiness AND polished planning-artifact-as-WIP, in parallel. If neither is ready at AWE, concept-sell verbally.

- **Plan A:** Live working demo — attendees use Brecha during the Table B rotations, host reads wrap-up from the platform.
- **Plan B:** Polished planning artifact + 5-minute screencast of partial functionality. Audience can see what the platform does even if live demo fails. Architecture pitch as supporting material.
- **Plan C:** Verbal concept-sell at the table. The planning docs + roadmap + clear thesis carry the value. Sticky notes capture barriers traditionally; platform exists as future state.

### No code fork required

The "fork" the user mentioned is the planning-document fork: comprehensive plan on `main` branch, AWE-day plan on `awe-day` branch. The codebase itself remains one repo (`AlleyTill/brecha-networks`) once it exists.

---

## Decision

- **Comprehensive build starts first.** Week 1 work begins after this planning session is committed and printed.
- **AWE-day demo is a checkpoint inside the comprehensive build, reached at the end of Week 2.**
- **One codebase, two planning documents, two branches.** Branches: `main` (comprehensive) and `awe-day` (demo). Codebase: `AlleyTill/brecha-networks` (single repo, no fork).
- **The $2-3K enterprise stress check** offered by a respected industry professional is scheduled for post-AWE when the full feature set is in place. AWE-day usage at ≤50 concurrent attendees does not require external stress testing.

---

## Cross-references

- `decisions-locked.md` — architectural source of truth (existing on `main` branch)
- `build-plan.md` — week-by-week execution (existing; will be updated to reflect this reasoning)
- `02-main-planning-supplement.md` — comprehensive planning supplement (this session's additions)
- `03-awe-day-plan.md` — narrow demo plan
- `04-session-recap-2026-05-12.md` — full audit trail of today's session

End of build-order reasoning.
