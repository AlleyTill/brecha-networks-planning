# Decisions Locked — Brecha Networks

*Last updated: April 28, 2026*
*Companion files: `build-plan.md` (week-by-week execution), `research-backlog.md` (unresolved items)*

---

## How to use this file

This document holds finalized architectural and product decisions for Brecha Networks. When working with Claude or Claude Code on this project, share this file at the start of a new session to restore context. Pair with `build-plan.md` for execution context and `research-backlog.md` for remaining unknowns.

**Editing rules:**
- Don't modify locked decisions mid-discussion. Propose changes, discuss them, then update once a new decision is locked.
- When a strategic question is resolved, capture the decision here. (Tactical to-dos and research prompts live in `research-backlog.md` and don't need migration.)
- Add a line to the change log at the bottom whenever this file is updated.

---

## Project context

- **Name:** Brecha Networks (Spanish for "gap" + plural "networks" — names the problem the platform addresses, calls out the many small networks the platform facilitates among professionals)
- **Audience:** Healthcare administrators, providers, policy professionals, legal/advocacy folks, healthcare developers/builders. Explicitly **not** patients sharing personal medical experiences. Platform is for coordinating on barriers, not for personal support.
- **Primary value:** Cross-border / cross-jurisdiction discovery and coordination on shared barriers among professionals and builders.
- **Mexico-core mission:** Cross-border telehealth between US and Mexico is one of the most acute coordination problems in healthcare today. The platform's framing, language support, and design honor this from day one while serving a global audience.
- **Solo builder context:** Beginner-leaning-on-AI, building during 8-week pre-AWE window with AI-assisted workflow.
- **Predecessor:** Barrier Registry (archived) — top-down state-by-state scraping was legally infeasible. Brecha Networks inverts the model: verified professionals contribute the barriers they actually encounter.

---

## Strategic framing

Three principles inherited from the planning phase that supersede ordinary product-development tradeoffs:

1. **AWE deadline is non-sacred.** The goal is a load-bearing platform, not a demo at AWE specifically. AWE 2026 is one window for the goal to land, not the only one.
2. **Infrastructure-first.** Pre-AWE focus is rock-solid infrastructure and verified-trust mechanics. No fabricated content. No fake accounts. Demo is the architecture if real users haven't onboarded by AWE day.
3. **Quality gates over feature deadlines.** Week 7 gate decision: is foundation load-bearing? Yes → outreach push for real users in week 7-8. No → final polish only, no outreach. Either way, AWE demo happens with whatever real state exists.

---

## Naming + casing spec

| Context | Format | Example |
|---|---|---|
| Display name (docs, UI, marketing) | Title Case, with space | `Brecha Networks` |
| Domain (DNS rules) | lowercase, concatenated | `brechanetworks.com` |
| Social handles | lowercase, concatenated | `@brechanetworks` |
| Git repo / folder on disk | lowercase, kebab-case | `brecha-networks` |
| Python module names | lowercase, snake_case | `brecha_networks` |
| Database name | lowercase, snake_case | `brecha_networks` |
| Environment variables | UPPER_SNAKE_CASE | `BRECHA_NETWORKS_API_KEY` |

**Brand mark logic:** ALL CAPS for *BRECHA* alone was considered (to mark it as a brand and avoid mispronunciation as "breach-a"), but Title Case "Brecha Networks" works better with the qualifier — the phrase already reads as a brand, and ALL CAPS would make the brand visually louder than peer companies (Cisco Systems, F5 Networks, Palo Alto Networks).

---

## Tech stack

| Layer | Choice |
|---|---|
| Backend framework | Django (Python) |
| Frontend approach | Server-rendered HTML, no SPA |
| Frontend interactivity | HTMX (inline updates without page reload) |
| Frontend micro-interactions | Alpine.js (dropdowns, modals, toggles) |
| CSS framework | Tailwind CSS |
| Database (development) | SQLite |
| Database (production) | PostgreSQL with PostGIS (geocoding/proximity) and pg_trgm (search) extensions |
| Geocoding | Mapbox Geocoding API |
| Auth | Magic link + code-based email verification via `django-allauth` |
| Translation | **DeepL API** (translate-on-demand) — better EN↔ES quality, covers full post-AWE language plan |
| Email | **Resend** — 3K/mo free tier covers AWE-launch volume; SPF/DKIM/DMARC supported on `brechanetworks.com` |
| Hosting | **Railway** (Pro tier) — paid; cheaper than Render at AWE-scale and faster DX for solo builder |
| Caching | `django-redis` |
| Analytics | Plausible (privacy-respecting) — adding in week 6 |
| Domain registrar | TBD (Namecheap or Cloudflare Registrar recommended; tactical purchase pending) |

### Locked Django package list

These are the battle-tested packages Brecha Networks assembles rather than custom-builds:

| Need | Package |
|---|---|
| Auth + signup customization | `django-allauth` |
| Threaded comments + moderation + notifications | `django-comments-xtd` |
| @mentions in comments | **In-house parser** (built in week 2; ~50 LOC regex + user lookup + notification dispatch) |
| Notification preferences (frequency, mute, multi-channel) | `django-nyt` |
| Connect requests + Follow Issue | `django-friendship` |
| Tailwind integration | `django-tailwind` |
| HTMX integration helpers | `django-htmx` |
| Environment variable management | `django-environ` |
| Caching layer | `django-redis` |
| Search (built into Django + Postgres) | PostgreSQL full-text + `pg_trgm` extension |
| Geocoding/proximity (built into Django + Postgres) | PostGIS extension |

**Maintenance verification (completed May 2, 2026):**
- `django-mentions` (ivirabyan): abandoned (no release or commit since 2015-12-30). `django-exo-mentions` (exolever): also unfit — no PyPI release since 2019, only 1 GitHub star, recent commits but no stable contract. **Decision: build a small in-house @mention parser in week 2 rather than inherit an abandoned dependency.**
- `pinax-notifications`: stale (last release 2020-07; last commit 2024-07 — fails the >1yr criterion). **Swapped to `django-nyt`** (last release 2025-11, 182 stars, actively maintained).

### What we explicitly rejected and why

- **React / Next.js frontend** → doubles stack complexity, harder for beginner to debug, AI assistance is meaningfully worse on integration between two systems
- **Streamlit** → great for data apps with one user, painful for multi-user community
- **Social logins (Google, Apple, etc.) at launch** → operational overhead too high; magic link is a stronger differentiator
- **AWS at launch** → Railway/Render is the beginner-correct choice
- **Self-hosted Nominatim or OpenStreetMap** → too much DevOps burden
- **Bulk SNOMED CT integration at launch** → licensing complexity, 352K+ concept hierarchy, enterprise-grade work
- **SMS authentication at launch** → SMS pumping fraud risk, infrastructure cost, deferred to v2 post-AWE
- **WhatsApp authentication at launch** → Meta Business approval timeline unpredictable, deferred to v1.5 post-AWE for Mexico-market expansion
- **Custom-built comment system** → `django-comments-xtd` is production-grade and handles threading, moderation, notifications, GDPR compliance
- **Custom-built person search** → PostgreSQL full-text + `pg_trgm` already in stack; ~1 hour of integration

---

## URL conventions (locked upfront to avoid mid-build refactors)

- **Plural collection names:** `/barriers/`, `/working-groups/`, `/users/`, `/issues/`
- **Slug-based detail URLs:** `/barriers/medicaid-cross-state-coverage/` (NOT `/barriers/123/`)
- **Trailing slashes consistent:** Django default — always present
- **Lowercase only**
- **Hyphens, not underscores, in slugs**

These conventions apply from week 1 onward to avoid SEO redirect chains during the build.

---

## Privacy & identity model

### Model D: Verified-Pseudonymous Identity

Account required to post. **No fully anonymous posting option.** Every account has a display identity that includes:

- **Username** (not necessarily real name; pseudonyms allowed; 3-30 chars; alphanumeric + underscore + hyphen; must start with letter; case-insensitive uniqueness)
- **Verified role** (from locked taxonomy — see `role-taxonomy.md`; 42 roles + "Other" across Clinical, Non-clinical, Builder/Tech)
- **Verified region** (state/province level for display; full geocoded data stored)
- **Optional:** real name, organization name, more specific location

Users can post under a pseudonym. Other users always see role + region attached to a post. Trust comes from verified role + region, not from real name.

**No "Anonymous" displayed identity.** Pseudonymity gives personal protection (e.g., from employer retaliation); verified role + region gives the trust signal needed for coordination among professionals.

### Verification approach

- **At signup:** email verification (auto, code-based — not magic link), self-declared role from a fixed dropdown, self-declared region (Mapbox autocomplete), agreement to ToS
- **Email-domain enhancement:** professional/organizational email domains get a small additional marker (subtle indicator, automatic). Personal domains (gmail.com, etc.) don't get this marker.
- **"Self-declared" indicator:** visible on profiles and posts (e.g., subtle dotted underline on role badge with tooltip "Self-declared, not platform-verified")
- **Verified badge:** post-AWE feature. Users will be able to apply for full credential verification (license upload, employment letter). Verified users get a distinct visual marker.
- **Good-faith ToS clause** backs the whole system: users agree to truthfully represent location, role, and barrier experience.

### Region granularity (launch behavior)

- **Data stored:** full geocoded structure from Mapbox — city, region/state, country, latitude, longitude — regardless of what users select at signup
- **User identity display:** state/province level by default on profiles and posts. So profiles show "Provider, WA, US" rather than "Provider, Tacoma, WA, US."
- **Barrier post locations:** full geocoded specificity allowed (city, address, specific facility if relevant) — decoupled from user identity granularity
- **Why decoupled:** a user can describe a Tacoma-specific barrier without their identity being permanently tied to Tacoma
- **Future flexibility:** because we store full data and treat display as a separate rule, we can change display granularity later, add user-controlled granularity settings, or refine without data migration

### Affected Individual category cut

The role taxonomy explicitly excludes "Affected Individual" or similar patient-as-patient categories. The platform is for professionals and developers coordinating on barriers, not for personal medical experience sharing.

This must be communicated unmissably at two points:

1. Signup flow (before role selection)
2. Terms of Service (explicit prohibition on personal medical narratives, alongside existing prohibitions on medical advice and PHI sharing)

A former patient who has moved into building solutions belongs in the Developer category (or Policy, or wherever fits their current work) — not in a patient category.

---

## Account creation + auth

### Signup flow (Flow C — code-based hybrid)

1. User enters email
2. System sends 6-digit verification code via email
3. User enters code in same browser tab (no email-tab-switch interruption)
4. Same session: user fills out **username + verified role + verified region + their issue (auto-suggest dropdown) + ToS agreement**
5. Account active

### Return logins

Magic link via email. One-click, no typing. Industry-standard pattern for return visits.

### Mobile-optimal HTML attributes

- `autocomplete="one-time-code"` on the verification code input (triggers iOS/Android keyboard auto-suggestion from email/SMS)
- `inputmode="numeric"` on the code input (numeric keyboard on mobile)
- 16px+ font size on all form inputs (smaller triggers iOS zoom-on-focus)
- 44px+ touch targets (Apple/Google standard)
- Full-screen Mapbox geocoding UI on small viewports

### Sub-decisions

- **Username uniqueness:** case-insensitive
- **Session persistence:** 30-day "remember me" with explicit logout option
- **Password:** none — passwordless via magic link

### Issue selection at signup (auto-suggest)

After role + region + ToS, user picks "their issue" from auto-suggest dropdown:

- **Format:** `Issue Name (Location)`
- **Priority order:** matching name + geographic proximity, but doesn't exclude geographic outliers
- **Display:** name + degree-of-distance indicator
- **Implementation:** existing barrier titles + category tags become the auto-suggest source (simpler model — each barrier IS its own "issue" at launch; richer "Issues" higher-level grouping deferred to v1.5 if user behavior demands)

---

## Content & language model

- **Content posting language:** native language allowed for full barrier description. **English summary required** (2-3 sentences) for every barrier post. On-demand translate button on detail pages for full descriptions in other languages.
- **UI language at launch:** **English only.** Codebase fully internationalized from day one (Django i18n, all UI strings in translation files). Adding additional languages post-AWE is a translation task only, no code changes. Which languages to add post-AWE is a research task (see `research-backlog.md`).
- **Translation:** Google Translate API or DeepL API, called on-demand when users click "translate" on a non-English description.

---

## Forum / coordination features

### At launch

- **Post a barrier** with title, description, origin location, **destination location** (optional, for cross-jurisdiction barriers), category tags, **status marker**, English summary
- **Browse barriers** via list view + faceted search (filter by location, category, status)
- **Single barrier page** with discussion thread (comments)
- **Status markers** on barriers: *identified* / *being worked on* / *resolved* with small color dot indicators
  - "Resolved" requires a brief required text field describing how it was resolved (credibility for CV-worthy claims)
  - Status changeable only by original poster + Working Group members (prevents status spam)
- **Working Groups** — barrier posts can become working groups when 2+ people indicate interest. Members list, join request flow, group description, threaded discussion.
- **Connect requests** — user-to-user, both must accept, exchange contact info, no in-platform messaging stored
- **Follow Issue** — follow specific barriers/issues for notifications. The follow list IS the cohort list per barrier. (Follow Person and Follow Location explicitly cut for privacy/professional comfort.)
- **Email notifications** — for replies, new working group activity, connect requests. Frequency controls: instant / daily digest / weekly digest / mute thread.
- **@mentions** in comments → notify mentioned user
- **Threaded comments** (1-level depth) with markdown support, quote-reply, edit history visible
- **Person search** — search users by username or display name (PostgreSQL full-text + trigram for typo tolerance). Only public profile fields are searchable.
- **Suggestions form** — private feedback form, emails admins (no public roadmap, no voting at launch)
- **Magic link login + code-based email verification at signup**
- **Self-declared role + region on every post and profile** (Model D)
- **Profile depth at launch:** username + verified role + verified region + free-text bio + their posts (cut: structured issue interests, activity feed)

### Cut from launch (deferred)

| Feature | Defer to | Reason |
|---|---|---|
| Status tracking variants | — | KEPT at launch (color-dot pattern, GitHub-Jira convention) |
| Multiple locations per user | v1.5 | Bio text mitigates; structured multi-location is build-cost-heavy |
| Three-way affiliation markers (interested / affected / working on) | v1.1 | Single "follow" toggle is enough at AWE-scale; markers needed when registry grows |
| Direct messages (DMs) with friction | v1.5 | Replaced by richer public comments; Connect requests cover off-platform contact exchange |
| Follow Person, Follow Location | v1.5 | Privacy — pseudonymous identity is undercut if anyone can track all activity |
| Voting / reactions on barriers or comments | v1.5 | Not earning build cost at launch |
| In-app notifications (bell icon) | v1.5 | Email is enough at launch |
| Cross-posting agent (auto-posting to IG/LinkedIn/etc.) | v2 | Manual pre-scheduling is fine at launch |
| Real interactive pin map | v2 | Choropleth/geocoded foundation only at launch; map UI deferred to when data density supports it |
| Full SNOMED CT integration | v2 | SNOMED-aligned curated taxonomy at launch instead |
| Full social login (Google, Apple, etc.) | v2 | Magic link is differentiator |
| Verified credential badge with credential review | v2 | Data model supports it; UI/process deferred |
| SMS authentication | v2 | US convenience only; defer for SMS pumping fraud risk + cost |
| WhatsApp authentication | v1.5 | Mexico-market expansion priority; defer for Meta Business approval timeline |
| Multi-location on barrier posts (3+ locations) | v1.5 | Origin + 1 destination at launch; 3+ for genuinely multi-jurisdiction barriers |
| "Issues" higher-level grouping (vs. each barrier = own issue) | v1.5 | Simpler model at launch; introduce hierarchy if demand emerges |
| Public roadmap with voting on suggestions | v1.5 | Private form at launch |
| Resolved-barrier map as professional achievement layer | v2 | Long-term vision; CV-linkable resolutions |

---

## Geographic & taxonomy foundation

- **Geocoding:** Mapbox API for place autocomplete on all location inputs. Stores structured data: name, lat/long, region, country.
- **Proximity search:** PostGIS in PostgreSQL handles "within X miles" queries. Radius slider in UI: 10mi / 25mi / 50mi / 100mi / Statewide / National.
- **No visual map at AWE.** Geocoded foundation only. Choropleth view deferred to post-AWE when data density supports it.
- **Categories at launch:** 15-25 curated barrier categories, each mapped to a SNOMED CT concept ID stored in the database. Display uses category names, not SNOMED's. ID stored for future migration.
- **SNOMED mapping approach:** self-serve via free SNOMED browser (browser.ihtsdotools.org). Healthcare informatics consult deferred post-AWE per Q11 lock.
- **Role taxonomy at launch:** **LOCKED** — 42 roles + "Other (free-text)" grouped Clinical / Non-clinical / Builder/Tech / Other. Predictive-text dropdown paired with a free-text "Specialty / focus area" field. Full table + sources + judgment calls in `role-taxonomy.md`.

---

## Legal & compliance posture

- **Lawyer consult: COMMITTED.** $300-500 one-hour consultation in week 5-6 with a healthcare/tech lawyer in WA. Pre-loaded agenda items: trademark filing strategy (Brecha Networks Class 42 + 44 with Brea Networks + Breca Peru audio-similarity flags), user-generated content liability (Section 230 + defamation mitigation), HIPAA boundary confirmation, GDPR cross-border data flow, templated ToS sufficiency, post-AWE verified credential feature liability shifts.
- **Healthcare informatics consult: DEFERRED.** Self-serve via free SNOMED browser at launch. Reserve $200-400 for post-AWE if SNOMED becomes more central to user-facing features.
- **Privacy policy:** **iubenda Essentials** (~$5-7/mo, 25K pageviews/mo cap — well above AWE-launch volume). GDPR + CCPA + auto-updates as laws change. Healthcare-specific clauses to be confirmed/added during the week 5-6 lawyer consult.
- **Terms of Service must include:**
  - **Good-faith representation clause:** users agree to truthfully represent their actual location, role, and barrier experience. Misrepresentation grounds for suspension/removal.
  - **Platform-purpose clause:** platform is for coordinating on healthcare barriers among professionals and developers, NOT for personal medical experience sharing or seeking medical advice.
  - Prohibition on giving or seeking medical advice
  - Prohibition on naming specific individuals in negative ways (defamation mitigation)
  - PHI / patient information sharing prohibited
  - Standard platform liability limitations
- **Data export:** Users can download their own data on request (GDPR requirement). Standard implementation: management command produces JSON export. ~1 day work in week 5.
- **Data deletion:** Users can request account + data deletion (GDPR requirement). Deletion = anonymize (replace user reference with "Deleted account" everywhere).
- **Inactive account retention:** indefinitely retained until deletion requested. No auto-purge.
- **GDPR:** Applies because EU users are expected. Privacy policy + consent + deletion mechanisms required.
- **HIPAA:** Probably doesn't apply (platform is for documenting barriers, not transmitting PHI). Confirm in lawyer consult. Revisit if/when partnering with covered entities.
- **Section 230 / defamation:** US protections apply; international protections vary. Mitigated by ToS + light moderation.
- **SNOMED licensing:** Storing concept IDs as references is fine, no license needed. Bulk SNOMED data import would require licensing — not doing that at launch.
- **Trademark filing:** Plan to file USPTO Class 42 + 44 for "Brecha Networks" with lawyer guidance in week 5-6. Consider Madrid Protocol for Mexico (IMPI) and Canada (CIPO) coverage.

### Audio-similarity flags for lawyer review

- **Brea Networks** (US, government IT contractor, Class 42) — different word, different audience, different sub-area; lawyer to confirm clearance
- **Breca** (Peruvian healthcare conglomerate) — audio-similar in Spanish; lawyer to confirm Peru market clearance if/when LATAM expansion happens

---

## Hosting / deployment

- **Public deploy date:** Week 4-5 of the 8-week timeline (NOT week 8). Site lives behind invite-only beta with Multiverse School cohort during the build.
- **Launch model:** **Invite-only beta** during build. No public-facing holding page at launch. Site is technically public but only shared with invited testers. At AWE, the beta transitions to public launch via content/banner changes (e.g., remove "BETA" indicator, update pinned social posts) — not a technical gate-lift.
- **Domain posture:** `brechanetworks.com` is the **primary canonical URL** and the only domain serving content or sending mail. Seven other domains are registered as **redirect-only (301 → `brechanetworks.com`)**: `brecha.network` (secondary / tech-audience asset), `brechanetwork.com`, `brechanetworks.org`, `brecha-network.com`, `brecha-networks.com`, `brecha-network.org`, `brecha-networks.org`. Redirect-only domains carry no MX records and no SPF/DKIM/DMARC. Full purchase list in `research-backlog.md` → Tactical follow-ups → Domain purchase.
- **HTTPS:** Handled automatically by Railway/Render
- **Database backups:** Automated, verified before week 5
- **Environment variables:** Secrets via env vars (`django-environ`), not in code
- **Email deliverability:** SPF/DKIM/DMARC records configured on **`brechanetworks.com` only** (the canonical sender domain) during week 5 hardening (~½ day with Postmark/Resend's walkthroughs). Redirect-only domains do not send mail and get no SPF/DKIM/DMARC records.
- **Production scaling:** Standard recipe handles 1000+ concurrent users without special architecture. Configuration: `CONN_MAX_AGE = 60`, PgBouncer for connection pooling, `django-redis` cache, `select_related`/`prefetch_related` discipline, S3 + Cloudflare CDN for static assets.
- **Hosting tier:** Paid tier (Railway Pro or Render Standard). Free tiers insufficient for production traffic.

---

## SEO / discoverability

- **Weeks 1-2:** `noindex, nofollow` meta tag + restrictive `robots.txt`. Site has minimal content during this phase; protect against thin-content indexing.
- **Week 3 onwards:** allow indexing. Submit sitemap to Google Search Console. ~5-6 weeks of crawl time before AWE = strong organic search presence by AWE day.
- **AWE day:** site is already indexed. No technical flip needed; only content/banner changes (remove "BETA" indicator, etc.).

---

## Eco-sphere (social / external presence)

- **Social accounts:** IG, LinkedIn, Substack, GitHub, X — all reserved/created with consistent branding (`@brechanetworks` where available; qualifier where not — see tactical follow-ups in `research-backlog.md`)
- **Footer links:** From site to all relevant social accounts (5-10 click discoverability requirement)
- **Pinned posts:** Each social account has a pinned "Launching at AWE" post until launch
- **Substack:** First post drafted and scheduled for AWE day morning
- **Posting strategy at launch:** Manual pre-scheduling (no cross-posting agent). Posts go live on AWE day morning via each platform's native scheduler or Buffer/Hootsuite.
- **Outreach timing:** WHO, DLA Piper, etc. emails sent **1-2 weeks before AWE** (not day-before). **LOCKED.**
- **Outreach gating:** outreach push only happens in week 7 IF foundation is load-bearing. If not, no outreach push pre-AWE.

---

## Branding / visual identity

- **Logo:** Wordmark + simple bridge icon at launch. Designer-led refinement post-AWE. Bridge motif honors the etymology (Brecha = gap; bridge = solution across the gap).
- **Color palette:**
  - **Primary direction:** deep teal (e.g., `#0F766E`) — professional, calm, distinct from Doximity (blue) and Sermo (red)
  - Secondary: white + warm neutrals
  - Accent (added in week 6 polish if needed): warm amber or coral
  - Avoid: scrubs blue, ER red, pastels (medical clichés or amateur-looking)
- **Typography:** **Inter**, single family, multiple weights (400/500/600/700). Free Google Font. Used by Vercel, Stripe, Linear, GitHub. Designed for screen rendering.
- **Voice/tone:** Direct, professional, warm. Active voice. Sentence case for buttons. No corporate-speak ("synergize," "leverage"), no startup-cute ("hey there!", "rad"). Treat users as professionals.
- **Tagline:** **"Find the people working on the same problem."** Concrete, active, names the user goal, translates well across languages.

Final hex values + tagline polish locked during week 6 polish or with a designer post-AWE.

---

## Build sequence (high-level)

Week-by-week deliverables. Detailed lessons + AI-assistance gradient + outsourcing decisions live in `build-plan.md`.

| Week | Focus |
|---|---|
| 1 | Project bootstrap — Django + django-allauth + Tailwind + HTMX + base templates + URL conventions |
| 2 | Data model + Django admin + barrier post + working group + comment skeleton |
| 3 | UI/UX polish — Tailwind components, mobile-first responsive design, HTMX patterns, signup flow with issue auto-suggest |
| 4 | Deploy to Railway/Render + Postgres + PostGIS + pg_trgm + custom domain + email service |
| 5 | Hardening — rate limiting, moderation flow, email deliverability (SPF/DKIM/DMARC), error pages, GDPR data export, lawyer consult, demo outline draft |
| 6 | Eco-sphere — social accounts, branding finalization, footer links, Substack first post, color/font polish, accent color decision |
| 7 | **Quality gate decision.** Foundation load-bearing? YES → outreach push for real verified users. NO → final polish only. Buffer week for bugs, performance, accessibility, demo dry runs. |
| 8 | Final polish + demo script refinement + AWE |

**AWE deadline is non-sacred.** If week 7 reveals foundation isn't ready, AWE demo is the architecture itself, not a populated platform. Outreach happens when ready.

---

## Backup chain (for week 7 bug-fix help)

In escalation order:

1. **Primary:** AI-assisted self-debug (Claude Code, Cursor — already in workflow)
2. **Secondary:** Multiverse School community + Django Discord/Forum + r/django + Stack Overflow
3. **Tertiary:** Pre-vetted Upwork/Toptal Django freelancer ($300-500 reserve, 2-5 hours of help; pre-vet 2-3 candidates in week 5-6)

**Pre-AWE-week prep tasks:** Document codebase for outside helpers (README + architecture sketch + key file map). Set up "freelancer-friendly" repo state (clean commits, env example file, runbook for local setup).

---

## Demo script timing

Hybrid approach:

- **Week 5:** draft demo OUTLINE (~1 hour) — what story, what features, identify build gaps for week 6 polish
- **Week 7-8:** refine SCRIPT (~3-5 hours over multiple sessions) — what you click, what you say, in what order; 3-5 dry runs; contingency for live failures; Q&A prep

**Script structure (~10-15 minutes):**

1. Opening hook (30-60s) — what is Brecha Networks, why does it matter
2. Live signup (2-3 min) — Model D in action
3. Browse + search barriers (2-3 min) — cross-jurisdictional discovery
4. Post a real barrier (2-3 min) — structured form, English summary
5. Working Group OR Connect request (2-3 min) — coordination
6. Status update flow (1-2 min) — "resolved" as CV-worthy achievement
7. Closing pitch (1-2 min) — concrete next-step ask

**AWE day tactics:** pre-loaded "good state" backup account ready; offline screenshots/video as Plan C; practice with real device + real connection; under 12 minutes; end with specific ask.

---

## Suggestions feature

- **Form fields:** title + description + free-text "kind of suggestion"
- **Behavior:** submits → emails admins → thank-you confirmation
- **No public visibility, no voting, no structured categories, no status workflow at launch**
- **Build cost:** ~½ day
- **v1.5 path:** public roadmap + voting + status workflow if community demands it post-launch

---

## User data over time

- **Account deletion request:** triggers anonymization of all user content (replace reference with "Deleted account" everywhere). GDPR-compliant.
- **Inactive account retention:** indefinitely retained until deletion requested. No auto-purge at launch.
- **Working group membership transfer when changing roles:** deferred post-AWE.
- **In-memoriam / former-member status:** deferred post-AWE.

---

## Budget

### Recurring (post-launch monthly)

Target: under $100/month total.

| Item | Estimated cost |
|---|---|
| Hosting (Railway Pro or Render Standard) | $20-50 |
| Postgres database | $0-10 |
| Mapbox geocoding | $0-5 |
| Email service (Postmark/Resend) | $0-15 |
| Translation API | $0-10 |
| Plausible analytics | $9 |
| Domain | ~$1 amortized |
| Privacy policy service | $5-15 |
| Buffer for surprises | $20 |
| **Total** | **~$60-100/mo at launch** |

### One-time pre-AWE costs

| Item | Estimated cost |
|---|---|
| Domain purchase (`brechanetworks.com` + defensive `.network`, `.org`, `.health`) | $35-150 |
| Lawyer consult (committed) | $300-500 |
| Healthcare informatics consult (deferred) | $0 (post-AWE if needed) |
| Optional: freelance design polish | $500-1500 |
| Pre-vetted freelancer reserve (week 7 backup) | $300-500 |
| **Total realistic range** | **$700-2700** |

---

## Change log

*Append-only record of when decisions were added or changed.*

- **April 25, 2026 (initial creation as Community Registry working name):** Tech stack locked. Privacy model locked as Verified-Pseudonymous (Model D). Light verification + email-domain enhancement. Region granularity launch behavior locked. Affected Individual category cut. Provisional Developer definition. Content language locked. Outreach timing locked.
- **April 25, 2026 (mid-conversation update):** Disclaimer placement corrected to signup + ToS only. Item 6 launch model locked as invite-only beta with school cohort, no holding page. Item 3 UI language locked as English-only at launch with fully internationalized codebase.
- **April 25, 2026 (file structure update):** Added "How to use this file" headers and cross-references. Added change log.
- **April 27-28, 2026 (project name lock + comprehensive question pass):** Name locked as **Brecha Networks** after comprehensive due-diligence sweep (US, Mexico, Argentina, Colombia, Peru, Chile, Spain healthcare; crypto/Web3; mobile apps; PyPI; npm; Reddit; Wayback Machine; Wikipedia; trademark databases). Two minor lawyer-clearance items flagged: Brea Networks (US gov IT) audio similarity, Breca (Peru healthcare) audio similarity. Casing spec defined. URL conventions locked upfront.
- **April 28, 2026 (open question resolution pass):** Q6 (account creation) — Flow C code-based hybrid via django-allauth + magic link return logins + mobile-optimal HTML. Q7 (scope cuts) — multiple locations per user, three-way affiliation markers, DMs, follow person/location all CUT; status tracking REINSTATED with color dots; person search, issue auto-suggest, multi-location barriers ADDED. Q8 (content seeding) — no pre-AWE seeding, infrastructure-first, real users post-launch only if load-bearing. Q9 (SEO) — noindex weeks 1-2, allow indexing from week 3. Q10 (lawyer consult) — COMMITTED $300-500 week 5-6. Q11 (informatics consult) — DEFERRED, self-serve SNOMED. Q12 (backup chain) — AI → community → freelancer escalation. Q14 (demo script) — hybrid timing week 5 outline + week 7-8 refinement. Q15 (suggestions) — private form, simplest. Q16 (branding) — wordmark + bridge icon, deep teal, Inter, "Find the people working on the same problem." tagline. Q17 (data over time) — deletion=anonymize, indefinite retention. AWE deadline reframed as non-sacred; quality gate is "load-bearing platform."
- **April 28, 2026 (package commitments):** Locked Django package list — django-allauth, django-comments-xtd, django-mentions, pinax-notifications, django-friendship, django-redis, django-environ, django-tailwind, django-htmx. Verify maintenance status in week 1.
- **April 28, 2026 (rewrite + new repo):** Comprehensive rewrite of `decisions-locked.md` reflecting all locks. Migration from `community-registry-planning` to `brecha-networks-planning` repo. Old repo deleted. Predecessor `barrier-registry` repo archived (kept public for cohort/portfolio reference).
- **May 2, 2026 (domain posture lock):** Locked 8-domain purchase list. `brechanetworks.com` = primary canonical + only email sender (SPF/DKIM/DMARC). `brecha.network` re-scoped to secondary / tech-audience redirect. Six additional defensive domains registered redirect-only (mashed/hyphenated × singular/plural × .com/.org). `brechanetworks.health` and `brechanetworks.io` dropped from the purchase list. Full table in `research-backlog.md`.
- **May 2, 2026 (file rename):** Renamed `open-questions.md` → `research-backlog.md` to better describe its actual contents (parked tactical to-dos + post-launch research prompts). All "Things still genuinely undecided" had already been resolved; the file's role is execution + research, not open strategic questions. Updated cross-references in `README.md`, `build-plan.md`, and this file.
- **May 2, 2026 (package maintenance check):** Ran the week-1 maintenance verification ahead of schedule. `pinax-notifications` swapped to `django-nyt` (cleanly maintained, last release 2025-11). `django-mentions` is abandoned (no commits since 2015) and the locked alternative `django-exo-mentions` also fails the bar (no PyPI release since 2019, 1 GitHub star). Decision: build a small in-house @mention parser in week 2 rather than inherit an abandoned dependency. Removed the maintenance-status-check tactical follow-up from `research-backlog.md`.
- **May 2, 2026 (provider locks + role taxonomy):** Locked four provider choices: **Email = Resend** (3K/mo free covers AWE-launch; cheaper at projected scale than Postmark Basic $15). **Translation = DeepL** (better EN↔ES quality, Mexico-core priority; 31 languages cover the post-AWE plan). **Hosting = Railway Pro** (cheaper than Render at AWE-scale, faster DX for solo builder). **Privacy policy = iubenda Essentials** (~$5-7/mo, 25K pageview cap fits launch volume). Tech stack table + Privacy policy bullet updated. Resolved tasks #1, #2, #3, #5. Locked role taxonomy: 42 roles + "Other" across Clinical / Non-clinical / Builder/Tech, sources synthesized from NUCC, AMA/ABMS, Doximity, LinkedIn, Health eCareers/JAMA, HRSA, CMS, ONC/HIMSS. Full table + judgment calls in new file `role-taxonomy.md`. Removed the "pending research" placeholders. Resolved Task 1 in `research-backlog.md`.
