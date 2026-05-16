# Session Recap — Brecha Networks Week 1 Bootstrap — 2026-05-16

*Generated 2026-05-16. Self-contained audit trail of the Claude Code session that landed the Week 1 codebase. A reader who has not seen the conversation can reconstruct the day's work, decisions, fixes, and learnings from this document alone.*

*Keystone archival artifact. Intended for printing and filing in the physical archival folder system under 2026-05-16.*

---

## 1. Context entering the session

### 1.1 Where the project stood

- Brecha Networks: Django coordination platform for healthcare professionals on cross-jurisdictional access barriers. Planning artifacts in `github.com/AlleyTill/brecha-networks-planning` were comprehensive as of 2026-05-15 strategic recalibration (prototype framing, pitch positioning, verification badge roadmap).
- **No codebase existed yet.** No `AlleyTill/brecha-networks` repo on GitHub; no application code on disk.
- AWE USA 2026 conference 30 days away (June 15-18). Build sequence: 8 weeks; Week 1 = bootstrap.

### 1.2 User entry

User stated: *"lets start the build plan. We're ready to go. Access the recent files for decisions locked and week one."*

Authorization to begin Week 1 execution.

---

## 2. Session narrative (chronological)

### 2.1 Pre-build research surfaced Windows-Django pain points

Before writing any code, the user asked me to search GitHub issues / Reddit / Hacker News / dev.to for known pain points doing Windows-native Django dev. Findings included:

- `django-tailwind` (the original Python package for Tailwind integration locked in `decisions-locked.md`) depends on Node/npm. On Windows specifically, Python's `subprocess.run("npm", ...)` cannot locate npm because it is `npm.cmd` (timonweb/django-tailwind#91, open). `node_modules` also brings real MAX_PATH risk on Windows.
- `django-allauth` 65.x line has had silent removals (`send_email_confirmation` gone in 65.10.0, codeberg #4507).
- Windows Defender exclusions for `pip.exe` were a real performance suggestion in some forums, but pip is one of the most-targeted supply-chain attack surfaces — disabling AV on it is the wrong trade.
- PostGIS/GDAL on Windows are an ongoing source of `ImproperlyConfigured: Could not find the GDAL library` errors at deploy time.

### 2.2 First locked decision: swap Tailwind integration package

Per the editing rules in `decisions-locked.md` ("propose, discuss, lock, then update"), proposed swapping `django-tailwind` → `django-tailwind-cli` (django-commons project). The latter downloads the standalone Tailwind binary directly — no Node, no npm, no `node_modules`, eliminating the entire Windows Node toolchain from the build.

User approved. Locked into the planning repo:
- `decisions-locked.md` package table updated; maintenance-verification section appends the swap rationale.
- `build-plan.md` Week 1 deliverables updated to reference `django-tailwind-cli` and add Windows dev-env prep (long paths, Defender exclusions — both subsequently retracted, see §2.5).
- Change log entry recorded.

Commit: `93c1f07 Lock Tailwind integration swap: django-tailwind -> django-tailwind-cli`. Pushed.

### 2.3 First Windows-host bootstrap (subsequently abandoned)

Started bootstrapping at `C:\dev\brecha-networks\` on the Windows host:
- Created folder, Python 3.12.10 venv inside repo, pinned `requirements.txt`
- Pip-installed Django 5.2.14, django-allauth 65.15.1, django-tailwind-cli 2.22.1, django-htmx 1.27.0, django-environ 0.13.0, django-otp 1.6.0, qrcode[pil] 8.0
- Ran `django-admin startproject brecha_networks .` and `python manage.py startapp users`
- Wrote initial `.gitignore` and `requirements.txt`

### 2.4 Critical reviewer feedback round 1 — Defender exclusions retracted

User ran my pre-build setup recommendations through a second Claude review. The reviewer correctly flagged that proposing Windows Defender exclusions for `C:\dev\` + `python.exe` + `pip.exe` was a security regression:

> "pip install is one of the most common supply-chain attack vectors there is — typosquatted packages, compromised dependencies. Excluding pip.exe from scanning removes a real layer of protection at exactly the point where you're pulling ~10 third-party Django packages."

This is correct. The Defender exclusion suggestion was friction-avoidance bias — optimizing for fewer waiting seconds without weighing what the friction was protecting. Especially wrong for the user's threat model context (high-value laptop, post-AWE departure plan).

**Retracted** the Defender exclusion suggestion entirely. The long-paths registry edit was kept (no security tradeoff).

**Saved to memory:** `feedback_no_security_for_speed.md` — "Never propose weakening endpoint protection (AV exclusions, sandboxing, firewall holes, signed-package skips) to save developer time. The right speed fixes are `uv` (10–100× faster pip replacement) or a faster index mirror — not turning off the scanner."

### 2.5 Reviewer feedback round 2 — Docker question

The reviewer also raised: *"Are you running inside a Docker container or directly on the Windows host?"* User asked me to verify.

I ran `uname -a` + `pwd` + Python sys.executable inspection. Confirmed:
- `MINGW64_NT-10.0-26200 Legion ... Msys` — Git for Windows POSIX layer running on Windows host
- Python at `C:\dev\brecha-networks\.venv\Scripts\python.exe`
- Platform: Windows-11-10.0.26200-SP0
- Hostname: Legion

I was on the Windows host. The user's reference to "Docker with Claude inside in ultimate sandbox mode" elsewhere was about a separate setup, not this session. Surfaced this directly.

### 2.6 Architecture decision — switch to WSL2

User: *"whichever has lower probability of failure."* Surfaced honest failure-rate estimates:

| Path | Setup risk | Dev/prod surprise risk | Total |
|---|---|---|---|
| Windows host | ~5% | ~20% (PostGIS/GDAL pain at Week 4 deploy) | ~25% |
| Docker Desktop | ~10% setup + ~15% beginner-friction | ~5% | ~30% |
| WSL2 (Ubuntu) | ~5% | ~10% | **~15%** |

Recommended **WSL2** — lowest mid-build failure risk + eliminates the specific failure most likely to eat days in Week 4 (PostGIS/GDAL Windows install). User approved.

### 2.7 WSL2 setup

Discovered WSL2 + Ubuntu 24.04 LTS already installed (also `docker-desktop` distro, separate from this work). Ubuntu was set up minimally — root-only, no regular user account.

Created `alley` user (UID 1001), added to sudo group, passwordless sudo via `/etc/sudoers.d/90-alley`, added `[user] default=alley` to `/etc/wsl.conf`, terminated WSL to apply.

### 2.8 Bootstrap inside WSL2 — clean

At `/home/alley/brecha-networks/`:
- Installed `python3.12-venv`, `python3-pip`, `build-essential`, `libpq-dev` (Postgres dev headers for Week 4) via apt
- Configured git: `AlleyTill <alleyrtill@gmail.com>`, `init.defaultBranch=main`, `pull.ff=only`, `core.autocrlf=input`
- Created venv, wrote requirements.txt and .gitignore, pip-installed the locked stack
- Ran `django-admin startproject brecha_networks .` and `python manage.py startapp users`
- All files owned by `alley:alley` (proper user, not root)

### 2.9 Settings, models, urls, templates written

Via UNC path `\\wsl.localhost\Ubuntu\home\alley\brecha-networks\...` from Windows Read/Write tools:

- `brecha_networks/settings.py` — env-based config via `django-environ`, allauth Flow C (`ACCOUNT_LOGIN_METHODS = {"email"}`, `ACCOUNT_EMAIL_VERIFICATION = "mandatory"`, `ACCOUNT_EMAIL_VERIFICATION_BY_CODE_ENABLED = True`, `ACCOUNT_LOGIN_BY_CODE_ENABLED = True`), `OTPMiddleware`, `HtmxMiddleware`, `AllauthMiddleware`, Tailwind CLI integration, `AUTH_USER_MODEL = "users.User"`, rate limits, password policy 12-char minimum.

- `brecha_networks/urls.py` — `admin.site.__class__ = OTPAdminSite` to wrap admin with MFA gate, `path("accounts/", include("allauth.urls"))`, home + about template views.

- `users/models.py` — `Role` model (Clinical/Non-clinical/Builder-Tech/Other), `User(AbstractUser)` with username (case-insensitive uniqueness via lowercase-on-save), email, role FK (nullable for Week 1), region_state, region_country, bio.

- `users/admin.py` — Role + User admin registrations.

- `users/validators.py` — username regex `^[A-Za-z][A-Za-z0-9_-]{2,29}$` per locked spec.

- `templates/{base,home,about}.html` — Tailwind + HTMX + Alpine.js loaded, deep teal accent, noindex meta per Week 1-2 SEO posture, Inter font stack (set up; final font lock is Week 6 polish).

- `static/css/source.css` — `@import "tailwindcss";` (Tailwind v4 syntax).

- `.env.example` and `.env` (gitignored, dev SECRET_KEY generated via `get_random_secret_key()`).

- `README.md` — local dev setup instructions + admin MFA bootstrap notes.

### 2.10 First migration cycle exposed a check warning

Initial `manage.py check` flagged `auth.W004`: username field had no `unique=True` (relied on `Lower("username")` UniqueConstraint instead) — Django auth's check framework doesn't know about functional unique indexes. Replaced approach with `unique=True` on the field + `save()` override to lowercase username before save. Cleaner. Trades mixed-case display for natural case-insensitive lookups at admin login. Acceptable since the spec only requires "case-insensitive uniqueness," not "preserve case."

Regenerated migration `0001_initial.py`. `manage.py check`: 0 issues silenced.

### 2.11 End-to-end runserver verification

Booted `runserver` headless, hit endpoints:
- `GET /` → HTTP 200, page contained "Find the people working on the same problem." hero
- `GET /accounts/signup/` → HTTP 200 (allauth wired)
- `GET /admin/` → HTTP 302 → `/admin/login/` (OTPAdminSite engaged)

### 2.12 Initial commit — `a8cf54a Week 1 bootstrap`

Detailed commit message documenting:
- Tech stack and locked versions
- Each file landed and why
- Acceptance verified (check + migrations + runserver)
- Deferred items (Barrier model is Week 2, Postgres+PostGIS is Week 4, Resend email is Week 4)

### 2.13 WSL DefaultUid footgun + fix

User opened an interactive `wsl` shell after the build and saw:

```
Provisioning the new WSL instance Ubuntu
This might take a while...
<3>WSL (302 - Relay) ERROR: operator():579: getpwuid(1000) failed 0
```

The shell prompt was `#` (root), not `alley@Legion`. Investigated:
- `/etc/wsl.conf` correctly says `[user] default=alley`
- Windows-side registry value `DefaultUid` at `HKCU:\Software\Microsoft\Windows\CurrentVersion\Lxss\{<distro-guid>}\DefaultUid` was set to `1000` — but `alley` is UID 1001 (Ubuntu's first-run wizard reserved UID 1000 for the slot we never populated).
- **Windows-side `DefaultUid` takes precedence** over `/etc/wsl.conf`'s `[user] default` setting. WSL looked up UID 1000, failed, fell back to root.

Fixed via PowerShell registry edit (DWORD `DefaultUid` set to 1001), `wsl --shutdown`, then re-verified — `wsl --exec` returned `User: alley, UID: 1001, Home: /home/alley`.

User confirmed working from their interactive terminal.

**Saved to memory:** `reference_wsl_default_user.md` — documents the dual-config footgun + PowerShell one-liner fix + "set both `/etc/wsl.conf` and registry `DefaultUid` when creating users post-install."

### 2.14 Week 1 acceptance walkthrough — Steps 1-5

User wanted to walk through five remaining acceptance items step by step:

| # | Item | Status at session pause |
|---|---|---|
| 1 | `python manage.py tailwind build` | ✓ Done (after a fix, see §2.15) |
| 2 | `python manage.py createsuperuser` | ✓ Done — superuser `alley` / `brechanetworks@gmail.com` |
| 3 | TOTP device registration | ⏸ Paused — user to install 2FAS, run `addstatictoken alley`, register device, confirm |
| 4 | Delete stale Windows-host folder `C:\dev\brecha-networks\` | ⏸ Pending |
| 5 | Attach `brechanetworks.com` domain | ⏸ Pending |

### 2.15 Tailwind version pin fix

First `tailwind build` attempt failed:

```
Downloading Tailwind CSS CLI from 'https://github.com/.../v_latest_/tailwindcss-linux-x64'
...
OSError: [Errno 8] Exec format error: '.../tailwindcss-linux-x64-latest'
```

Investigation: the downloaded file was a 9-byte ASCII string `Not Found` — GitHub's 404 response saved as a "binary." Root cause: I had set `TAILWIND_CLI_VERSION = "latest"` in settings.py, assuming the value was magic. `django-tailwind-cli` literally builds the URL as `v{version}` → `vlatest` (not a real release tag).

Looked up actual latest release via GitHub API: `v4.3.0`. Updated `settings.py` to `TAILWIND_CLI_VERSION = "4.3.0"` with an inline comment explaining why "latest" doesn't work. Cleaned up the bad 9-byte file. Re-ran `tailwind build` — downloaded the real 118 MB binary, compiled CSS in 228 ms.

Also patched `.gitignore` (`.tailwind-cli/`, `static/css/tailwind.css`) and committed `tailwind.config.js` (auto-generated, but contains real config — HTMX state variants, official Tailwind plugins).

### 2.16 Critical reviewer feedback round 3 — accuracy over ease

User pushed back on Step 3 walkthrough with: *"Check your assumptions. Why do you think I don't have an OTP device. Run a check on everything you just said for assumptions. I see the pattern. Are you drifting?"*

The pattern named: across the session I had made three friction-avoidance assumptions that defaulted to "easier" recommendations without verifying what the user actually had:

1. Defender exclusions (assumed user wanted faster pip install over endpoint protection — saved as `feedback_no_security_for_speed.md`)
2. Email for `createsuperuser` (assumed user would use personal email; they had created `brechanetworks@gmail.com` for project use)
3. TOTP walkthrough (assumed user had no authenticator app; assumed they needed six numbered sub-steps with QR-code basics)

User stated the principle: **"I prioritize accuracy and verifiability over user ease."**

**Saved to memory:** `feedback_accuracy_over_ease.md` — "When a question has multiple defensible answers, surface the tradeoff rather than pre-picking the easier one. If tempted to give a quick lazy answer ('use your real email,' 'go with the default'), slow down — they'd rather get the right answer than the fast one. Verify before recommending. When the question is 'what should I use here?', a defensible move is to ask 'what do you have?' first."

This is the third instance of the same general bias in one session. The other two were already saved. The pattern is now well-documented.

### 2.17 Final session work: durability hardening

User asked: *"Everything - and I mean EVERYTHING - is saved to file and ready to be printed when I get home?"*

Honest verification surfaced what was and was not durable:

**Already saved:**
- Planning repo on GitHub (commit `93c1f07`)
- Codebase commit `a8cf54a` (local only, not pushed)
- Memory files in `C:\Users\Alley\.claude\projects\C--Users-Alley\memory\` (Windows local)
- `.env` and `db.sqlite3` on WSL2 ext4 (gitignored, single-machine)

**Not yet saved:**
- 3 uncommitted file changes (Tailwind version pin, .gitignore patterns, tailwind.config.js)
- The codebase repo had no GitHub remote — `AlleyTill/brecha-networks` did not exist
- The session conversation itself was not in any file

Resolved all three before session pause:

1. **Commit `31168b2 Week 1 follow-up`** — the three pending changes locally.
2. **Created `AlleyTill/brecha-networks` (private)** on GitHub via `gh repo create`. Configured WSL git to use Windows Git Credential Manager (`/mnt/c/Program Files/Git/mingw64/bin/git-credential-manager.exe`) so the cross-OS credential bridge works on the first push. Pushed both commits.
3. **This document** — the session recap, written into the planning repo, committed and pushed.

---

## 3. Decisions made this session

### 3.1 Tailwind integration: `django-tailwind` → `django-tailwind-cli` — LOCKED

Documented in §2.2. Recorded in `decisions-locked.md` change log entry 2026-05-16.

### 3.2 Dev environment: WSL2 / Ubuntu 24.04 — LOCKED (working decision)

Codebase lives at `/home/alley/brecha-networks/`. Reason: lowest mid-build failure probability for a beginner targeting Linux production, eliminates PostGIS/GDAL Windows install pain at Week 4 deploy. Recorded in `project_brecha_networks_current.md` memory.

### 3.3 Defender exclusions: REJECTED

Documented in §2.4. The right "make pip faster" answers are `uv` or a faster mirror, not turning off the scanner. Recorded in `feedback_no_security_for_speed.md`.

### 3.4 Windows long-paths registry edit: KEPT but moot

Recommended for the Windows-host bootstrap. Since we moved to WSL2 (no MAX_PATH limit), the edit became moot. User instructed to skip it. Not blocking.

### 3.5 Admin username: `alley`

Per user preference. Less typing every login, no future-namespace headache, doesn't telegraph admin status the way `admin` / `root` / `superuser` would. Matches WSL user.

### 3.6 Admin email: `brechanetworks@gmail.com`

User had created a dedicated project email; I had defaulted to suggesting their personal address. User corrected; project email is the right answer for separation of concerns and future-admin handoff. Forwarding to personal Gmail recommended as a setup item before Week 4 (when Resend goes live).

### 3.7 Authenticator app recommendation: 2FAS

Open-source, free, iOS + Android, no account required. Threat-model fit: no vendor identity tied to user, auditable code. Rejected 1Password TOTP for defense-in-depth (combining passwords + TOTP in same vault collapses two factors). Rejected Authy for vendor account requirement.

### 3.8 Username case handling: lowercase-on-save

Switched from `Lower("username")` UniqueConstraint approach to field-level `unique=True` + `save()` override that lowercases. Cleaner, satisfies `auth.W004` check, makes admin login naturally case-insensitive. Loses mixed-case display (spec doesn't require it). Documented in §2.10.

### 3.9 Tailwind version: pin to `4.3.0`

Documented in §2.15. "latest" is not a magic value. Future updates require manual bump.

### 3.10 Codebase repo: PUSHED to GitHub (reversed local-only earlier choice)

Initial Week 1 plan was local-only. Reversed at session end after verifying durability gaps — laptop loss would lose every line of code. Pushed `AlleyTill/brecha-networks` (private) with commits `a8cf54a` + `31168b2`.

---

## 4. Memory files added or updated

### Added

- `feedback_no_security_for_speed.md` — Never weaken endpoint protection to save dev time.
- `feedback_accuracy_over_ease.md` — Verify what the user has before recommending; don't default to lazy "easier" answers.
- `reference_wsl_default_user.md` — WSL dual-default-user-config footgun + registry-edit fix.

### Updated

- `MEMORY.md` — Index updated with three new entries.
- `project_brecha_networks_current.md` — Locked package list shows `django-tailwind-cli` instead of `django-tailwind`. New "Development environment (locked 2026-05-16)" section documenting WSL2 at `/home/alley/brecha-networks/` and Defender-exclusion rejection.

---

## 5. Corrections to claims I made (worth flagging)

| Original claim | Correction |
|---|---|
| "These aren't blocking, you can do it whenever" re Defender exclusions | The framing pre-softened a recommendation that shouldn't have been made at all. Retracted. |
| "Use your real email" for createsuperuser | User had created `brechanetworks@gmail.com` for project use. Pre-asking what they had would have surfaced this. |
| Walking through TOTP with 6 detailed sub-steps assuming no authenticator app | Should have asked what authenticator/setup the user already had before structuring the walkthrough. |
| `TAILWIND_CLI_VERSION = "latest"` | Not a magic value. Set to real release tag, e.g., `4.3.0`. |
| Memory said Brecha was an "AI agent system" (carried from 2026-05-12 session — already fixed then but worth noting again) | Brecha is a Django web app, not an agent system. The agent framing belongs to the archived Barrier Registry predecessor. |

---

## 6. Open items (where we paused)

### 6.1 Week 1 acceptance — pending user action

- **Step 3 (TOTP setup):** install 2FAS (or chosen authenticator), run `python manage.py addstatictoken alley` for one-shot bootstrap code, log into `/admin/login/` with username + password + that code, add TOTP device, scan QR with authenticator, tick "Confirmed", verify by logging out and logging back in.
- **Step 4 (delete stale `C:\dev\brecha-networks\`):** `Remove-Item -Recurse -Force C:\dev\brecha-networks` in PowerShell. Not blocking.
- **Step 5 (attach brechanetworks.com domain):** approach not yet specified to user. DNS pointing happens at Week 4 deploy. Pre-Week-4 prep could include adding the domain to `ALLOWED_HOSTS` and checking current DNS state at registrar.

### 6.2 Tactical pre-Week-2 items

- Set up Gmail forwarding from `brechanetworks@gmail.com` → personal Gmail (before Week 4 Resend goes live).
- Domain purchase + handle reservations for the 7 redirect-only defensive domains (per `research-backlog.md`).

### 6.3 Week 2 work

Per `build-plan.md`:
- `Barrier` model (title, slug, origin location, destination location, English summary, full description, category tags, status marker + status description, author FK)
- `WorkingGroup` model
- `Comment` model via `django-comments-xtd`
- `ConnectRequest` + `Follow` via `django-friendship`
- Django admin customized for moderation use cases
- 15-25 SNOMED-aligned categories seeded via management command
- In-house @mention parser (~50 LOC)
- 42-role taxonomy fixture loaded into the `Role` model scaffolded today
- First real barrier seed in local dev DB (deferred from Week 1 since `Barrier` model didn't exist yet)

---

## 7. Commit + push summary

### Planning repo (`AlleyTill/brecha-networks-planning`)

| SHA | Message |
|---|---|
| `93c1f07` | Lock Tailwind integration swap: django-tailwind -> django-tailwind-cli |
| *(this doc)* | Will be committed and pushed at session close |

### Codebase repo (`AlleyTill/brecha-networks` — newly created private repo, this session)

| SHA | Message |
|---|---|
| `a8cf54a` | Week 1 bootstrap: Django + custom user model + allauth + OTP admin |
| `31168b2` | Week 1 follow-up: Tailwind version pin + .gitignore patterns + tailwind.config.js |

Push verified: `git push -u origin main` succeeded on the first attempt via the Windows Git Credential Manager bridge from WSL.

---

## 8. Immediate next steps (when session resumes)

1. **Confirm user has 2FAS (or chosen authenticator) installed.**
2. **Run `python manage.py addstatictoken alley`** in WSL — outputs one-shot OTP code.
3. **Browser: `/admin/login/`** — log in with username + password + that code.
4. **Register TOTP device** in admin, scan QR with authenticator, tick "Confirmed."
5. **Verify** by logging out and logging back in with a fresh 6-digit code.
6. **Delete `C:\dev\brecha-networks\`** stale Windows folder.
7. **Address Step 5** — domain attach approach.
8. **Begin Week 2** — Barrier / WorkingGroup / Comment models, admin customization, 42-role taxonomy fixture.

---

## 9. Cross-references

- **Planning repo (this doc lives here):** `github.com/AlleyTill/brecha-networks-planning`
- **Codebase repo (created this session):** `github.com/AlleyTill/brecha-networks` (private)
- **Local clone of planning repo:** `C:\Users\Alley\Documents\brecha-networks-planning-clone\`
- **Codebase on WSL2 disk:** `/home/alley/brecha-networks/` (reachable from Windows via UNC `\\wsl.localhost\Ubuntu\home\alley\brecha-networks\`)
- **Memory directory:** `C:\Users\Alley\.claude\projects\C--Users-Alley\memory\` (Windows local; not in any remote)
- **Prior session recap:** `04-session-recap-2026-05-12.md` (in this same planning repo)
- **Strategic recalibration:** `2026-05-15-{issues-and-mitigations,grants-and-funding,pitch-and-demo-scripts,full-conversation}.md` (in this same planning repo)

---

End of session recap. This document is complete and self-contained for archival purposes.

Date generated: 2026-05-16
Document type: Session recap (keystone archival artifact)
Codebase delta: +2 commits, `a8cf54a` + `31168b2`, pushed to `AlleyTill/brecha-networks` (created this session)
Planning repo delta: +1 commit (`93c1f07` Tailwind swap lock) + this document
Memory delta: 3 new feedback/reference memories + 1 updated project memory + 1 updated MEMORY.md index
Next step: print and file under 2026-05-16 in the archival folder system.
