# Security overview — MAXIMAULISM

MAXIMAULISM is a **static Hugo site on GitHub Pages**: no server, no database, no
runtime user input, nothing to exploit at request time. The content is authored by
a single trusted author and committed to the repo. The real risks are about
**secrets, the build pipeline, privacy, and any future contribution path** — not
intrusion. This file is the current record of them and how they are handled.

**Status:** the site is built, pushed to the public repo
(`github.com/FMSchnetler/MAXIMAULISM`), and a focused security review of the full
site diff was run — **no exploitable findings.** The single forward-looking note
from that review is captured in §6.

## 1. Secret leakage — the Gemini API key (highest, project-specific)

A Gemini API key lives in a local `.env` (**never committed** — `.gitignore` blocks
`.env`, `.env.*`, `*.key`, etc.; verified absent from all git history). It is a
live, billable credential.

- **Residual risk:** a forced `git add -f`, pasting it into a committed file, or a
  single leak into commit history.
- **Action (still open):** the site does **not** use Gemini (see `docs/adr/0002`),
  so the cleanest fix is to **revoke/delete the key** — an unused credential is
  pure liability. If kept, enable GitHub secret-scanning **push protection**.

## 2. The GitHub Actions build pipeline (public repo) — implemented

The deploy workflow (`.github/workflows/hugo.yml`) is live. Public repos accept PRs
from strangers, so a malicious PR could try to alter a workflow to exfiltrate
secrets.

- **Defense in place:** the workflow uses **no secrets at all** (`baseURL` is
  auto-derived; no Gemini in CI), least-privilege permissions
  (`contents: read`, `pages: write`, `id-token: write`), pinned action versions,
  and trusted triggers only (`push` to `main` + `workflow_dispatch` — no
  `pull_request_target`, no fork inputs).
- The security review confirmed **no injectable `${{ }}` expressions** in `run:`
  steps (only trusted `runner.temp`, `steps.pages.outputs.base_url`, and a repo
  `env` version). Keeping secrets out of CI is the whole defense — do not add any.

## 3. Supply chain (npm + fonts) — implemented

Tailwind pulls npm dependencies that run at build time.

- **Defense in place:** `package-lock.json` is committed, CI uses `npm ci`, and the
  dependency surface is minimal (`@tailwindcss/cli`). Optionally enable Dependabot.
- The *prototypes* loaded Tailwind + fonts from CDNs; the **production site
  compiles Tailwind locally and self-hosts the fonts** (`static/fonts/`) — no
  third-party runtime dependency and no leaking visitor IPs to Google Fonts.

## 4. Privacy, permanence, consent (non-technical, but real) — now live

The repo is public, so the author's essays and poems are **indexed by search
engines and archived** (e.g. the Wayback Machine) — effectively permanent even if
the repo is later deleted, and tied to whatever name/handle is published.

- Confirm the author consents to **public, permanent, named** publication.
- The essay *"How to be Alive, or Not Kill Yourself"* touches suicide; consider a
  brief content note.

## 5. Custom domain (only if added later)

If a domain is pointed at Pages and later abandoned without removing its DNS
record, it is exposed to **subdomain takeover**. Enforce HTTPS (Pages supports it)
and clean up DNS on any migration.

## 6. Future contribution path — XSS sink to design around (from the review)

The site intentionally renders author Markdown with Goldmark `unsafe = true` and a
`safeHTML` verse shortcode. This is safe **today** because all content is trusted
and committed by hand. The moment an **untrusted-contributor** workflow is added
(the deferred "let someone else add stuff" idea), that same `unsafe = true` +
`.Content` path becomes a **stored-XSS sink**.

- **Before that ships:** sanitize contributed HTML/Markdown, or keep contributors
  behind a trusted review/merge gate, or render untrusted content with `unsafe`
  disabled. Re-run a security review at that point.

---

**Already in place:** `.gitignore` secret net (verified, `.env` never committed),
`.env.example` so no real key is ever needed in-repo, no secrets in CI,
locally-compiled Tailwind + self-hosted fonts, and a public-domain hero image (no
licensing risk).
