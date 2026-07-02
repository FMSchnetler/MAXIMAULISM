# MAXIMAULISM

A website hosting one author's essays and poems, styled in a
"schizo-INFOWARS-crusader-lightning-apocalypse" aesthetic. Built with
[Hugo](https://gohugo.io) + [Tailwind CSS](https://tailwindcss.com) and deployed
to GitHub Pages.

**Live site:** https://fmschnetler.github.io/MAXIMAULISM/
**Slogan:** *"Life is sweetest near the bone."*

## Sections

The navigation labels *are* the domain terms — not "Essays" / "Poems".

| Section      | Content            | Path               |
| ------------ | ------------------ | ------------------ |
| **Rhetoric** | Essays (prose)     | `content/rhetoric` |
| **Eros**     | Poems (verse)      | `content/eros`     |
| **Fables**   | Fiction (stories)  | `content/fables`   |

*Fables* currently has no pieces; its section page shows a placeholder until
content is added.

## Local development

Requires **Hugo Extended** (v0.142.0+) and **Node.js** (v22) for the Tailwind
CLI.

```sh
npm ci            # install the Tailwind CLI
hugo server       # serve with live reload at http://localhost:1313
```

The site compiles Tailwind locally through Hugo's native `css.TailwindCSS`
pipeline and self-hosts its fonts — no CDN or third-party runtime dependency.

> The `.hugo_build.lock`, `/public/`, `/resources/`, and `hugo_stats.json`
> artifacts are regenerated on every build and are gitignored — don't commit
> them.

## Adding content

Create a Markdown file under the relevant `content/<section>/` directory. Verse
uses the `poem` shortcode so the author's line breaks and punctuation survive
verbatim (Goldmark smart-punctuation is disabled by design).

## Deployment

Pushing to `main` triggers [`.github/workflows/hugo.yml`](.github/workflows/hugo.yml),
which builds with Hugo Extended and publishes to GitHub Pages via GitHub
Actions. The `baseURL` is derived from the Pages deployment automatically — the
workflow uses **no secrets**.

To enable deployment on a fresh clone: **repo → Settings → Pages → Build and
deployment → Source → GitHub Actions.**

> **Note — the repo's "About" website link is set manually.** This is a project
> site, so its URL includes the repo path: `https://fmschnetler.github.io/MAXIMAULISM/`.
> GitHub does *not* fill this in for you. Set it under **repo page → About (⚙️) →
> Website**; if it's blank or set to `https://fmschnetler.github.io/` it will 404,
> even though the deployed site itself is fine.

## Project docs

- [`CONTEXT.md`](CONTEXT.md) — project overview and ubiquitous language.
- [`docs/SECURITY.md`](docs/SECURITY.md) — security posture (secrets, CI, privacy).
- [`docs/adr/`](docs/adr/) — architecture decision records.

## License

UNLICENSED — all rights reserved. The essays, poems, and hero image treatment
are the author's; the hero image derives from a public-domain 1843 portrait.
