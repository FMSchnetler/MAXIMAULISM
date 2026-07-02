# Security notes — MAXIMAULISM

MAXIMAULISM is a **static Hugo site on GitHub Pages**: no server, no database, no
user input, nothing to exploit at runtime. The real risks are about **secrets, the
build pipeline, and privacy** — not intrusion. This file records them and how they
are handled.

## 1. Secret leakage — the Gemini API key (highest, project-specific)

A Gemini API key lives in a local `.env` (never committed — `.gitignore` blocks
`.env`, `.env.*`, `*.key`, etc.; verified). It is a live, billable credential.

- **Residual risk:** a forced `git add -f`, pasting it into a committed file, or a
  single leak into commit history.
- **Recommended:** because the site does **not** use Gemini
  (see `docs/adr/0002`), the cleanest fix is to **revoke/delete the key** — an
  unused credential is pure liability. If kept, enable GitHub secret-scanning
  **push protection** on the repo.

## 2. The GitHub Actions build pipeline (public repo)

Public repos accept PRs from strangers; a malicious PR can try to alter a workflow
to exfiltrate secrets.

- **Defense:** the deploy workflow uses **no secrets at all** (`baseURL` is
  auto-derived, no Gemini in CI), least-privilege permissions
  (`contents: read`, `pages: write`, `id-token: write`), and pinned action
  versions. Keeping secrets out of CI is the whole defense — do not add any.

## 3. Supply chain (npm + CDNs)

Tailwind pulls npm dependencies that run at build time.

- **Defense:** commit `package-lock.json`, use `npm ci`, keep dependencies
  minimal, optionally enable Dependabot.
- The *prototypes* load Tailwind + fonts from CDNs; the **production site compiles
  Tailwind locally and self-hosts fonts** — no third-party runtime dependency and
  no leaking visitor IPs to Google Fonts.

## 4. Privacy, permanence, consent (non-technical, but real)

Once public, the author's essays and poems are **indexed by search engines and
archived** (e.g. the Wayback Machine) — effectively permanent even if the repo is
later deleted, and tied to whatever name/handle is published.

- Confirm the author consents to **public, permanent, named** publication.
- The essay *"How to be Alive, or Not Kill Yourself"* touches suicide; consider a
  brief content note.

## 5. Custom domain (only if added later)

If a domain is pointed at Pages and later abandoned without removing its DNS
record, it is exposed to **subdomain takeover**. Enforce HTTPS (Pages supports it)
and clean up DNS on any migration.

---

**Already in place:** `.gitignore` secret net (verified), `.env.example` so no real
key is ever needed in-repo, no secrets in CI, and a public-domain hero image (no
licensing risk).
