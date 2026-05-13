# Main Planning Supplement — Brecha Networks

*Generated: 2026-05-12. Supplements `decisions-locked.md` and `build-plan.md` with additions established during the 2026-05-12 planning session. Self-contained: a reader of this document and `decisions-locked.md` together has the full operational picture for the comprehensive build.*

---

## How to read this document

This file does not replace `decisions-locked.md` or `build-plan.md`. Those remain the source of truth for architecture and weekly execution respectively. This supplement adds material from the 2026-05-12 planning session that is not yet captured in those files:

1. Physical archival protocol (operational hygiene for the threat model)
2. Three security additions to the build (admin MFA Week 1, deliberate logging Week 5, backup key custody Week 5)
3. Threat model context (concrete current threat; protective theory of AWE visibility; post-AWE departure)
4. AWE-day demo relationship to the comprehensive build
5. Enterprise stress test scheduling (post-AWE, value $2-3K, offered)
6. Co-host context for AWE roundtable
7. Updated post-AWE roadmap notes
8. Memory and continuity notes

When the items above are integrated into `decisions-locked.md` and `build-plan.md` via commits, this supplement document can be retired or marked archived. Until then, this is a live planning artifact.

---

## 1. Physical archival protocol

### Daily print

- Code committed that day + chat histories from that day, filed by date in a 3-inch industrial folder.
- Four backup folders are pre-prepared and on hand.
- Blank notebook and printer paper available for any-day-of writing.

### Weekly full reprint

- Full codebase printed weekly as a complete recovery checkpoint.
- `decisions-locked.md`, `build-plan.md`, `role-taxonomy.md`, and `research-backlog.md` print on the same weekly cadence as the codebase. Architectural memory is what makes a scan-back recoverable as the same project rather than a fresh start with similar features.

### Print settings (locked for OCR recoverability)

- Monospaced font: Fira Code, JetBrains Mono, or plain Courier.
- Single-column layout.
- Generous margins.
- One-sided printing.
- Every page header includes: filename + commit hash + date.

These choices significantly reduce OCR error rates if scan-back-to-digital is ever needed, and the commit hash allows verification against any remaining git fragment that the scan matches what was committed.

### Threat-model rationale

Git history is only as durable as access to the host. Paper with dated isolation provides a recovery point that cannot be revoked, corrupted remotely, or selectively edited after the fact. Five-folder redundancy means a single physical loss does not end the project.

### Scope

The protocol applies to:

- Codebase (full reprint weekly)
- All planning documents (`decisions-locked.md`, `build-plan.md`, `role-taxonomy.md`, `research-backlog.md`, this supplement, the AWE-day plan, the session recap)
- Daily chat history relevant to the build
- Decision logs from session-to-session reviews

---

## 2. Three security additions to the build

These follow from the threat model and are not yet reflected in `decisions-locked.md` or `build-plan.md` as of 2026-05-02.

### 2.1 Admin MFA from Week 1 (not Week 5)

The `django-allauth` package supports multi-factor authentication natively. The original locked plan treated MFA as a Week 5 hardening item. Under the threat model, MFA on the admin account is foundational infrastructure, not polish: if the admin account is compromised, every user's identifying data is exposed.

**Action:** Configure admin MFA during Week 1 alongside the initial `django-allauth` setup. Use TOTP (Time-based One-Time Password) via `django-otp` or the `django-allauth` MFA extension. Test the recovery codes; store them in the physical archival folder.

### 2.2 Deliberate logging configuration (Week 5)

Django and most third-party packages log IP addresses, session data, request headers, and PII in error reports by default. For a platform whose users could be targets, the principle is: minimize what is collected, minimize what is retained, minimize who can access it under subpoena.

**Action in Week 5 hardening phase:**

- Audit Django's default logging configuration. Identify what is logged about user requests and errors.
- Configure `LOGGING` in settings to exclude PII from error reports. Use `django.utils.log.AdminEmailHandler` carefully; redact IP, session keys, and request bodies from emailed error reports.
- Disable or scope-limit IP address logging in `django-allauth` (audit `allauth.account.signals`).
- For request logs at the web server level (Railway's default logging), set short retention windows where configurable.
- Document what IS logged and where it is retained, in a `LOGGING.md` file in the codebase repo. This makes the privacy posture auditable.

### 2.3 Backup key custody (Week 5)

Railway's at-rest encryption is managed by Railway, which means Railway holds the keys, which means Railway can decrypt under subpoena from US authorities. The threat model includes US state-actor risk; this is not theoretical.

**Action in Week 5:**

- Research and implement an additional encryption layer: encrypted database dumps where the encryption key is held by the user (Alley), not Railway.
- Use a tool like `age` or `gpg` with an offline-stored key. The encrypted dumps can be transported to off-Railway storage (encrypted USB drives, secure cloud storage in a non-US jurisdiction).
- Daily automated dump, encrypted, retained for 7 days. Weekly dump retained for 4 weeks. Monthly dump retained indefinitely.
- The encryption key is stored separately from the dumps: one copy in the physical archival folder system, one copy with a trusted out-of-country contact, one copy in a hardware security module if available.
- Document the procedure in `BACKUP.md` in the codebase repo. Include the recovery walkthrough so a non-Alley operator could decrypt and restore.

### 2.4 Operational hygiene around public surface

The planning repository and `AlleyTill` GitHub handle are intentionally public. Visibility is the protective theory of the AWE unveiling. Therefore: everything in the public layer should be reviewable by adversaries without giving them anything operational.

**Public surface (architecture, decisions, role taxonomy):** continue to be public. These are the protective signals.

**Not public:** daily build notes, AWE day logistics, names of people on stage with the user, physical movements, demo timing details, departure plans, destination details, contact lists. These go in private channels — a private repo, encrypted notes, or out of writing entirely. Do not place a `notes/daily.md` on the public planning repo.

### 2.5 Offline encrypted copy

One-evening task to complete before AWE travel:

- Offline encrypted copy of the entire planning repo, codebase scaffolding (once it exists), credentials, contacts, and important emails on encrypted external media.
- If GitHub access is lost mid-transit, rebuild from local.
- Standard threat-model practice. Combines with the physical archival folder system to ensure full redundancy across digital and analog media.

---

## 3. Threat model context

The 2026-05-12 conversation surfaced threat model context that was not previously captured in the planning docs. Recording it here so the protective theory of the project is documented.

### 3.1 Concrete current threat

The user belongs to multiple groups that help marginalized communities. Two of those groups have been declared "anti-American" by the US president and Department of Justice, with public statements that members of those groups will be found and killed. Immediate family includes LGBTIQQ and disabled members; some family members are employed in roles that have experienced direct danger.

This is not structural background risk. It is concrete and current as of the conversation date (2026-05-12).

### 3.2 Protective theory of AWE visibility

Successful AWE unveiling and adoption across roundtables provides visibility on the C-suite/enterprise map. That visibility is a form of protection against state-actor targeting because:

- Public-facing founders with active investor relationships are more difficult to "disappear" without notice.
- Coverage of the platform and its founder creates a record of existence that complicates erasure.
- Connections with international industry leaders open paths to non-US jurisdictions where counsel and rights remain functional.

The platform's success matters in proportion to the personal-protection theory. Things that protect the project (health, decision quality, redundant advisors, secure operations, legal coverage) matter more proportionally than they would in a non-threat scenario.

### 3.3 Post-AWE departure

The user and family plan to leave the United States as soon as AWE hype dies down, after gaining as much investment or social capital as possible at the event. All work will continue remotely from the destination location on the existing laptop.

Operational implications:

- Public surface must not confirm departure plans or destination on the planning repo, social accounts, or any indexable location.
- Personal-protection legal counsel lives in the destination country, not the United States. The committed $300-500 platform-lawyer consult addresses platform-level legal questions; personal-protection counsel is a separate budget in the destination country.
- Offline encrypted copy on external media must be portable and prepared before flight.
- Travel-day operational details (flight numbers, dates, names of accompanying persons) stay out of writing entirely.

### 3.4 Roommate as cognitive and welfare check

The user has a roommate who is a psychiatrist and has agreed to serve as the active welfare check during the build:

- Real-time output review (sees what gets produced and when).
- Brings food.
- Sends the user to sleep when needed.
- Qualified to assess cognitive state, not only physical state.

This is the external scaffolding for sustained 12-14 hour days plus a prescribed Adderall protocol. It is sufficient. Claude (this assistant) does not need to repeat concerns about cognitive monitoring; that role is filled by a qualified professional.

### 3.5 Personnel continuity (open question)

If the user is incapacitated mid-build (illness, arrest, anything), there is currently no defined handoff plan for the codebase. The physical archival system handles data loss; it does not handle operator loss. Worth resolving before the build crosses the AWE-day checkpoint:

- One-page handoff document in the codebase repo, kept current weekly.
- "If you are picking this up cold, read these files in this order, run these commands, here is where I am, here is who to contact."
- A second person designated as picking up the work if needed (roommate, a Multiverse classmate, or a co-presenter).

This is filed in `research-backlog.md` (or this supplement) as a pre-AWE checklist item.

---

## 4. AWE-day demo relationship to the comprehensive build

See `01-build-order-reasoning.md` for the full logic. Summary:

- One codebase, one comprehensive build sequence per `build-plan.md`.
- AWE-day demo is a checkpoint within that sequence, reached at end of Week 2.
- The AWE-day plan (`03-awe-day-plan.md`) identifies which subset of the comprehensive plan must be functional by June 15, 2026 (flight day) plus the operational AWE-day-of tasks.
- No codebase fork. Planning docs are on different branches (`main` and `awe-day`) of the existing `AlleyTill/brecha-networks-planning` repo so the difference between "ship eventually" and "ship for AWE" is visible as a branch diff.

The codebase repository `AlleyTill/brecha-networks` (separate from the planning repo) will be created during Week 1 and remain a single repo with no demo fork.

---

## 5. Enterprise stress test scheduling

The user has been offered an enterprise-level stress check valued at $2-3K, by a respected professional in the field. The offer is to test the platform for bugs and load-handling under realistic conditions.

**Scheduling decision:** the stress test runs post-AWE, once the comprehensive feature set is in place. AWE-day usage at ≤50 concurrent attendees on a narrowly-scoped MVP does not require external stress testing. Spending the offered service on a 5-feature MVP would waste the value of the test; running it on the full feature set (working groups, comments, search, geocoded queries, notifications) gives a meaningful return.

**Replaces:** the previously-budgeted $300-500 freelancer reserve for Week 7 bug help. That budget line can be retired or reallocated; the offered stress test covers the bug-discovery use case at higher quality.

**Pre-test prep:** before the stress test runs, ensure the codebase has:

- A clean README and architecture sketch for the tester.
- Documentation of expected behavior and known limitations.
- A test environment that can be torn down without affecting production.
- A clear scope for the test (what to look for, what to ignore).

---

## 6. AWE roundtable co-host context

The Healthcare Roundtable at AWE USA 2026 is a 60-minute session on June 17, 2026, 3:30-4:30 PM, Room 103. Three tables, three hosts, rotating attendees:

- **Table A:** José Ferrer Costa — *Beyond the Pilot: XR in Real Healthcare*
- **Table B:** Alley Till — *Global Telehealth: Gaps and Partnerships* (the user)
- **Table C:** Robin White Owen — *XR Education and Leadership*

Format: 5-minute welcome → three 15-minute rotations (with 1-minute transitions) → 5-10-minute wrap-up where each host shares one recurring barrier, one practical solution, and one takeaway.

Each table uses a "Healthcare Innovation Barrier Map" poster + sticky notes by default. The same three prompts apply at all tables: *lived experience*, *biggest barrier*, *solution/workaround*.

**Brecha's potential role at the session:** as a replacement or supplement for sticky notes at Table B specifically. Attendees scan a QR code, signup via magic link on mobile, post their barrier per the prompts, see other posts. Host (Alley) reads the wrap-up directly from the platform's filtered view of barriers posted that day.

**Open question on session reach:**

If José and Robin are willing, Brecha could serve all three tables instead of just Table B. Three tables × ~30 attendees per rotation cycle = potentially ~90 unique users by session end. This is the AWE-day user target met within a single 60-minute session. Each table's prompts map to the same Brecha Barrier model; the topic difference (telehealth vs XR adoption vs XR education) is reflected in the barrier's free-text description, not in a schema change.

Pitch language for José and Robin (suggested):
*"Wrap-up summaries get easier if attendees post their barriers into a shared platform instead of sticky notes. I'll have a QR code at my table; happy to put one at yours too. Same three prompts, same five-minute signup. At wrap-up we read recurring patterns directly from the screen."*

This pitch belongs in the AWE-day plan as a pre-AWE outreach task.

---

## 7. Post-AWE roadmap notes (additions)

These additions are not yet in the locked `build-plan.md` post-AWE roadmap. Capturing them here for future migration.

### 7.1 Stress test (v1.0 hardening, immediately post-AWE)

- Schedule the $2-3K enterprise stress test for the week immediately following AWE.
- Address findings before opening platform to wider audience.
- Document results in a `STRESS-TEST-RESULTS.md` file in the codebase repo for transparency and reference.

### 7.2 Personal-protection legal counsel in destination country (v1.0 ops)

- Identify and consult a lawyer in the destination country who handles asylum, refugee, or threatened-founder cases.
- This is a separate budget line from the platform legal consult.
- Earliest action: once destination is committed. Document the consult in private notes only; do not record on the planning repo.

### 7.3 SNOMED category list (revisit)

The 2026-05-12 conversation surfaced that the original locked plan's "15-25 curated SNOMED-aligned categories" was an opinionated UX choice to avoid overwhelming users with the 352K+ SNOMED concept hierarchy. For the narrow AWE-day MVP, categories are not needed: users post free-text barrier title + summary + description; categorization is not part of the demo flow.

For the comprehensive build, the curated category list serves the filter/browse UX:

- Without categories, barriers are searchable by keyword only.
- Without categories, working groups cannot form around shared categories.
- Without categories, faceted search ("show me all barriers in cardiology") doesn't function.

**Open question:** revisit the curated category list once the AWE-day requirements are committed and the data model is in place. Decide whether to lock 15-25 categories or to defer categorization to a different mechanism (free-text tags + cleanup, autocomplete from existing tags, etc.) for the comprehensive build.

This is added to `research-backlog.md` as a deferred decision.

### 7.4 Rebecca Wagner's translation mistakes corpus (v1.5)

The user is in contact with Rebecca Wagner, a healthcare translation specialist with a documented body of work on common translation errors in medical contexts. When digitized, the corpus becomes a reference layer for the post-AWE Spanish UI launch.

**Use:** wrap around the DeepL translation service. Glossary of healthcare terms with locked translations; pre-translation prompt that flags known false cognates; post-translation review checklist. Does not require changing the locked DeepL service.

**Status:** Rebecca's book is not yet digitized. Form factor (PDF, Word, physical) and licensing not yet confirmed. Filed in `research-backlog.md` as a parallel research task.

---

## 8. Memory and continuity notes

### 8.1 What the user is building

Brecha Networks is a coordination platform for healthcare professionals and developers working on cross-jurisdictional access barriers. It is a Django + HTMX + Alpine.js + Tailwind + PostgreSQL/PostGIS web application. It is **not** an AI agent system, **not** a patient platform, **not** a Streamlit data app, **not** an ML/embeddings platform. The Mexico-core mission is the platform's framing priority; the audience is professionals, not patients.

The predecessor project, Barrier Registry, was an AI-agent-based top-down policy scraping system that was archived in late April 2026 after being deemed legally infeasible for a solo builder. Brecha Networks inverts the model: verified professionals contribute the barriers they actually encounter.

Memory entries from before late April 2026 that describe an "AI agent system" or "SALT belief layer" or "CrewAI / Anthropic SDK" or "Qdrant vector store" refer to Barrier Registry, not Brecha Networks. Those entries are archived and should not inform current Brecha decisions.

### 8.2 Source of truth precedence

In order of precedence:

1. `github.com/AlleyTill/brecha-networks-planning` — the planning repo, branch `main`.
   - `decisions-locked.md` (architectural)
   - `build-plan.md` (week-by-week)
   - `role-taxonomy.md` (42 roles + Other)
   - `research-backlog.md` (parked tactical to-dos)
2. This supplement (`02-main-planning-supplement.md`), `01-build-order-reasoning.md`, `03-awe-day-plan.md`, and `04-session-recap-2026-05-12.md` — captures 2026-05-12 session additions until merged.
3. Memory files in `C:\Users\Alley\.claude\projects\C--Users-Alley\memory\` — point-in-time observations, may be stale.

When in doubt, check the planning repo's `decisions-locked.md` change log for the authoritative timeline.

### 8.3 Tooling environment

- Development: Docker container with Claude in "ultimate sandbox mode" + VS Code on the laptop.
- Editor: VS Code (Cursor alternative available but not adopted).
- Version control: Git, GitHub (planning repo public, codebase repo will be created Week 1, may be private until ready).
- Codebase repo URL: `AlleyTill/brecha-networks` (to be created).
- Planning repo URL: `AlleyTill/brecha-networks-planning` (exists; default branch `main`).
- Laptop spec: 6K beast, 64GB RAM, 2TB storage (sufficient for local dev and local AI model use).
- Local storage available for AI models (Hugging Face), datasets, and curriculum archives. These are reference assets, not production dependencies.

### 8.4 Sustained pace assumptions

- 14 hours per day available, every day, until departure for AWE (~34 days from 2026-05-12).
- Cap at 12 hours/day with one rest day per week for sustainability.
- Hours 11-14 reserved as buffer for high-demand days, not as routine.
- Psychiatrist roommate monitors cognitive state and physical welfare in real time.
- Adderall protocol is intentional and supported. External checks do not rely on user self-assessment of fatigue.

---

## 9. What gets committed and where

This supplement and its companion files belong in the planning repository at `AlleyTill/brecha-networks-planning`:

- `01-build-order-reasoning.md`
- `02-main-planning-supplement.md` (this file)
- `03-awe-day-plan.md`
- `04-session-recap-2026-05-12.md`

Branch strategy:

- `main` branch — comprehensive plan. All four files committed.
- `awe-day` branch — slimmed copy for demo execution. Same four files, with the comprehensive content de-prioritized so the AWE-day plan is the operational focus on this branch.

Once the items in this supplement are integrated into `decisions-locked.md` (archival protocol, security additions, threat model section) and `build-plan.md` (Week 1 admin MFA addition, Week 5 logging and key custody additions), this supplement file can be marked archived or retired.

---

## 10. Open items not yet resolved

These belong in `research-backlog.md` once that file is updated:

- SNOMED category list — defer or lock 15-25, see section 7.3.
- Rebecca Wagner translation corpus digitization — see section 7.4.
- Personnel continuity plan / handoff doc — see section 3.5.
- Co-host pitch to José and Robin re: Brecha at all three tables — see section 6.
- Personal-protection legal counsel identification in destination country — see section 7.2.
- Username/identity at signup display convention for AWE-day demo (pseudonymous vs real-name in-person) — see AWE-day plan section.

End of main planning supplement.
