# Build Plan — Brecha Networks

*Last updated: May 16, 2026*
*Companion files: `decisions-locked.md` (architectural source of truth), `research-backlog.md` (parked tactical to-dos + research prompts)*

---

## How to use this file

This document maps the 8-week build sequence to specific deliverables, hands-on tasks, outsourceable tasks, and embedded learning topics. Lessons exist because the next build step demands them — minimum viable knowledge, nothing more.

The plan is **living, not fixed.** Each week's lesson content gets refined just before you reach it, informed by what actually happened in the previous week.

---

## Strategic framing recap

Three principles inherited from `decisions-locked.md`:

1. **AWE deadline is non-sacred.** Goal = load-bearing platform. AWE 2026 is one window for the goal to land.
2. **Infrastructure-first.** Pre-AWE focus is rock-solid infrastructure. No fabricated content. Demo is the architecture itself if real users haven't onboarded.
3. **Quality gates over feature deadlines.** Week 7 gate decides outreach push vs. polish-only.

---

## AI-assistance gradient

Across the 8 weeks, AI involvement gradually decreases as your hands-on capability grows. **Not a hard cliff at any week — gradient.**

| Phase | AI involvement | What you do |
|---|---|---|
| Weeks 1-2 | AI writes most code | You read, run, and *predict-before-reveal* (describe what you think the code does in your own words before reviewing) |
| Weeks 3-4 | AI scaffolds | You fill in business logic by hand |
| Weeks 5-6 | You drive | AI is consultant ("explain this error" / "is this the cleanest way?") |
| Weeks 7-8 | You debug unaided where possible | AI helps only when stuck for >15 minutes |

The "predict-before-reveal" beat is the lightweight muscle-builder. Costs ~60 seconds per code chunk. Trains pattern recognition without forcing you to build everything from scratch.

---

## Make-a-thon protection rules

Make-a-thon prep (April 27 – June 5) and event week (June 8) coexist with the build:

- **Brecha Networks always has the morning slot (7:30am-2pm)**
- **Make-a-thon takes evenings and weekends**
- A whole day off the Brecha Networks schedule for Make-a-thon = no flag, no concern
- Two consecutive days off = flag at next session, schedule check-in
- Make-a-thon never claims morning hours without explicit acknowledgment from you

---

## Outsourcing framework

For each build step, ask: **can it be outsourced before or during AWE?**

- **YES** → don't deep-learn it. Delegate or follow tutorial steps, no mastery required.
- **NO** → must learn enough to debug at AWE.
- **Maybe** → learn enough to delegate intelligently.

Examples:

| Step | Outsourceable? | Implication |
|---|---|---|
| Initial Django + Tailwind setup | ⚠️ Sort of — but core to your daily work | Learn it. You'll touch it daily. |
| DNS configuration | ✅ Yes — one-time, follow tutorial | Don't deep-learn DNS. Just do steps. |
| PostGIS migration in week 4 | ✅ Yes — $200-400 contractor option | Learn enough to delegate; don't master. |
| Django ORM mental model | ❌ No — daily work | Must learn. Core to everything. |
| HTMX patterns | ❌ No — debug-essential at AWE | Must learn enough to debug. |
| Reading stack traces during demo | ❌ No — can't outsource at AWE | Must learn before demo. |
| Git rebase / cherry-pick / recovery | ✅ Yes — fumble through with AI | Defer. AI assistance covers it. |
| Email deliverability (SPF/DKIM/DMARC) | ✅ Yes — Postmark/Resend walks you through | Follow steps; no mastery needed. |
| Trademark filing | ✅ Yes — lawyer in week 5-6 | Don't try to file yourself. |

---

## Week-by-week breakdown

### Week 1: Project bootstrap

**Build deliverables:**
- Django project initialized with `django-allauth` (pinned to specific 65.x), `django-tailwind-cli` (standalone binary — no Node), `django-htmx`, `django-environ` configured
- Custom user model with verified-pseudonymous fields (username + role + region + bio + email)
- URL conventions locked from day 1 (plural collections, slug-based detail URLs)
- Base template structure (Tailwind + HTMX + Alpine.js loaded)
- Basic landing page + About page + signup page (skeleton)
- Maintenance verification of locked Django packages (confirm no major staleness)
- First barrier seeded into local dev database (your own — for testing only, NOT for production launch)

**Hands-on tasks:**
- Windows dev environment prep: enable long paths (`LongPathsEnabled=1` registry + `git config --global core.longpaths true`); add Windows Defender exclusions for `C:\dev\` and `python.exe`
- Create venv at `C:\dev\brecha-networks\.venv\`; pin all dependencies in `requirements.txt` (including `django-allauth` to a specific 65.x — silent removals in the 65.x line make auto-upgrades unsafe)
- Run Django startproject + startapp commands; understand what each file does
- Configure `django-allauth` Flow C signup
- Set up `django-tailwind-cli` (standalone Tailwind binary download; no `node_modules`)
- Configure admin MFA from day one (`django-otp` or `allauth` MFA) per the 2026-05-12 supplement threat-model decision — moved from Week 5 to Week 1
- First commit + push to new codebase repo (`AlleyTill/brecha-networks`)
- Tactical: domain purchase + handle reservations (parallel to coding)

**Outsourceable tasks:**
- Logo/icon initial draft (use a free SVG library; defer designer-led version)
- DNS pointing to dev environment (follow Railway/Render tutorial)

**Must-learn topics (with resources):**
- Python refresher: free resources at [Real Python](https://realpython.com) — focus on classes, decorators, list comprehensions (1 day)
- HTTP basics: [MDN HTTP overview](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview) (½ day)
- Django mental model: [official Django tutorial Parts 1-2](https://docs.djangoproject.com/en/5.0/intro/tutorial01/) (1 day)
- Django ORM basics: same tutorial, Part 2 (1 day)
- Git basics for daily work: add, commit, push, pull, branch, merge (½ day refresh; you've used it)

**Outsourceable / post-AWE-deferable:**
- Deep DNS knowledge (follow steps; no mastery)
- Django middleware internals (defer)
- Custom management commands (defer until needed)

---

### Week 2: Data model + admin + skeleton features

**Build deliverables:**
- `Barrier` model with title, slug, origin location (PostGIS point), destination location (optional point), English summary, full description, category tags (M2M to SNOMED-aligned categories), status marker, status description (required when status = resolved), author FK
- `WorkingGroup` model with members (M2M), description, threaded discussion FK to comment thread
- `Comment` model via `django-comments-xtd` integration
- `ConnectRequest` model via `django-friendship`
- `Follow` (Issue follows) via `django-friendship`
- Django admin configured for all models with appropriate filters and search fields
- 15-25 SNOMED-aligned categories seeded via management command (looked up via [SNOMED CT browser](https://browser.ihtsdotools.org))
- Running locally with seeded test data

**Hands-on tasks:**
- Write Django models with relationships
- Generate + run migrations
- Customize Django admin for moderation use cases
- SNOMED ID lookups via free browser (~3-4 hours of focused work)

**Outsourceable tasks:**
- Initial database design review (have AI sanity-check your schema)
- Admin UI customization patterns (copy from `django-comments-xtd` and `django-friendship` docs)

**Must-learn topics:**
- Django models + migrations + ORM relationships (ForeignKey, ManyToMany, OneToOne) — [Django models docs](https://docs.djangoproject.com/en/5.0/topics/db/models/) (1-2 days)
- Django admin customization — [Django admin docs](https://docs.djangoproject.com/en/5.0/ref/contrib/admin/) (½ day)
- PostGIS basics for geographic data — [GeoDjango tutorial](https://docs.djangoproject.com/en/5.0/ref/contrib/gis/tutorial/) (1 day, but only the parts you'll use)
- SNOMED browser usage (free, ~30 min tutorial on YouTube)

**Outsourceable / post-AWE-deferable:**
- SNOMED hierarchy traversal (defer; we just need IDs at launch)
- Custom admin actions for bulk operations (defer)

---

### Week 3: UI/UX polish + signup flow + responsive design

**Build deliverables:**
- Mobile-first responsive layouts using Tailwind
- Signup flow: email → 6-digit code → username/role/region/issue/ToS in same session
- Issue auto-suggest dropdown (queries existing barrier titles + category tags, sorts by name match + geographic proximity)
- Mapbox geocoding autocomplete on region + barrier location inputs
- HTMX patterns for inline updates (post creation, comment submission, follow toggle)
- Profile pages (minimum viable: username + role + region + bio + their posts)
- Barrier list + detail pages with status color dots, follower count, working group affordance

**Hands-on tasks:**
- Build signup wizard with mobile-optimal HTML attributes (`autocomplete="one-time-code"`, `inputmode="numeric"`, 16px+ inputs, 44px+ touch targets)
- Wire HTMX endpoints for follow/unfollow, post submission, comment submission
- Style with Tailwind utility classes
- Implement issue auto-suggest backend (PostgreSQL `pg_trgm` similarity + Mapbox proximity sort)

**Outsourceable tasks:**
- Visual design polish (defer to post-AWE designer; use Tailwind defaults at launch)
- Logo finalization (post-AWE)

**Must-learn topics:**
- Tailwind responsive utility classes (sm:, md:, lg:) — [Tailwind responsive design](https://tailwindcss.com/docs/responsive-design) (1 day)
- HTMX core patterns (hx-get, hx-post, hx-target, hx-swap) — [HTMX documentation](https://htmx.org/docs/) (1-2 days)
- Alpine.js for dropdowns and modals (½ day)
- Forms + validation in Django — [Django forms docs](https://docs.djangoproject.com/en/5.0/topics/forms/) (1 day)

**Outsourceable / post-AWE-deferable:**
- Advanced HTMX patterns (server-sent events, websocket extensions)
- Custom Tailwind plugins
- CSS animations beyond Tailwind defaults

---

### Week 4: Deploy

**Build deliverables:**
- Production deploy to Railway or Render (paid tier)
- PostgreSQL database with PostGIS + pg_trgm extensions enabled
- Custom domain (`brechanetworks.com`) pointing at production
- HTTPS verified (auto-handled by Railway/Render)
- `noindex, nofollow` meta tag + restrictive `robots.txt` (per Q9 lock for weeks 1-2; transition to allow indexing this week if architecture is stable)
- Email service connected (Postmark or Resend)
- Environment variables managed via `django-environ`
- Database backups verified working
- First production "smoke test" — sign up for an account end-to-end on production

**Hands-on tasks:**
- Configure Railway/Render project
- Set environment variables (API keys, DB URL, secret key)
- Configure DNS (point domain at Railway/Render)
- Migrate from SQLite to Postgres
- Test PostGIS + pg_trgm extensions are working
- Connect email service + verify deliverability

**Outsourceable tasks:**
- Initial DNS configuration (follow Railway/Render tutorial)
- Email deliverability setup (SPF/DKIM/DMARC walks you through)
- PostGIS extension enablement (one-line SQL command)

**Must-learn topics:**
- DNS basics: A record, CNAME, what propagation means (½ day, follow tutorial)
- Environment variable management (½ day)
- Migration from SQLite to Postgres (1 day — most differences are transparent, but a few gotchas)
- Reading deploy logs to debug deploy failures (1 day, ongoing)

**Outsourceable / post-AWE-deferable:**
- Custom CDN configuration (Cloudflare CDN — set up if traffic warrants in week 5)
- Database connection pool tuning (defaults are fine for launch)
- Monitoring/alerting beyond Railway/Render dashboards (defer)

---

### Week 5: Hardening + lawyer consult + demo outline

**Build deliverables:**
- Rate limiting on signup, login, post submission, comment submission (django-allauth provides defaults; `django-ratelimit` for custom rate limits if needed)
- Content moderation flow via Django admin (review flagged posts, suspend accounts)
- Email deliverability fully configured (SPF/DKIM/DMARC verified; test emails arrive in inbox not spam)
- Custom error pages (404, 500) styled with Tailwind
- GDPR data export feature (management command produces JSON of user's data on request)
- Account deletion flow (anonymizes user content per Q17 lock)
- Lawyer consult completed (~$300-500, agenda from `research-backlog.md` Tactical Follow-ups)
- Demo outline drafted (~1 hour — what story, what features, identify build gaps for week 6 polish)
- Pre-vet 2-3 Django freelancers for week 7 backup chain (15-min intro calls)

**Hands-on tasks:**
- Implement rate limiting middleware
- Build moderation queue view in Django admin
- Write data export management command
- Test email deliverability with [Mail Tester](https://www.mail-tester.com)
- Schedule + attend lawyer consult; bring agenda from `research-backlog.md`
- Write demo outline document

**Outsourceable tasks:**
- Email deliverability setup (Postmark/Resend walkthroughs)
- Templated GDPR privacy policy (Termly/iubenda — $5-15/mo)
- Lawyer consult itself (full delegation — you bring questions, they answer)

**Must-learn topics:**
- Reading stack traces (1 day, ongoing — debug-essential at AWE)
- Browser dev tools: Network tab, Console (½ day)
- Email deliverability concepts: SPF, DKIM, DMARC (½ day, follow tutorial)
- GDPR baseline requirements (1 hour reading)

**Outsourceable / post-AWE-deferable:**
- Advanced Django middleware internals (defer)
- Custom moderation tooling beyond Django admin (defer)
- Sentry / Datadog integration for error monitoring (consider in week 6 if budget allows)

---

### Week 6: Eco-sphere + branding finalization + accent color

**Build deliverables:**
- Final color palette decision (primary deep teal verified or alternative; accent color decided)
- Final font sizing pass (Tailwind type scale verified across breakpoints)
- Footer links to all social accounts (5-10 click discoverability requirement)
- About page + Contact page populated with brand voice copy
- Pinned "Launching at AWE" post drafted on each social account
- First Substack post drafted (scheduled for AWE day morning)
- Plausible analytics added
- Open Graph + meta SEO tags on all public pages
- Optional: Cloudflare CDN if traffic warrants

**Hands-on tasks:**
- Color hex finalization across Tailwind config + custom components
- Write About page + Contact page copy in brand voice
- Set up Plausible (paste tracking script in base template)
- Open Graph image creation (use [meta tags](https://metatags.io) preview tool)

**Outsourceable tasks:**
- Open Graph image design (use templates from Canva or similar; designer-led version post-AWE)
- Substack post drafting (you write; AI helps refine)
- Social account creation/refresh

**Must-learn topics:**
- Open Graph protocol basics (1 hour)
- Plausible analytics setup (30 min)

**Outsourceable / post-AWE-deferable:**
- Advanced SEO (structured data, schema.org markup) — defer
- Lighthouse performance optimization beyond defaults — defer to week 7 if needed
- A/B testing infrastructure — post-AWE

---

### Week 7: Quality gate decision + outreach OR polish

**Quality gate decision (early week 7):**

Is the foundation load-bearing? Test with the following criteria:

- ✅ Signup flow works end-to-end on mobile + desktop
- ✅ Posting a barrier works
- ✅ Following an issue works
- ✅ Working Group formation works
- ✅ Comments + mentions + notifications work
- ✅ Email deliverability is reliable (test emails arrive in <2 min, no spam)
- ✅ Search returns relevant results
- ✅ No critical bugs blocking core flows
- ✅ Demo outline reviewed; no major build gaps

**If YES (foundation load-bearing):**
- Outreach push to your existing connections (real verified healthcare professionals)
- Goal: 5-10 real verified users by AWE day with 5-10 real barrier posts
- Send WHO, DLA Piper, etc. emails 1-2 weeks before AWE per locked outreach timing

**If NO (foundation has gaps):**
- No outreach push
- Final polish only
- AWE demo is the architecture itself, not a populated platform
- Document remaining gaps; tackle post-AWE without deadline pressure

**Hands-on tasks (regardless of gate):**
- Demo script refinement (start of work for week 7-8)
- Bug triage from beta usage (Multiverse School cohort feedback)
- Performance check (Lighthouse audit, fix obvious wins)
- Accessibility audit (basic WCAG AA — Tailwind defaults are mostly compliant)

**Outsourceable tasks:**
- Bug fixes you can't crack: pre-vetted freelancer (Tier 3 of backup chain) — $300-500 reserve
- Accessibility audit beyond basic (defer post-AWE)

**Must-learn topics:**
- Reading stack traces unaided (you've been building this skill all along)
- Browser dev tools deep dive (Network, Console, Performance tabs)
- Lighthouse audit interpretation

**Outsourceable / post-AWE-deferable:**
- Advanced accessibility (full WCAG AAA)
- Performance optimization beyond Lighthouse-suggested wins

---

### Week 8: Final polish + dry runs + AWE

**Build deliverables:**
- Demo script fully refined (3-5 dry runs recorded)
- Q&A prep document (anticipated questions with prepared one-liners)
- Pre-loaded "good state" backup account ready for live demo failure scenario
- Offline screenshots/video as Plan C
- All social accounts have pinned "Launching at AWE" posts ready
- Substack first post scheduled for AWE day morning
- WHO, DLA Piper, etc. outreach emails sent 1-2 weeks before AWE day

**Hands-on tasks:**
- Demo dry runs with real device + real internet connection (not friend's MacBook)
- Q&A prep
- Final smoke tests
- AWE day plan documented (when to flip "BETA" indicator, when to send Substack post, etc.)

**Outsourceable tasks:**
- None this week — execution mode

**Must-learn topics:**
- None this week — execution mode

**AWE day — June 15-18, 2026:**
- Roundtable demo
- Live signup demonstrations to attendees who want to see it
- Soft conversations about post-AWE collaboration
- Capture any feedback for v1.1 backlog

---

## Pre-AWE-week tasks (outside the build sequence)

These happen during build but aren't core build work. Schedule into afternoon "rest and whatever catches user's fancy" hours.

| Task | When |
|---|---|
| Domain purchase + handle reservations | Week 1 |
| SNOMED browser self-serve mapping (~3-4 hours) | Week 2 |
| Lawyer consult ($300-500) | Week 5-6 |
| Pre-vet Django freelancers (15-min intro calls) | Week 5-6 |
| Outreach email drafting | Week 6 |
| Substack first post drafting | Week 6 |
| Demo outline drafting | Week 5 |
| Demo script refinement + dry runs | Week 7-8 |
| Outreach emails sent (1-2 weeks before AWE) | Week 6-7 |

---

## AWE day plan

**Morning of:**
- Remove "BETA" indicator from site
- Flip noindex/robots.txt to allow if not already done in week 3
- Submit fresh sitemap to Google Search Console
- Update pinned social posts to "Live now" version
- Publish Substack first post

**During roundtable:**
- 10-15 minute demo per script
- Q&A with prepared one-liners
- Capture feedback in real time (notebook or laptop)
- End with specific ask ("If you know a healthcare professional working on cross-jurisdictional issues, here's how to invite them.")

**Plan C (if live demo fails):**
- Switch to pre-loaded backup account
- If site is down: show offline screenshots/video
- Fall back to architecture pitch ("here's what we built; here's the GitHub repo")

---

## Post-AWE roadmap (cuts that come back)

Features cut from launch in priority order:

### v1.1 (1-3 months post-AWE)
- Three-way affiliation markers (interested / affected / working on)
- Public roadmap with voting on suggestions (if community demands transparency)
- Sentry / error monitoring integration
- Basic analytics dashboard for admins

### v1.5 (3-6 months post-AWE)
- WhatsApp authentication (Mexico market expansion priority)
- Multiple locations per user profiles
- DMs (if community asks; otherwise stay public-comments-only)
- Multi-location on barrier posts (3+ locations for genuinely multi-jurisdiction barriers)
- Spanish UI translation (Mexico-core mission alignment)
- Public suggestions roadmap with voting + status workflow
- Follow Person, Follow Location (with privacy controls — opt-in to be followable)
- Activity feed on profiles (with privacy controls)
- "Issues" higher-level grouping concept (if user behavior signals demand)

### v2 (6+ months post-AWE)
- Verified credential badge with credential review process
- SMS authentication (US convenience)
- Resolved-barrier achievement map (long-term vision — public map of CV-linkable resolutions)
- Real interactive pin map (when data density supports it)
- Cross-posting agent (auto-post to IG/LinkedIn/etc.)
- Full SNOMED CT integration
- Full social login (Google, Apple, etc.)
- Additional UI languages (French, German, Portuguese, Mandarin, Arabic)
- Healthcare informatics consult to refine SNOMED mappings (if user-facing SNOMED features expand)

---

## Living-document protocol

This file is updated weekly during the build:

1. End of each week: review what shipped vs. what was planned. Adjust next week's lessons accordingly.
2. End of each week: append a brief "Week N retro" entry at the bottom of this file — what worked, what didn't, what to adjust.
3. Quality gate at end of week 7 is documented in real time.
4. Post-AWE: a v1.1 / v1.5 / v2 roadmap retrospective once feedback is integrated.

---

## Week retros

*Append-only log of weekly retrospectives.*

*(First entry will be added at end of Week 1.)*
