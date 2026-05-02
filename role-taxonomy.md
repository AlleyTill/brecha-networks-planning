# Role Taxonomy — Brecha Networks

*Locked: May 2, 2026*
*Companion files: `decisions-locked.md` (Privacy & identity model + Geographic & taxonomy foundation sections reference this file), `build-plan.md` (consumed in Week 2-3 signup form)*

---

## How to use this file

This document holds the locked role taxonomy used by the signup dropdown and shown alongside every post (per Verified-Pseudonymous Identity / Model D in `decisions-locked.md`). Total: **42 roles + "Other (free-text)"**, grouped into Clinical / Non-clinical / Builder/Tech / Other.

The signup form pairs this dropdown with a free-text **"Specialty / focus area"** field so sub-specialty (e.g., "Cardiology," "Pediatric Oncology") is captured without polluting the role list.

---

## Sources synthesized

- **NUCC Healthcare Provider Taxonomy** (National Uniform Claim Committee) — NPI taxonomy used on the CMS NPPES registry. ~870 individual codes collapsed into role-shaped buckets.
- **AMA / ABMS specialty list** — informed the "Physician (MD/DO)" collapsing logic.
- **Doximity role taxonomy** (US healthcare social network) — Brecha intentionally widens beyond Doximity's clinician + industry scope.
- **LinkedIn industry groupings** ("Hospitals and Health Care," "Pharmaceutical Manufacturing," "Medical Practices") and associated job-function tags.
- **Health eCareers / JAMA Career Center** category lists.
- **HRSA health workforce categories** (clinician, public health, community health worker).
- **CMS provider type lists** for adjacency to the regulatory side.
- **ONC / HIMSS workforce categorizations** — informed the Builder/Tech bucket (clinical informaticist, health-data analyst, EHR/interoperability engineer).

---

## Synthesis logic

Three layered moves:

1. **Collapse clinical sub-specialties.** "Physician (MD/DO)" is one row, not 24. Predictive search in the dropdown handles disambiguation; sub-specialty captured by a separate free-text field on the signup form. Avoids the awkward dual-boarded problem.
2. **Graft on non-clinical professional roles** Doximity excludes but Brecha needs: administration, policy, legal/advocacy, public health, research, journalism, translation/cultural-broker.
3. **Add an explicit Builder/Tech cluster** (developer, designer/PM, informaticist, founder, investor) because the platform's audience explicitly includes "Developer in healthcare."

US-recognizable terminology preferred (RN not "nurse," PA not "physician associate," EMT/Paramedic not "ambulance technician").

---

## Final taxonomy

| # | Label | Definition | Category |
|---|---|---|---|
| 1 | Physician (MD/DO) | Licensed allopathic or osteopathic physician practicing in any specialty. | Clinical |
| 2 | Resident or Fellow | Physician in an accredited graduate medical training program (residency or fellowship). | Clinical |
| 3 | Medical Student | Currently enrolled in an MD, DO, or international equivalent medical degree program. | Clinical |
| 4 | Nurse Practitioner (NP) | Advanced practice registered nurse with independent or collaborative prescribing authority. | Clinical |
| 5 | Physician Assistant (PA) | Licensed physician assistant practicing under a supervisory or collaborative agreement. | Clinical |
| 6 | Certified Nurse-Midwife (CNM) | Advanced practice nurse credentialed in midwifery and reproductive care. | Clinical |
| 7 | Certified Registered Nurse Anesthetist (CRNA) | Advanced practice nurse credentialed to deliver anesthesia. | Clinical |
| 8 | Registered Nurse (RN) | Licensed registered nurse in any practice setting. | Clinical |
| 9 | Licensed Practical / Vocational Nurse (LPN/LVN) | State-licensed practical or vocational nurse. | Clinical |
| 10 | Nursing Student or Trainee | Enrolled in an RN, LPN/LVN, or APRN training program. | Clinical |
| 11 | Pharmacist (PharmD/RPh) | Licensed pharmacist in retail, hospital, clinical, or specialty pharmacy. | Clinical |
| 12 | Pharmacy Technician or Trainee | Certified or in-training pharmacy support professional. | Clinical |
| 13 | Dentist (DDS/DMD) | Licensed dentist in general or specialty dental practice. | Clinical |
| 14 | Dental Hygienist or Assistant | Licensed dental hygienist or certified dental assistant. | Clinical |
| 15 | Mental Health Clinician | Licensed psychiatrist, psychologist, LCSW, LMFT, LPC, or equivalent behavioral health provider. | Clinical |
| 16 | Therapist (PT / OT / SLP) | Licensed physical therapist, occupational therapist, or speech-language pathologist. | Clinical |
| 17 | Optometrist or Ophthalmic Provider | Optometrist (OD), opticianry professional, or ophthalmic technician. | Clinical |
| 18 | Chiropractor (DC) | Licensed doctor of chiropractic. | Clinical |
| 19 | Podiatrist (DPM) | Licensed doctor of podiatric medicine. | Clinical |
| 20 | Dietitian or Nutritionist | Registered dietitian (RD/RDN) or licensed clinical nutritionist. | Clinical |
| 21 | Paramedic or EMT | Nationally registered or state-licensed prehospital emergency clinician. | Clinical |
| 22 | Allied Health Professional | Licensed/certified allied health role not listed above (e.g., respiratory therapist, radiologic technologist, medical lab scientist, sonographer). | Clinical |
| 23 | Doula, Midwife (non-CNM), or Birth Worker | Trained perinatal support professional, including community-based and traditional midwives. | Clinical |
| 24 | Community Health Worker or Promotor(a) | Frontline community-based health worker, navigator, or promotor(a) de salud. | Clinical |
| 25 | Public Health Professional | Epidemiologist, health department staff, sanitarian, or other public health practitioner. | Non-clinical |
| 26 | Healthcare Administrator or Executive | Hospital, clinic, or health system leadership and operations (CEO, COO, practice manager, ops). | Non-clinical |
| 27 | Healthcare Policy Professional | Federal, state, or NGO staff working on health policy, regulation, or government affairs. | Non-clinical |
| 28 | Healthcare Lawyer or Compliance Professional | Attorney, paralegal, compliance officer, or regulatory affairs specialist focused on healthcare. | Non-clinical |
| 29 | Patient Advocate or Navigator | Professional advocate, ombudsperson, or care navigator (advocating for patients as a profession, not as a patient). | Non-clinical |
| 30 | Healthcare Researcher or Academic | Clinical, translational, health-services, or social-science researcher; faculty member. | Non-clinical |
| 31 | Medical or Scientific Writer | Professional medical writer, science journalist, or health communications specialist. | Non-clinical |
| 32 | Medical Interpreter or Translator | Certified or working medical interpreter / translator (especially relevant to cross-border care). | Non-clinical |
| 33 | Healthcare Educator or Trainer | Faculty, CME/CE instructor, simulation educator, or clinical preceptor. | Non-clinical |
| 34 | Pharmaceutical, Biotech, or Device Professional | Industry professional in pharma, biotech, medtech, or diagnostics (commercial, medical affairs, R&D). | Non-clinical |
| 35 | Payer or Insurance Professional | Health plan, PBM, or benefits professional (medical director, UM, claims, actuarial). | Non-clinical |
| 36 | Developer in Healthcare | Software engineer, data engineer, ML engineer, or technical builder working on healthcare problems. | Builder/Tech |
| 37 | Healthcare Product Manager or Designer | PM, UX/UI designer, or service designer building healthcare products. | Builder/Tech |
| 38 | Health IT or Informatics Professional | Clinical informaticist, EHR analyst, interoperability/HL7/FHIR specialist, or health-data analyst. | Builder/Tech |
| 39 | Healthcare Founder or Operator | Founder, co-founder, or early operator at a healthcare-focused company or nonprofit. | Builder/Tech |
| 40 | Healthcare Investor or Funder | VC, angel, foundation program officer, or grantmaker funding healthcare work. | Builder/Tech |
| 41 | Student or Trainee (Non-Clinical) | Student in public health, health policy, health admin, biomedical engineering, or related non-clinical program. | Non-clinical |
| 42 | Other (please describe) | None of the above — free-text description required. | Other |

---

## Notes on judgment calls

- **Specialty collapsing.** The ~24 ABMS specialties (Cardiology, Oncology, Family Medicine, etc.) are NOT enumerated as separate dropdown rows. A single "Physician (MD/DO)" row plus a follow-up free-text "Specialty / focus area" field handles this. If structured filtering on specialty is needed later, add a second-step structured picker rather than expanding this list.
- **"Developer in Healthcare" placement.** Required by the brief. Placed as the first row of Builder/Tech with deliberately broad definition so a backend engineer at a payer, an indie dev building a clinic tool, and an ML researcher at a digital-health startup all see themselves in it. Adjacent Builder/Tech rows (PM/Designer, Informatics, Founder, Investor) prevent over-selection by non-engineers.
- **Trainee split.** Clinical trainees (Resident/Fellow, Medical Student, Nursing Student, Pharmacy Tech Trainee) live in their parent clinical clusters. A single "Student or Trainee (Non-Clinical)" row covers MPH / health policy / biomedical engineering students.
- **Mental Health vs. Therapist.** Separated because in US usage "therapist" without qualifier reads ambiguously. Mental Health Clinician is a deliberate union row (psychiatrist + psychologist + LCSW + LMFT + LPC); predictive search on "psych," "social work," "counselor" should land users on it.
- **Cross-border specifics.** "Medical Interpreter or Translator," "Community Health Worker or Promotor(a)," and "Doula, Midwife (non-CNM), or Birth Worker" are present specifically because they are load-bearing roles in US/Mexico cross-border care that a pure Doximity-style list would miss. The Spanish token "Promotor(a)" is kept even though the UI is English-only — it is the term this workforce uses to identify itself in the US.
- **No patient/affected-individual category.** Confirmed excluded. "Patient Advocate or Navigator" is a *professional* advocacy role; the definition explicitly says "advocating for patients as a profession, not as a patient" to prevent miscategorization at signup. Tighten this further if misuse becomes a moderation issue post-launch.
- **"Other" handling.** Last row, free-text required. The signup form should treat "Other" as a moderation-queue trigger rather than auto-verified — most likely vector for off-mission signups (including the patient-as-patient case the platform excludes).
- **Roles intentionally omitted.** Veterinarian, Acupuncturist, Naturopath — out of scope for the cross-border telehealth access mission. "Other" with free-text captures them; promote any to first-class rows post-launch based on demand.

---

## Implementation notes for week 2-3

- Store role as an enum-like field on the user model. Do **not** store the display label — store a stable identifier (e.g., `physician_md_do`) so labels can be rewritten without data migration.
- Predictive-text dropdown: search across label + tooltip text for forgiving lookup ("psych" → Mental Health Clinician).
- The free-text "Specialty / focus area" field should be optional, max ~80 chars, captured at signup and editable on profile.
- "Other" submissions go to a moderation queue (admin review before account is fully verified).

---

## Change log

- **May 2, 2026 (file created):** Locked taxonomy from research synthesizing NUCC, AMA/ABMS, Doximity, LinkedIn, Health eCareers/JAMA, HRSA, CMS, ONC/HIMSS sources. Resolved Task 1 in `research-backlog.md`. Replaces the "pending research" placeholders in `decisions-locked.md` (Privacy & identity model + Geographic & taxonomy foundation sections).
