# Use a public-domain painting for the Crusader, not AI generation

The original plan was to generate the crusader hero image via the Gemini API
(a plain base image, then CSS/SVG lightning-eye effects on top). We instead use a
cropped public-domain 1843 portrait of **Bohémond I of Antioch** (Merry-Joseph
Blondel, Versailles), with the name plate removed, and layer the same CSS effects
(CRT scanlines, lightning eyes, glow) over it.

**Why:** the painting gives higher fidelity than a draft-quality generation, at
zero cost and with no per-build nondeterminism; it needs no API key at build time;
and it carries no image-licensing risk (public domain).

**Consequence:** the Gemini scaffolding — `.env.example` `GEMINI_API_KEY`, the
`.gitignore` secret rules — remains in the repo but is **unused**. It is retained
deliberately in case we revisit AI generation later; this ADR explains why that
scaffolding exists without any code that calls Gemini. The live key can be revoked.
