# AGENTS.md (template — adapt per project)

Operating guide for AI coding agents (and humans) working in this repository.
Read this and `context.md` before making changes. The shared method lives in
`cronologia/core` (skills: sourcing-rules, bootstrap-project, mine-video,
dossier-research); the architecture rationale in `cronologia/fsp` → `docs/adrs/`.

## What this project is

A compiled static website documenting the chronology of **perennialism / the Traditionalist School as an intellectual movement** — works, journals, ideas and reception. The Maryamiyya ORDER is the sibling project `cronologia/tariqa`; events belong to exactly one of the two.
A single JSON file is the source of truth; a zero-dependency Node script
compiles it into static HTML served by GitHub Pages.

## Repository map

```
data/chronology.json     SOURCE OF TRUTH — facts, events, figures, organizations, references (hand-edited)
src/styles.css           Stylesheet (copied into the build)
scripts/validate-data.js Schema check (runs in CI before the build)
build.js                 Compiler: data/chronology.json -> docs/
test/                    node:test suites (helpers + data invariants + drift check)
.github/workflows/deploy.yml  CI: validate, test, build, drift check, Pages deploy (main + manual dispatch)
docs/                    COMPILED OUTPUT, served by GitHub Pages (committed)
```

## Working agreements

1. **Edit data, not output.** Change `data/chronology.json`, run
   `node build.js`, commit the regenerated `docs/` in the same change.
2. **Keep the build green.** `node scripts/validate-data.js`, `node --test`
   and `node build.js` must all pass; CI fails if `docs/` drifts.
3. **Cite every fact; flag every uncertainty; attribute every contested
   characterization.** The validator enforces non-empty `sources[]`.
4. **A merged PR is finished** — branch fresh from `main` for new work.

## Data quality & sourcing rules

Beyond the family's five core rules (sourcing-rules skill in
`cronologia/core`):

- **Respect the boundary.** Order-internal events (initiations, zawiyas,
  branch politics, the 1991 affair) live in `cronologia/tariqa`; this repo
  covers the published-ideas layer. Link, don't duplicate.
- **Three perennialisms, kept apart**: the Guénonian Traditionalist School ≠
  Aldous Huxley's popular 'perennial philosophy' (1945) ≠ the older
  philosophia perennis of Steuco/Leibniz. The site exists partly to keep
  these distinct.
- **The Evola line and its political receptions** (postwar radical right,
  Dugin, Bannon, the alt-right reading) are the most contested terrain:
  every lineage claim attributed to the scholar or actor making it
  (Sedgwick and Teitelbaum are the academic anchors), never asserted.
- **Living figures** (Nasr and others) get BLP-grade care; public roles and
  published works only.
- **Contested affiliations** (Eliade's debated relation to the school; royal
  or celebrity 'sympathies') are reported as attributed scholarly claims
  about influence — never as membership assertions.
