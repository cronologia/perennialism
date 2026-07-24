# ADR-0001 — Template-derived build, one source of truth, and the ideas/order boundary

- **Status:** accepted (2026-07-24)
- **Context repo:** `cronologia/perennialism`
- **Depends on:** `cronologia/core` ADR-0001 (shared renderer contract),
  ADR-0002 (vendored glossary and skills), ADR-0003 (preservation / link-health
  split), ADR-0004 (Python agent tooling vs Node build)

## Context

This repo is one chronology project among peers. Three things about it had been
settled in practice — by the bootstrap commit, by later template ports, and by
the split with `cronologia/tariqa` — but were recorded only in prose, where they
drift. They are worth pinning: how the site is built, what may be hand-edited,
and which subject matter belongs here at all.

The subject makes the third question load-bearing rather than cosmetic. The
Traditionalist School's political receptions (Evola → the postwar radical right
→ Dugin → Bannon) are actively disputed scholarship, and the same figures —
Schuon, Nasr, Burckhardt, Lings — appear in the sibling `tariqa` dataset with a
different facet. Without a standing boundary, the two repos duplicate and then
contradict each other.

## Decision

1. **The build is `cronologia/core`'s template, consumed by copy.** Nothing is
   fetched at build time. Adopted from the template as of this ADR:
   - **renderers** in `build.js` — the glossary `[[term-id]]` marker link,
     the Wayback "archived" fallback link beside each reference, the `vizChips`
     nav strip, the **lineage tree** (typed edges: solid = direct discipleship
     or collaboration, dashed = influence / reception / cross-link) and the
     **branch timeline** (works by author);
   - **scripts** — `validate-data.js`, `sync-glossary-terms.js`,
     `archive-refs.js`, `check-links.js`;
   - **workflows** — `deploy.yml` (validate → test → build → docs drift check →
     Pages), `wayback.yml`, `link-health.yml`;
   - **tests** — `build-helpers`, `data-invariants`, `glossary-links`,
     `viz-renderers`.

   Deliberately **not** adopted: the template's i18n layer
   (`scripts/translate.js`, `data/i18n/`, the `/en/` `/es/` `/pt/` routes) —
   this site is single-locale English — and the `numbersChart` renderer, which
   has no data to draw. Template features are optional and data-driven, so their
   absence changes no output. Improvements made here are ported back up to the
   template rather than kept local.

2. **`data/chronology.json` is the single source of truth; everything else in
   the output path is generated.** `docs/` is compiled and committed;
   `data/archives.json` is written by `scripts/archive-refs.js`;
   `data/glossary-terms.json` is a pinned copy of the glossary's term ids
   written by `scripts/sync-glossary-terms.js`; `.claude/skills/` is a pinned
   copy of core's skills written by `core/tools/sync-skills.py`. None of these
   is hand-edited — each is regenerated and its diff committed. CI fails the
   build when `docs/` drifts from the data.

3. **The build is network-free.** Preservation (`archive-refs.js`) and link
   health (`check-links.js`) hit the live network and therefore run only
   out-of-band or from their scheduled workflows; `check-links.js` never edits
   the dataset, and an inconclusive result (403/429/5xx/timeout) is never
   recorded as a dead link.

4. **Standing subject boundary: this repo is the ideas, `tariqa` is the order.**
   Published works, journals, doctrines, the Evola political adaptation,
   academic institutionalization and reception belong here. Initiations,
   zawiyas, branch politics and the 1991 Bloomington affair belong to
   `cronologia/tariqa`. An event belongs to exactly one repo; the other
   cross-links it (rendered as a dashed lineage edge). Shared figures may appear
   in both, with different facets and without contradiction — checked with
   `core/tools/xref.py`. `cronologia/fsspx` keeps Catholic traditionalism and
   cross-links here; the three "traditionalisms" are never merged.

5. **Political lineage is attributed, never asserted, and structurally
   separated.** Evola's line is a *distinct* tree in the lineage visualization,
   not a branch of the Guénonian one; Huxley's 1945 "perennial philosophy" is
   rendered as an explicit name collision. Every claim of influence,
   affiliation or descent is written as a named scholar's or actor's dated
   claim, with the dissenting reviews cited alongside.

## Consequences

- A template change lands here as a port, not a rewrite: same renderer names,
  same optional-key contract, output byte-identical when a key is absent.
- Adopting i18n later is a data change plus two script copies, not a redesign —
  but until then, no `/en/` route exists and links must not assume one.
- Hand-editing `docs/`, `data/archives.json`, `data/glossary-terms.json` or
  `.claude/skills/` will be reverted by the next regeneration, and the docs
  drift check turns it into a red build.
- Contributors must decide *which repo an event belongs to* before writing it.
  The cost is a routing judgement per event; the benefit is that neither site
  half-tells the other's story.
- The site can be read offline and rebuilt with no network; the price is that
  snapshot coverage in `data/archives.json` is only as fresh as the last
  scheduled run (`dataset-query.py perennialism refs --unarchived` shows the
  gaps).
