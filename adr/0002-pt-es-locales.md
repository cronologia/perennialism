# ADR-0002 — Portuguese and Spanish locales adopted from the core template

- **Status:** accepted (2026-07-25)
- **Context repo:** `cronologia/perennialism`
- **Implements:** `cronologia/perennialism` #25
- **Depends on:** `cronologia/core` #9 and `core/template/adrs/0001-multilingual.md`
  (the org-wide multilingual decision), this repo's ADR-0001 (template-derived
  build, one source of truth)

## Context

The site was English-only. The reach goal for Portuguese- and Spanish-speaking
readers is *being found in those languages*, which means independently indexable
per-locale pages, not a client-side toggle. The shared design was already built
and merged in `core/template`; this repo only had to adopt it.

The subject matter raises the stakes. This dataset carries contested scholarship
(the Evola line and its postwar radical-right receptions, the Eliade debate, the
royal reception, the Brazilian Carvalho material) where every characterization is
attributed rather than asserted. A translation that drops a hedge or an
attribution turns a recorded claim into the site's own voice.

## Decision

1. **Adopt, do not re-implement.** `build.js`, `scripts/translate.js`, the
   `.lang-switch` / `.i18n-disclaimer` styles and the locale tests are ported
   from `core/template` unchanged. The only thing this repo adds is
   `meta.siteUrl` and the two translation caches. Divergence from the template
   is a defect, not a local flavour.
2. **`en` + `pt` + `es`,** with the locale as a path segment after the project
   (`/perennialism/{en,pt,es}/`) and `docs/index.html` as a redirect stub, so
   every existing `/perennialism/` link keeps working. English moves to
   `/perennialism/en/` and stays authoritative.
3. **English is unchanged in meaning.** Localization happens at the data level
   against a cache keyed by the English source string; the English dictionary is
   empty, so `/en/` is the identity transform on content. This is asserted by a
   test, not by inspection.
4. **Translation is machine translation with no human-review gate,** per core#9 —
   which is exactly why every non-English page carries a *visible* disclaimer in
   the body naming English as the reference version.
5. **What is not translated:** proper names, place-name components without an
   established target-language exonym, organization names, the whole
   `references[]` array (titles, publishers, URLs, ids), dates, book and journal
   titles (they are cited objects, so they stay in the form the English text
   uses), Latin terms of art (`philosophia perennis`, `religio perennis`,
   `Tertium non datur`), and primary-source quotations, which stay in their
   original language.
6. **Hedges and attributions are load-bearing and survive translation.** "never a
   formal member", "per the Gifford records", "attributed to the scholar or actor
   making it", "a debate, not a verdict", "(to verify)" and the like are
   translated as hedges of the same strength, with the attribution intact. This
   is the rule a future re-translation must not relax.
7. **`[[term-id]]` glossary markers are preserved exactly.** For the
   `[[id|visible text]]` form only the visible text is translated; the id is a
   URL. `scripts/validate-data.js` fails the build on an unknown id.

## Consequences

- `data/i18n/pt.json` and `data/i18n/es.json` are **generated data**: they are
  owned by `scripts/translate.js` (`--stats` reports coverage) and are re-authored
  when the English content changes, never patched by hand in isolation.
- Adding a page to this site means adding its route to `ROUTES` in `build.js`, or
  the sitemap and the hreflang alternates go stale.
- A new or reworded English string silently falls back to English on the
  non-English pages until the caches are refreshed — visible as a coverage drop
  in `node scripts/translate.js --stats`, which is the intended signal.
