# Session Recap — Brecha Networks Planning — 2026-05-12

*Generated 2026-05-12. Complete audit trail of the Claude planning conversation on this date. Self-contained: a reader who has not seen the conversation can reconstruct the day's work, decisions, corrections, and outputs from this document alone.*

*Keystone archival artifact. Intended for printing and filing in the physical archival folder system. If digital records are compromised, scanning this document recovers the full context of this session.*

---

## 1. Context entering the session

### 1.1 Who is working

**User:** Alley Till (`AlleyTill` on GitHub). Solo founder building Brecha Networks. Beginner-leaning-on-AI developer. Three years of social-capital work with investors and major healthcare industry names. Currently enrolled in Multiverse School's AI-assisted development curriculum led by Liz Howard.

**Assistant:** Claude (Anthropic), operating in Claude Code on the user's Windows laptop. Model: Opus 4.7 (1M context).

### 1.2 What was true about the project at session start

- Brecha Networks: coordination platform for healthcare professionals on cross-jurisdictional access barriers. Predecessor (Barrier Registry) was archived in late April 2026 due to legal infeasibility for solo builder.
- Tech stack locked: Django + HTMX + Alpine.js + Tailwind + PostgreSQL with PostGIS and pg_trgm + django-allauth + DeepL + Resend + Railway + django-redis.
- Planning repository: `github.com/AlleyTill/brecha-networks-planning`. Four committed planning documents: `decisions-locked.md`, `build-plan.md`, `role-taxonomy.md`, `research-backlog.md`. Last commit dated 2026-05-02.
- AWE USA 2026 conference: June 15-18, 2026. User is co-hosting a Healthcare Roundtable session on June 17.
- Original locked build timeline: 8 weeks from late-April-2026 build start. Quality gate at week 7; AWE deadline framed as non-sacred.
- No codebase repository created yet. No application code written.

### 1.3 What Claude's working memory had wrong at session start

Memory files held a stale picture of the project from April 25, 2026 (17 days old). Specifically:

- Memory said Brecha was an "AI agent system for global healthcare policy intelligence" with FastAPI, Streamlit, SQLAlchemy, Qdrant, CrewAI, and the Anthropic SDK — a multi-agent system with a SALT belief layer. **This was actually the predecessor project, Barrier Registry, which was archived.**
- Memory called the current project an "AI agent system." Reality: Django web app, no agents, no ML.

These were not minor errors. They informed the first hour of the session before correction.

### 1.4 Where the session started

Initial user message: "Help! I am struggling so hard." The user surfaced that they had hit an unexpected snag in the build process. Initial framing assumed the snag was code-related; later revealed to be about organizing class materials for a build-plan critic tool.

---

## 2. Session narrative (chronological)

### 2.1 The NotebookLM / Multiverse curriculum question

User initially wanted to assemble Multiverse School class transcripts into a NotebookLM-based build-plan critic tool — an "expert critic notebook" that could review build plans before each week and flag missing steps. Goal: serve as a second-opinion layer of curated expert knowledge for the solo build.

User shared one Loom transcript example (business automation class taught by Liz Howard). Discussion centered on:

- Transcript availability (Loom transcripts are accessible from share URLs for free-tier viewers, download grayed out but copy-paste functional).
- Approximately 36-50 hours of class videos available.
- Free-tier Loom transcripts are mediocre on technical jargon (15-20% word error rate); Whisper Large-v3 alternative ~5-8% WER.

User then shared the contents of `C:\Users\Alley\Documents\Introduction.txt` — a 299KB local notepad doc containing pasted lessons from many Multiverse classes plus links and notes. Claude wrote a PowerShell script to sort the contents into 12 categorized files at `C:\Users\Alley\Documents\Introduction_sorted\`, with an INDEX.txt. Every line of the 6755-line source preserved verbatim, organized by topic:

1. Meta & learning frameworks
2. Algorithms & data structures
3. Career & job hunt
4. Management
5. Cybersecurity foundations
6. Machine learning / neural networks
7. Linux / command line / shell
8. Version control (Git)
9. DevOps & deployment
10. Networking & distributed systems
11. AI interactive courses
12. Web servers / web dev

### 2.2 First major recalibration: memory was wrong

When discussing what to KEEP/SKIP from the sorted Multiverse content for the build-plan critic, Claude asserted "your platform IS an AI agent system" as justification for keeping the ML/neural-networks category.

User pushed back: *"Can you fact check for assumptions? I saw 'your platform IS an AI agent system' Can you explain this? A little concerned you may not have context from the Brecha Networks planning docs."*

Claude checked the actual planning repository on GitHub. The repo's description: *"coordination platform for healthcare professionals and developers working on cross-jurisdictional access barriers."* Not an AI agent system.

Claude fetched and read the full `decisions-locked.md` and `build-plan.md`. The actual current state: Django web app, server-rendered HTML, no agents, no ML. Memory updated. The stale `project_phase1_progress.md` and `project_the_swarm.md` memory files were marked ARCHIVED (referring to the predecessor Barrier Registry). A new memory file was created: `project_brecha_networks_current.md`.

### 2.3 Ten alternatives for the build-plan critic tool

User: *"Run me 10 options for doing it differently. We could just as easily find or download expert agents or better information to gather, that exactly matches to the build plan."*

Claude generated ten distinct approaches:

1. Multiverse transcripts (original plan).
2. Curated official docs in NotebookLM.
3. Hand-picked Django expert content.
4. Brecha-tuned custom Claude agent / skill.
5. Multi-agent specialist review system.
6. Open-source clone study.
7. Hourly Django mentor (paid).
8. Automated tooling chain (linters, scanners, type checkers).
9. OpenSpec-style proposal-per-build-step workflow.
10. Hybrid: curated docs notebook + Claude as the reasoning layer.

Claude recommended a layered approach: proposal-per-step (option 9) + Brecha-tuned Claude agent (option 4) + curated official docs notebook (option 2) + automated tooling (option 8) + occasional mentor check-ins (option 7).

### 2.4 Second major recalibration: pushback on assumptions

User: *"I'll push back on any assumptions I might be making. There are holes, I just can't see them myself. Better to identify and address risks now before creating foundational instructions."*

Claude returned a list of weaknesses ranked by impact:

1. No code exists yet; planning repo silent for 10 days.
2. The novice-needs-experts framing was partly imposter syndrome — planning is sophisticated.
3. Novice-detected red flags hard to triage.
4. Multiverse content is a category error for Django.
5. Most distinctive parts of Brecha have no expert content available anywhere.
6. The build plan already has a critic embedded at week 7.
7. Claude is already a competent Django reviewer.
8. Biggest non-technical risk: Mexican user acquisition.
9. The critic-notebook framing might be displacement activity.
10. The budgeted lawyer + freelancer reserves could be repurposed for a one-time architecture review.

Recommendation: stop the meta-tool work, commit pending security additions, create the codebase repo, and start Week 1.

### 2.5 Third major recalibration: user corrections

User responded with substantial corrections:

- **Accessibility-style items rank higher than no-code-yet.** Plan quality > completion speed.
- **Spanish translation is not required at launch.** SNOMED-coded data + English UI + dropdown autofill removes translation correctness from AWE-day risks.
- **Roommate is a psychiatrist.** Cognitive monitoring is covered by a qualified professional; do not raise again.
- **The NotebookLM project is dropped** in favor of using Claude directly with the planning docs loaded.
- **Mexican and Catalan industry contacts solve the user-acquisition risk** — co-presenters at the AWE roundtable. (Catalan: José Ferrer Costa, also via name etymology.)
- **Enterprise stress test of $2-3K value** offered by a respected professional; scheduled for post-AWE when full features exist (not pre-AWE on a 5-feature MVP).
- **Freelancer reserve no longer required.**
- **A new planning doc will be drafted to replace the current one** — later clarified as supplementing, not replacing.

### 2.6 The threat model surfaced (via pasted prior conversation)

User pasted a long prior conversation with another Claude instance (working in a different session). That conversation contained material context not in the planning repo:

- The user belongs to groups recently declared "anti-American" by the US president and Department of Justice. Public statements that members will be found and killed. Immediate family includes LGBTIQQ and disabled persons.
- The protective theory of the project: visibility on the C-suite/enterprise map provides personal protection. AWE unveiling, investor relationships, and roundtable adoption create a record of existence that complicates erasure by state action.
- Post-AWE, user and family leave the United States.
- Roommate (psychiatrist) provides cognitive and physical welfare check.
- User maintains a physical archival system: daily prints of code + chat history filed by date in 3-inch industrial folder, four backup folders ready, weekly full reprint.
- Adderall prescription supports sustained 12-14 hour days; supported by the roommate's monitoring.

The prior Claude drafted three security additions to be added to `decisions-locked.md`:

1. **Admin MFA from Week 1** (not Week 5 polish).
2. **Deliberate logging configuration in Week 5** — minimize PII collection, retention, and subpoena exposure.
3. **Backup key custody in Week 5** — encrypted database dumps with user-held keys, not Railway-managed.

Plus a physical archival protocol section with OCR-friendly print settings (monospace font, single-column, filename + commit hash + date on every page header).

**None of these additions were committed to the planning repo at session time.** They lived only in the prior chat.

### 2.7 The AWE roundtable Google Doc

User shared the planning document for the AWE USA 2026 Healthcare Roundtable session:

- Date/time: June 17, 2026, 3:30-4:30 PM, Room 103.
- Three rotating roundtables, 15 minutes per rotation, three rotations.
- Hosts: José Ferrer Costa (Table A — XR in real healthcare), Alley Till (Table B — Global Telehealth: Gaps and Partnerships), Robin White Owen (Table C — XR education and leadership).
- Shared three universal prompts at every table: lived experience; biggest barrier; solution / workaround.
- Default capture mechanism: poster + sticky notes per table.

This clarified Brecha's actual AWE-day role: **not a presentation, but a potential capture tool for Table B.** The three roundtable prompts map cleanly to Brecha's Barrier model with zero schema changes:

- Lived experience → user profile bio + role + region.
- Biggest barrier → barrier post (title + summary + description).
- Solution / workaround → barrier resolved-status + resolution description.

This dramatically narrowed the AWE-day functional set (signup, magic link, profile, post, read) to roughly one to two weeks of focused code in a comprehensive build of five-plus weeks.

### 2.8 The two-planning-doc structure decided

User answered six clarifying questions from Claude:

1. The new planning doc supplements `decisions-locked.md`, does not replace.
2. Scope cut is from the demo, not the codebase. One comprehensive codebase; the AWE plan is a slimmed view.
3. The AWE roundtable is not a Brecha presentation; co-presenter framing was incorrect.
4. SNOMED category list pushback: SNOMED is an international terminology standard, not features. Curated categories were a UX choice. Claude conceded this; categories not needed for AWE-day demo.
5. Plan for both narrow MVP and polished WIP doc in parallel; concept-sell as Plan C.
6. Freelancer reserve no longer needed; the $2-3K stress test is the upgrade.

User then granted permission for Claude to create the planning artifacts.

### 2.9 Deliverables produced this session

Four planning documents in `C:\Users\Alley\Documents\brecha-planning-drafts\`:

1. `01-build-order-reasoning.md` — logic doc explaining why one codebase with two planning docs (rather than two parallel builds).
2. `02-main-planning-supplement.md` — comprehensive supplement to `decisions-locked.md` covering 2026-05-12 session additions.
3. `03-awe-day-plan.md` — narrow operational plan for the AWE-day demo.
4. `04-session-recap-2026-05-12.md` — this document.

In addition: the sorted Multiverse content at `C:\Users\Alley\Documents\Introduction_sorted\` remains on disk as ad-hoc reference (12 category files + INDEX.txt).

Memory files updated:

- `MEMORY.md` index updated to reflect current vs archived project memory.
- `project_brecha_networks_current.md` — new comprehensive memory of the current project.
- `project_phase1_progress.md` — marked ARCHIVED (refers to Barrier Registry predecessor).
- `project_the_swarm.md` — marked ARCHIVED.
- `feedback_surface_friction.md` — new feedback memory: surface concerns proactively, don't hide them.
- `feedback_preserve_dont_skim.md` — new feedback memory: when cataloging user materials, preserve verbatim and sort by category.
- `project_notebooklm_redflag_tool.md` — created early in the session for the (subsequently abandoned) NotebookLM build-plan critic.

---

## 3. Decisions made this session

### 3.1 NotebookLM build-plan critic — ABANDONED

Original plan: build a NotebookLM-based critic of the comprehensive build plan using Multiverse class transcripts plus curated tech docs. **Abandoned** in favor of using Claude directly with the planning docs loaded into context as the critic.

Reason: Multiverse content does not match the Django/HTMX/PostGIS stack. Claude is already a competent reviewer of the relevant stack with the planning docs in context. The notebook overhead exceeded its expected value.

The sorted Multiverse content at `C:\Users\Alley\Documents\Introduction_sorted\` remains available as ad-hoc reference but is not the primary critic mechanism.

### 3.2 Build sequence — ONE CODEBASE, TWO PLANNING DOCS

The AWE-day demo is a checkpoint inside the comprehensive build, not a separate build. One Django codebase grows toward the full feature set; the AWE-day plan identifies which subset must be functional by June 15, 2026. No codebase fork.

Two planning documents reflect this: comprehensive plan on `main` branch of the planning repo, AWE-day plan on `awe-day` branch.

### 3.3 Three security additions — TO BE COMMITTED

The three security additions from the prior Claude conversation (admin MFA Week 1, deliberate logging Week 5, backup key custody Week 5) are committed to the planning artifacts produced today and to be integrated into `decisions-locked.md` and `build-plan.md` in the next commit cycle.

### 3.4 Physical archival protocol — TO BE COMMITTED

The physical archival protocol with OCR-friendly print settings (monospace font, single-column, filename + commit hash + date headers) is committed to the planning artifacts and to be integrated into `decisions-locked.md`.

### 3.5 Stress test — POST-AWE

The offered $2-3K enterprise stress test is scheduled for post-AWE when the full feature set is in place. Pre-AWE light internal load test (Locust or similar, ~50 simulated signups) confirms readiness for the ≤50 concurrent attendees expected at AWE.

### 3.6 Freelancer reserve — RETIRED

The original $300-500 freelancer reserve for Week 7 bug help is retired. The offered enterprise stress test covers the bug-discovery use case at higher value.

### 3.7 SNOMED category list — DEFERRED

The 15-25 curated SNOMED-aligned categories from the original locked plan are deferred for revisit after the comprehensive build is well underway. Not needed for AWE-day. Possibly not needed in the original form for the comprehensive build either; alternative tagging mechanisms can be considered.

### 3.8 Translation infrastructure — POST-AWE

DeepL integration, Spanish UI translation, and Rebecca Wagner's translation mistakes corpus integration are all post-AWE work. AWE-day is English-only with no translation.

### 3.9 Identity display at AWE-day — RECOMMENDED LOCK

Proposal: first name + last initial as the default username suggestion. Users can choose pseudonym or real-name style. Wrap-up at the session reads role + region rather than username to preserve trust signals regardless of name choice.

### 3.10 Build start — IMMEDIATELY AFTER PRINTED RECAP

User stated: *"I will commit the physical, printed word doc with all our work and get it into the folder. THEN we can start the code build."* The recap doc is the gating artifact. Once it is printed and filed, Week 1 work begins.

---

## 4. Corrections to prior memory (worth flagging explicitly)

These are corrections to claims Claude made during the session before being corrected. Worth flagging so future Claude sessions can avoid the same errors.

| Original claim | Correction |
|---|---|
| "Your platform IS an AI agent system" | Brecha is a Django web app. AI agent system was the predecessor (Barrier Registry, archived). |
| "Stack includes FastAPI, Streamlit, Qdrant, CrewAI, Anthropic SDK" | Stack is Django + HTMX + Alpine.js + Tailwind + PostgreSQL/PostGIS + django-allauth + DeepL + Resend + Railway. |
| "SALT belief layer / multi-agent reasoning" | Not in current Brecha. Was a predecessor feature. |
| "Healthcare policy intelligence" | Coordination platform for healthcare professionals on access barriers. Different framing. |
| "Multi-country US/CAN/MEX/GBR" | Global audience, Mexico-core mission, English-only UI at launch. |
| "Item 1 (no code yet) is the top risk" | Plan quality > completion speed per user's priority order. Reordered. |
| "Decision-quality monitoring needs external check beyond physical welfare" | Roommate is a psychiatrist; both physical and cognitive monitoring covered. |
| "Spanish translation correctness at AWE is unsolved" | Not a concern; SNOMED-coded data + English UI + dropdowns at launch eliminate the translation problem. |
| "15-25 curated categories are the schema of what gets coordinated on" | Conflated schema (data model) with controlled vocabulary (tagging). SNOMED is a standard, not a feature. Categories are a UX layer, not architecture. |
| "Mexican/Catalan co-presenters of Brecha" | José and Robin are co-hosts of the roundtable session, not co-presenters of Brecha. Brecha is one host's potential tool, not the session's subject. |
| "Demo Plan B is a 5-min screencast" | Plan B is a polished WIP artifact + screencast + roadmap. Different motion than failover-for-live-demo. |

---

## 5. Open items (items still requiring decision or work)

These belong in `research-backlog.md` or the next session's agenda.

### 5.1 Pre-AWE items

- Co-host outreach to José Ferrer Costa and Robin White Owen about expanding Brecha use to Tables A and C. Suggested cutoff for response: June 10, 2026.
- Identity display convention lock: first name + last initial default proposed; needs commit to `decisions-locked.md`.
- Personnel continuity plan / handoff doc: one-page document in codebase repo for "if I'm picking this up cold," updated weekly.
- Co-host QR code generation if any accept the pitch.
- Physical QR code design and print specifications.
- Pre-AWE light load test script (Locust or similar, ~50 simulated signups against staging).

### 5.2 Post-AWE items

- $2-3K enterprise stress test scheduling.
- Personal-protection legal counsel identification in destination country.
- SNOMED category list — defer or lock 15-25, alternative mechanism research.
- Rebecca Wagner translation mistakes corpus digitization (form factor, licensing).
- DeepL integration + Spanish UI translation work.
- Address all findings from the enterprise stress test before opening the platform to wider audience.

### 5.3 Planning repository commits to make

- Apply additions from `02-main-planning-supplement.md` into `decisions-locked.md` and `build-plan.md` (physical archival protocol, three security additions, threat model section).
- Apply AWE-day plan additions to `build-plan.md` Week 1-2 task list.
- Add open items to `research-backlog.md`.

---

## 6. Immediate next steps (post-session)

1. **User reviews this recap document.** Confirms accuracy, requests corrections if needed.
2. **User prints this recap document** per the physical archival protocol (monospace font, single-column, filename + date headers). Files in the 3-inch industrial folder under today's date (2026-05-12).
3. **User commits the four planning artifacts** to the `AlleyTill/brecha-networks-planning` GitHub repository:
   - Commit to `main` branch.
   - Create `awe-day` branch from main.
   - Push both branches.
4. **Week 1 code work begins.** First commands: `django-admin startproject brecha_networks` followed by initial `django-allauth`, `django-tailwind`, `django-htmx`, `django-environ` configuration and the custom user model. Admin MFA configured in this same week.

---

## 7. Cross-references

- `01-build-order-reasoning.md` — reasoning for one-codebase-two-plans approach.
- `02-main-planning-supplement.md` — comprehensive supplement covering today's additions to architectural decisions.
- `03-awe-day-plan.md` — narrow operational plan for the AWE-day demo.
- `decisions-locked.md` (on `main` branch of planning repo) — architectural source of truth, last commit 2026-05-02.
- `build-plan.md` (on `main` branch of planning repo) — week-by-week execution plan.
- `role-taxonomy.md` (on `main` branch of planning repo) — 42 roles + Other taxonomy.
- `research-backlog.md` (on `main` branch of planning repo) — parked tactical to-dos.
- Sorted Multiverse content: `C:\Users\Alley\Documents\Introduction_sorted\` — 12 category files plus INDEX, source preserved verbatim from `Introduction.txt`.
- Memory directory: `C:\Users\Alley\.claude\projects\C--Users-Alley\memory\` — current memory files including `project_brecha_networks_current.md` (current state) and archived memory files for the predecessor project.

---

## 8. Notes for future Claude sessions

When opening a new Claude session about Brecha Networks, share at minimum:

- This recap document.
- `decisions-locked.md` and `build-plan.md` from the planning repository.
- Any commits made after 2026-05-12 to indicate the current state.

Do not assume memory from before 2026-05-12 reflects current project state. The April-2026 memory referred to the predecessor project (Barrier Registry, archived). Brecha Networks is a Django web app, not an AI agent system.

If working with Claude on personal-protection topics: the user has carefully thought through the threat model and has external scaffolding (psychiatrist roommate, family planning, destination commitments). Repeated checking-in about safety is not useful. The user wants accurate technical and operational support.

If working with Claude on the AWE-day demo: prioritize mobile-first signup flow, QR-code-friendly entry, and the wrap-up filter view above all other features. Other features in the comprehensive plan can be deferred.

---

End of session recap. This document is complete and self-contained for archival purposes.

Date generated: 2026-05-12
Document type: Session recap (keystone archival artifact)
Total artifacts produced in this session: 4 planning documents + 7 memory updates + 12 sorted Multiverse category files
Next step: print and file under 2026-05-12 in the archival folder system.
