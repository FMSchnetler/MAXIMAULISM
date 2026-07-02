# Compile Tailwind v4 through Hugo's native asset pipeline

We compile Tailwind CSS v4 through Hugo's built-in `css.TailwindCSS` asset
pipeline (Hugo Extended), rather than running a standalone Tailwind CLI watcher
alongside Hugo or using the legacy PostCSS + Tailwind v3 setup.

**Why:** the whole build is a single `hugo` command (locally and in CI), it is the
current path Hugo's own docs steer toward, and it keeps configuration minimal.

**Consequence / trade-off:** it requires **Hugo Extended**, and Tailwind must be
told which utility classes the templates actually use. We do that with Hugo's
build stats (`[build.buildStats] enable = true`) plus a `cachebuster` rule and a
mount of `hugo_stats.json` into `assets/`, referenced from `main.css` via
`@source`. That config is non-obvious, hence this record — do not delete the
buildStats/cachebuster block thinking it is unused; without it Tailwind purges
classes it can't see and the styles silently break.
