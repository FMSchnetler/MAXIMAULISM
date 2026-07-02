# Host on a GitHub Pages project site, deployed by GitHub Actions

We host MAXIMAULISM as a GitHub Pages **project** site — repo named `MAXIMAULISM`,
served at `https://fmschnetler.github.io/MAXIMAULISM/` — built and deployed by a
GitHub Actions workflow, rather than as a user site (`<user>.github.io`) or a
custom domain.

**Why:** setup is trivial, it needs no domain purchase, and Actions builds from
source so we never commit the generated `/public` output. The Actions deploy also
derives `baseURL` automatically, so no secret or username is needed in CI.

**Consequence / trade-off:** the site lives under a `/MAXIMAULISM/` path prefix, so
`baseURL` and all asset/link references must respect that subpath (Hugo handles
this when links go through `relURL`/`.RelPermalink`). A custom domain can be added
later with a `CNAME` file + DNS change — at which point the subpath goes away.
