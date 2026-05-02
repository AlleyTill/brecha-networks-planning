# Open Questions — Brecha Networks

*Last updated: April 28, 2026*
*Companion file: `decisions-locked.md`*

---

## How to use this file

This file holds unresolved questions and parked tactical items for Brecha Networks. Most prior questions (Q0, Q1, Q6-Q17) have been resolved and migrated to `decisions-locked.md`. What remains are research tasks for desktop sessions and tactical follow-ups parked for later execution.

**Editing rules:**

- When a question gets answered, move the resolution to `decisions-locked.md` and remove the question from this file.
- The "Desktop research tasks" section is for self-contained research prompts that can be picked up in any session.
- The "Tactical follow-ups" section is for to-do items that don't require strategic discussion — just execution.

---

## Tactical follow-ups (parked)

These don't require further strategic discussion. Execute when ready.

### Domain purchase

Verify availability and purchase via Namecheap or Cloudflare Registrar (Cloudflare cheaper, Namecheap easier UX for first-time buyer):

**Locked purchase list (all 8):**

| # | Domain | Role |
|---|---|---|
| 1 | `brechanetworks.com` | **Primary canonical URL.** Site + all email sender domains (SPF/DKIM/DMARC). |
| 2 | `brecha.network` | Secondary / redirect / tech-audience asset. 301 → `brechanetworks.com`. |
| 3 | `brechanetwork.com` | Defensive (mashed singular). 301 → primary. |
| 4 | `brechanetworks.org` | Defensive (mashed plural .org). 301 → primary. |
| 5 | `brecha-network.com` | Defensive (hyphenated singular). 301 → primary. |
| 6 | `brecha-networks.com` | Defensive (hyphenated plural). 301 → primary. |
| 7 | `brecha-network.org` | Defensive (hyphenated singular .org). 301 → primary. |
| 8 | `brecha-networks.org` | Defensive (hyphenated plural .org). 301 → primary. |

Only `brechanetworks.com` serves canonical content and sends mail. The other 7 are redirect-only — no SPF/DKIM/DMARC, no MX records.

`brechanetworks.health` and `brechanetworks.io` are **not** on the purchase list (deferred unless squatting concern emerges).

WHOIS privacy on for every domain (free on Namecheap and Cloudflare, default-on for both).

### Social handle reservation

All available per April 28, 2026 verification. Reserve before someone squats:

- IG: `@brechanetworks` (verify availability via signup)
- LinkedIn: `linkedin.com/company/brechanetworks` (create company page)
- Substack: `brechanetworks.substack.com` (sign up + claim handle)
- GitHub: `github.com/brechanetworks` (create org account)
- YouTube: `@brechanetworks` (channel creation)
- X: `@brechanetworks` (account creation)

### Lawyer consult agenda items (pre-loaded)

For the $300-500 one-hour healthcare/tech lawyer consult in WA in week 5-6:

**Tier 1 — must answer:**
1. Trademark filing strategy: USPTO Class 42 + 44 for "Brecha Networks"? Brea Networks (Class 42) collision risk? Madrid Protocol vs. USPTO + IMPI for Latin America?
2. Filing timing: before launch (intent-to-use) or after first use?
3. User-generated content liability: Section 230 baseline; defamation mitigation when users name specific providers; healthcare-adjacent moderation requirements
4. Defamation mitigation specifics: ToS clauses

**Tier 2 — should answer:**
5. HIPAA boundary confirmation
6. GDPR cross-border data flow
7. Templated ToS sufficiency (Termly/iubenda for healthcare context)

**Tier 3 — nice to have:**
8. Verified credential feature (post-AWE) — liability/insurance shifts
9. WA-specific privacy/professional licensing rules

### Pre-vet freelancers (week 5-6)

For week 7 bug-fix backup chain. Identify 2-3 Django freelancers on Upwork or Toptal. Brief 15-min intro calls so they're familiar with the project context if needed in week 7.

### Maintenance status check (week 1)

Verify these packages are actively maintained as of build start:
- `django-mentions` — if stale (>1 year without commits), swap to `django-exo-mentions`
- `pinax-notifications` — if stale, swap to `django-nyt`

---

## Desktop research tasks

Self-contained research prompts that can be picked up in any future session.

### Task 1: Role taxonomy research

**Prompt:** Look up dropdown menu trends in reputable services (LinkedIn role categorizations, Doximity, NPI taxonomy codes, AMA specialty lists, healthcare job boards). Find the most inclusive and exhaustive list of healthcare-related roles. Don't worry about list length — predictive text in the dropdown will help users find their role.

**Constraints:**
- Must be familiar/recognizable to US healthcare workers (the primary anchor audience)
- Must include space for non-clinical roles (Admin, Policy, Legal, Researcher, Developer)
- Must NOT include "Affected Individual" or any patient-as-patient category
- "Developer in healthcare" must be present as a category for builders/founders/technologists
- "Other" with required free-text description as fallback

**Output:** Finalized list of roles for the signup dropdown. Each role should have a one-line definition that appears as help text or tooltip.

**Status:** Blocks signup-form completion in week 2-3. Run before then.

---

### Task 2: Region granularity refinement research

**Prompt:** Look at how comparable professional platforms (LinkedIn, Doximity, healthcare directories, professional networks like Behance, AngelList/Wellfound) handle location specificity for user identity vs. content.

**Questions:**
- Do they show city level by default on profiles, or state/region only?
- Can users adjust their own display granularity?
- How do they handle small-region privacy (where city-level identity is functionally identifying)?
- Do they decouple "where you are" from "where your work happens"?

**Output:** Recommendation on whether to keep state/province display default, change it, or surface a user-controlled granularity setting in profile settings.

**Status:** Launch behavior is locked (state/province display default). This is a post-launch refinement question.

---

### Task 3: Barrier post structured-form refinement

**Prompt:** Decide whether barrier posts should add structure beyond the locked required fields (title + origin location + English summary + full description + category tags + status marker), or stay free-text + prompts for additional context.

**Optional additional structure to consider:**
- "What would resolution look like?" (text)
- "Are you currently working on this?" (yes/no — though this is somewhat covered by status marker)
- "Who is affected?" (free text or structured)

**Trade-off:** Required fields nudge every post toward solution-orientation rather than complaint. They also create signup friction and risk junk content in fields users don't have good answers for.

**Output:** Final list of optional fields for the barrier post form, with rationale for each.

**Status:** Locked fields are sufficient for launch. This is a post-launch refinement.

---

### Task 4: Post-AWE language selection

**Prompt:** The platform launches with English UI only but is fully internationalized in the codebase. Adding new languages post-AWE is a translation task with no code changes. Decide which languages to add and in what order.

**Considerations:**
- Spanish (Americas reach — strongest second language candidate; Mexico-core)
- French (Quebec, France, West Africa, parts of EU)
- German (DACH region, strong healthcare policy presence)
- Mandarin (largest population reach, culturally distant from typical AWE audience)
- Portuguese (Brazil, Portugal)
- Arabic (MENA, but adds RTL layout work)

**Translation source options:**
- Professional UI translation (~$0.10-0.20/word; ~1000-1500 words of UI; ~$300-600 per language)
- Machine translation (Google/DeepL) with manual review (free but quality varies)
- Community/volunteer translation (free, slow, inconsistent)

**Output:** Ordered list of languages to add post-AWE, translation source for each, rough timeline.

**Status:** Post-AWE work. Spanish should be first given Mexico-core mission.

---

## Future-flagged items (for later consideration)

Things that should be revisited at specific future moments.

### Multi-language UI launch order

Tied to Task 4 above. Spanish first, given Mexico-core. Likely v1.5 alongside WhatsApp authentication for Mexico expansion.

### Public roadmap with voting on suggestions

Currently launch model is private form only. Revisit if/when user community asks for transparency.

### Healthcare informatics consult ($200-400)

Currently deferred. Revisit if SNOMED becomes more central to user-facing features (e.g., displaying SNOMED-formatted category names, integrating with EHR/EMR systems, pursuing clinical research partnerships).

### Multi-location on barrier posts (3+ locations)

Launch supports origin + 1 destination. Extend to 3+ if cross-jurisdictional barriers genuinely span 3+ regions in real user data.

### Resolved-barrier achievement layer

Long-term v2 vision — public map of resolved barriers as professional accomplishment portfolio that providers can link to on their CV. Tied to platform identity arc from "complaint registry" to "professional accomplishment platform."

### Three-way affiliation markers (interested / affected / working on)

Cut for launch in favor of single "follow" toggle. Revisit at v1.1 when registry has 100+ barriers and Working Group auto-formation becomes meaningful.

### Multiple locations per user profiles

Cut for launch. Revisit at v1.5 when user count justifies the structured M2M relationship complexity.

---

## Things still genuinely undecided

None at this time. All pre-build strategic questions have been resolved. Tactical execution items are tracked in the "Tactical follow-ups" section above.
