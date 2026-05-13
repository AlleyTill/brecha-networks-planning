# AWE-Day Plan — Brecha Networks

*Generated: 2026-05-12. Narrow operational plan for the AWE USA 2026 Healthcare Roundtable, Table B, June 17, 2026, 3:30-4:30 PM, Room 103. Self-contained: a reader of this document has the full picture for AWE-day demo execution.*

---

## What this plan covers

This document is the operational plan for the Brecha Networks AWE-day demo. It defines:

- The minimum platform functionality required for AWE-day use.
- The pre-AWE checklist (what must be ready before June 15 flight).
- Plan A (live working demo), Plan B (polished WIP artifact), Plan C (verbal concept-sell).
- The wrap-up mechanism for the host.
- The pitch to co-hosts about expanding Brecha use to Tables A and C.
- AWE-day-of operational logistics.
- What is explicitly cut from AWE-day scope.

This plan is a subset of the comprehensive build sequence in `decisions-locked.md` and `build-plan.md`. It does not introduce any new architectural decisions; it identifies which subset of the comprehensive plan must be functional by the AWE-day checkpoint and what must happen operationally on the day.

For the reasoning behind the one-codebase-two-plans approach, see `01-build-order-reasoning.md`.

---

## 1. Session context

### 1.1 The roundtable

- **Event:** AWE USA 2026
- **Session:** Healthcare, Scaling and Solutions
- **Date:** June 17, 2026
- **Time:** 3:30 PM – 4:30 PM (60 minutes)
- **Room:** 103
- **Format:** Three rotating roundtables with shared welcome and wrap-up

### 1.2 The three tables

| Table | Host | Topic |
|---|---|---|
| A | José Ferrer Costa | Beyond the Pilot: XR in Real Healthcare |
| B | Alley Till (user) | Global Telehealth: Gaps and Partnerships |
| C | Robin White Owen | XR Education and Leadership |

### 1.3 The session flow

1. Shared welcome (5 minutes) — three hosts introduce themselves and the format.
2. Rotation 1 (15 minutes) — attendees at first chosen table.
3. Transition (1 minute).
4. Rotation 2 (15 minutes).
5. Transition (1 minute).
6. Rotation 3 (15 minutes).
7. Shared wrap-up (5-10 minutes) — each host shares one recurring barrier, one practical solution, and one takeaway.

### 1.4 The three universal prompts at every table

- *What is your lived or professional experience with this topic?*
- *What is the biggest barrier you have encountered?*
- *What solution, workaround, or lesson helped you overcome it?*

These prompts map cleanly to Brecha's Barrier model:

- *Lived experience* → user profile bio + role + region
- *Biggest barrier* → barrier post (title + English summary + description)
- *Solution/workaround* → barrier resolved-status + resolution description (locked feature)

No schema changes are needed for Brecha to serve as the capture tool for this session's prompts.

---

## 2. AWE-day functional requirements

This is the minimum set of platform functionality that must be operational by June 15, 2026 (flight day) for Brecha to be used at Table B during the June 17 session.

### 2.1 Mobile-friendly signup with QR-code entry

- Physical QR code at Table B routes to `brechanetworks.com/signup?event=awe-2026` or similar event-tagged signup URL.
- Mobile-first signup form: email → 6-digit verification code → username + role (dropdown from 42-role taxonomy) + region (text field; Mapbox autocomplete optional for AWE-day) + brief bio (free text) + ToS agreement.
- Locked mobile-optimal HTML attributes from `decisions-locked.md`: `autocomplete="one-time-code"`, `inputmode="numeric"`, 16px+ font, 44px+ touch targets.
- Target signup completion time: under 90 seconds.

### 2.2 Magic link return logins

- For attendees who close the browser between rotations and return later, magic link login via email.
- Delivery via Resend with SPF/DKIM/DMARC configured on `brechanetworks.com`.
- Magic link landing page directs user to "Post a Barrier" view by default.

### 2.3 Minimal user profile

- Username + verified role + verified region + bio + list of their own posts.
- No avatar at AWE-day (use initials or color block); avatar upload deferred.
- No activity feed.
- No follow toggle, no connect button at AWE-day.

### 2.4 Post a barrier

- Form fields: title (required, 100 char), English summary (required, 280 char, 2-3 sentences), full description (required, free text), origin location (optional, free text), destination location (optional, free text for cross-jurisdictional barriers).
- No category tagging at AWE-day (categories revisit post-AWE per `02-main-planning-supplement.md` section 7.3).
- No status marker at AWE-day (defaults to "identified"; resolved-status feature deferred).
- No image upload.
- Successful submission redirects to the barrier's detail page.

### 2.5 Read other barriers from the event

- Public list view at `/barriers/` showing all barriers, most recent first.
- Filter or query parameter to limit to barriers posted during the event window (e.g., `/barriers/?event=awe-2026` or `/barriers/?since=2026-06-17`).
- Each barrier displays: title, English summary, author username + role + region, posted time.
- Clicking a barrier opens the detail page with full description.

### 2.6 Host-side wrap-up filter view

- Alley needs to skim all barriers posted at Table B during the session in time for the 4:25-4:30 PM wrap-up.
- Could be the same list view as section 2.5 with a date filter, or a separate admin view at `/admin/barriers/?event=awe-2026`.
- Sortable by recency.
- Visible from a phone or tablet (Alley will be holding the device during the session).

### 2.7 What is NOT required for AWE-day

The following features are part of the comprehensive build but are explicitly cut from the AWE-day demo:

- Working Groups (members M2M, join requests, threaded discussion)
- @mentions in comments
- Threaded comments
- Connect requests
- Follow Issue
- Email notifications (other than magic-link delivery)
- Person search via PostgreSQL `pg_trgm`
- Geographic proximity search via PostGIS
- Mapbox geocoding autocomplete (region can be free-text or simple dropdown)
- Status markers (identified / being worked on / resolved)
- Resolved-barrier achievement layer
- Multiple locations per user
- Three-way affiliation markers
- Direct messages
- Multi-language UI (English only — locked in `decisions-locked.md`)
- DeepL translation integration (post-AWE)
- Curated SNOMED category list (post-AWE decision)
- Plausible analytics (post-AWE)
- Cross-posting agent
- Real interactive pin map
- Verified credential badges

Each of these has a place in the comprehensive plan; none of them are required for AWE-day operation.

---

## 3. Pre-AWE checklist

Tasks that must be complete before the June 15 flight to AWE. Owner is the user unless otherwise noted.

### 3.1 Code

- [ ] Week 1 work complete: Django bootstrap, `django-allauth`, `django-tailwind`, `django-htmx`, `django-environ` configured. Custom user model with verified-pseudonymous fields. URL conventions locked. Base templates. Admin MFA configured and tested.
- [ ] Week 2 (early) work complete: `Barrier` model with title, English summary, full description, origin and destination location fields, author FK. Django admin configured for barrier review.
- [ ] Minimal signup flow tested on at least three mobile browsers (iOS Safari, Android Chrome, mobile Firefox).
- [ ] Magic link email delivery tested end-to-end. SPF/DKIM/DMARC configured on `brechanetworks.com`. Test delivery to Gmail, iCloud, Outlook, and at least one non-major provider.
- [ ] Barrier post form tested on mobile.
- [ ] Barrier list view tested on mobile and desktop.
- [ ] Host wrap-up filter view tested.
- [ ] Production deployment to Railway with PostgreSQL + PostGIS + pg_trgm verified.
- [ ] `brechanetworks.com` resolves to production deployment with HTTPS.
- [ ] Rate limiting on signup endpoint (anti-spam for the event window).
- [ ] GDPR data export feature available (management command).
- [ ] Privacy policy live at `brechanetworks.com/privacy/` via iubenda.
- [ ] Terms of service live at `brechanetworks.com/terms/`.

### 3.2 Physical materials for Table B

- [ ] Printed QR codes (high contrast, large format, at least 4-inch square) — multiple copies in case one is damaged.
- [ ] Backup QR code on phone screen as Plan B.
- [ ] Printed "Brecha Networks — Scan to Post Your Barrier" signage for the table.
- [ ] Printed instruction card (5-10 step signup walkthrough) for attendees who prefer paper instructions.
- [ ] Backup sticky notes + pens at the table for attendees who decline the platform.
- [ ] Charged phone or tablet for Alley to use the host-side wrap-up filter view.
- [ ] Backup device (second phone or laptop) in case the primary fails.
- [ ] Spare charger for both devices.

### 3.3 Plan B and Plan C materials

- [ ] Plan B screencast: 5-minute video showing signup → post → read flow on a test environment. Voiceover. Available on phone in airplane mode (downloaded, not streamed).
- [ ] Plan B printed one-pager describing what the platform does, with the QR code that resolves to the planning repo and a contact email.
- [ ] Plan C verbal pitch rehearsed: 60-90 second description of Brecha for attendees who ask "what is this?" if the platform is not live.
- [ ] Architecture diagram (one-page PDF) ready to share if asked.
- [ ] Roadmap one-pager ready to share if asked: comprehensive build sequence + what's done vs. what's in progress + post-AWE direction.

### 3.4 Co-host outreach

- [ ] Reach out to José Ferrer Costa with the co-host pitch (see section 5).
- [ ] Reach out to Robin White Owen with the co-host pitch.
- [ ] If either or both accept, prepare table-specific QR codes (`/signup?table=a` and `/signup?table=c`) and additional table signage. Tags on barriers allow the wrap-up filter to scope by table.

### 3.5 Operational hygiene

- [ ] Offline encrypted copy of the entire codebase, planning repo, credentials, and contacts on external media. Standard pre-flight task.
- [ ] Physical archival folder updated with the latest weekly full reprint.
- [ ] Roommate (psychiatrist) briefed on AWE-day schedule and contingencies.
- [ ] Travel-day operational details (flight times, accompanying persons, destination) recorded in private notes only — not on the planning repo.
- [ ] Demo Plan A/B/C decision tree rehearsed in advance: "if live demo is broken, switch to screencast within 2 minutes; if screencast unavailable, switch to verbal pitch within 30 seconds."

### 3.6 Stress test

- The offered $2-3K enterprise stress test is scheduled for **post-AWE**, not pre-AWE. AWE-day usage at ≤50 concurrent attendees on a 5-feature MVP does not require external stress testing.
- Pre-AWE: light internal load test using a script (50 simulated signups + posts via Python `requests` or a tool like Locust, run from the laptop against the production deployment in a low-traffic window). Confirms no obvious breakage at the expected attendee count.

---

## 4. Plan A / Plan B / Plan C

### 4.1 Plan A — Live working demo

- Brecha Networks is fully operational at `brechanetworks.com`.
- Attendees at Table B (and possibly Tables A and C) scan the QR code, sign up via magic link on mobile, post their barrier per the three prompts, see what others have posted.
- Host (Alley) uses the wrap-up filter view to read recurring patterns at minute 55-60.
- Platform handles 30-50 concurrent users without degradation.
- This is the goal. All pre-AWE work targets making Plan A achievable.

### 4.2 Plan B — Polished WIP artifact

- Code is partially functional but cannot survive 30-50 concurrent live users, or a critical signup flow is broken on mobile, or magic link delivery is unreliable.
- Switch to Plan B within 2 minutes of recognizing the failure.
- Present the printed one-pager, the screencast on a phone, and the architecture diagram.
- Use the table's poster + sticky notes as the actual capture mechanism for the session.
- After the session: collect sticky notes, transcribe to Brecha post-session as the first real content on the platform.
- Frame for attendees: "the platform launches in production after AWE; you've helped shape what gets posted first."

### 4.3 Plan C — Verbal concept-sell

- Code is not functional. Plan B materials are unavailable (laptop failure, missed screencast). Sticky notes only.
- Switch to Plan C within 30 seconds of recognizing Plan B is also unavailable.
- Use the 60-90 second verbal pitch describing what Brecha is and why it matters.
- Use the table's poster + sticky notes per the session's default format.
- The Mexican/Catalan co-presenter network connections and ongoing relationships carry the value of the AWE appearance even if the platform is not on display.
- Frame for attendees: "we're building the platform that turns these conversations into ongoing coordination — happy to connect after the session about how you might be involved."

### 4.4 Decision tree

```
Is brechanetworks.com loading correctly on a fresh mobile browser? 
  NO  → Plan B
  YES → Is signup completing successfully on a fresh phone? 
    NO  → Plan B
    YES → Is magic link arriving within 60 seconds? 
      NO  → Plan B (use direct signup; skip magic link demo)
      YES → Plan A. Begin session.
```

Run this check at 3:00 PM, 30 minutes before session start. If failure, there is time to switch deliberately rather than under pressure.

---

## 5. Co-host pitch

Pitch language for José Ferrer Costa and Robin White Owen, suggesting Brecha as a shared capture tool across all three tables.

### 5.1 Suggested message

> Hi [name],
>
> I'm working on a coordination platform called Brecha Networks for healthcare professionals on cross-jurisdictional access barriers. It launches around the AWE timeframe.
>
> For Table B on June 17, I'm planning to have attendees post their barriers directly into Brecha via a QR code instead of sticky notes — same three prompts (lived experience, biggest barrier, solution), but the content stays accessible afterward and the wrap-up summary gets easier to pull together.
>
> If you're open to it, I'd be happy to set up a parallel QR code for your table too. Same five-minute mobile signup, table-tagged barriers so you can pull your wrap-up summary directly from the screen. No work from your end other than placing a printed QR code on the table.
>
> If you'd prefer to stick with poster + sticky notes, totally fine — I'll only run my own table that way.
>
> Either way, looking forward to the session.

### 5.2 What to do with each response

- **Yes:** Prepare table-tagged QR codes (`/signup?table=a` for José, `/signup?table=c` for Robin). Update the wrap-up filter view to scope by table. Add their table to the printed signage list.
- **No:** Proceed with Table B only. Their tables continue with poster + sticky notes per the session default.
- **No response by ~June 10:** Default to no; do not press. The host has prerogative over their own table.

### 5.3 Why this matters

If both accept, three tables × ~30 attendees = ~90 unique signups by session end. This meets the AWE-day user target within a single 60-minute session. If only one accepts, ~60 signups. If neither accepts, ~30 signups from Table B alone — still useful.

---

## 6. AWE-day-of operational logistics

### 6.1 Morning of June 17, 2026

- 8:00 AM — verify `brechanetworks.com` is operational (load home page, signup page, post page on mobile).
- 9:00 AM — review the wrap-up filter view; confirm date filter is working with current date.
- 10:00 AM — pull the latest commit hash; printed copy of the commit hash filed in the archival folder.
- Early in the day — locate Room 103. Verify physical access. Test phone signal in the room (signup requires mobile data for first-time users without conference wifi).
- Charge all devices. Confirm spare chargers in the kit.

### 6.2 3:00 PM (30 minutes before session)

- Arrive at Room 103.
- Set up Table B: printed QR codes (multiple copies), table signage, printed instruction cards, backup sticky notes + pens.
- Run the Plan A / Plan B / Plan C decision tree (section 4.4).
- If running Plan A: open the wrap-up filter view on Alley's phone or tablet. Confirm it shows zero barriers (will fill during the session).
- If running Plan B: confirm screencast plays from phone in airplane mode.
- Greet José and Robin. Confirm whether their tables are using Brecha.

### 6.3 3:30 PM — Session start

- Shared welcome (5 minutes). Three hosts introduce themselves and the format.
- Alley's 45-60 second introduction (per session format): mentions Brecha as the capture tool for Table B (and possibly other tables).

### 6.4 3:35 PM, 3:51 PM, 4:07 PM — Three rotations

- 15-minute rotations.
- Attendees scan QR code at the start of their rotation, sign up while introducing themselves to the table, post their barrier in real time.
- Alley facilitates the discussion using the three prompts. Encourages but does not require platform use; sticky notes available for attendees who prefer paper.
- Mid-rotation (around minute 8 of each rotation), Alley glances at the wrap-up filter view to track what's being posted.

### 6.5 4:23 PM — End of third rotation

- Quick scan of the wrap-up filter view. Identify one recurring barrier theme across the three rotations.
- Identify one practical solution mentioned multiple times.
- Identify one key takeaway worth sharing.

### 6.6 4:25-4:30 PM — Shared wrap-up

- Each host (in turn) shares: one recurring barrier, one practical solution, one takeaway.
- Alley reads from the wrap-up filter view if Plan A; reads from sticky notes if Plan B/C.
- Networking continues informally after the formal close.

### 6.7 Post-session (4:30 PM onward)

- Photograph the table's poster (if used as supplement).
- Collect sticky notes for post-session transcription if running Plan B/C.
- Note attendee usernames who expressed continued interest; follow up via Brecha's notification system (post-AWE feature) or manually via email.
- Save the wrap-up filter view URL / state.
- Add a brief retro entry to the codebase repo's `notes/` directory (private channel) describing what worked and what to refine for future events.

---

## 7. Identity display decision for AWE-day

Open question from the 2026-05-12 conversation: should AWE-day signup display attendees pseudonymously (per Model D in `decisions-locked.md`) or encourage real-name display?

### 7.1 Considerations

- **Pseudonymous (Model D default):** preserves the platform's privacy posture, demonstrates the verified-role + verified-region trust signal. Wrap-up reads like "recurring barrier from BlueOwl42, RedFox13, and PurpleDolphin7." Less warm.
- **Real-name encouraged:** in-person session creates a context where attendees may prefer their actual names for cross-table networking. Wrap-up reads like "recurring barrier from Dr. Maria Garcia (clinician, Texas), James Lin (developer, California), and Sofia Reyes (policy, Mexico City)." Warmer. But contradicts the platform's locked privacy posture.

### 7.2 Recommendation

Allow attendees to choose at signup. Default to first name + last initial (e.g., "Maria G.") as a compromise. The username field can hold either a pseudonym or a real-name-style entry; the platform does not enforce one or the other. Verified role and verified region accompany the username regardless.

For the wrap-up: read the role + region rather than the username. "One recurring barrier from a clinician in Texas, a developer in California, and a policy professional in Mexico City..." This preserves the trust signal and respects whatever name choice the attendee made.

This decision should be locked before AWE-day and added to `decisions-locked.md`'s identity section.

---

## 8. What goes on which branch

This planning doc (`03-awe-day-plan.md`) is committed to **both** the `main` and `awe-day` branches of `AlleyTill/brecha-networks-planning`.

On `awe-day` branch:

- This document is the operational focus.
- The comprehensive `decisions-locked.md` and `build-plan.md` remain accessible.
- The `02-main-planning-supplement.md` is present as reference.

On `main` branch:

- This document is one of several planning artifacts.
- Comprehensive plan is the operational focus.
- AWE-day plan is referenced from `02-main-planning-supplement.md` section 4.

The branch separation makes the "what's for AWE" vs. "what's comprehensive" diff visible in the GitHub UI.

---

## 9. Items still requiring decision before AWE-day

- Identity display convention (section 7.2) — propose first-name + last-initial default, lock in `decisions-locked.md`.
- Co-host outreach response (section 5) — by approximately June 10, 2026.
- Personnel continuity plan / handoff doc (section 3.5 of `02-main-planning-supplement.md`).
- Final QR code design and print specifications.
- Test environment for the light pre-AWE internal load test.

End of AWE-day plan.
