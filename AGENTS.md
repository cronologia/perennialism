# AGENTS.md

Operating guide for AI coding agents (and humans) working in this repository.
Read this and [`context.md`](context.md) before making changes; then load the
skill that matches the job (they are vendored in `.claude/skills/`). The shared
method and machinery live in [`cronologia/core`](https://github.com/cronologia/core);
the family interdependency map is `cronologia/core/DEPENDENCIES.md`; the
decisions that govern *this* repo are in [`adr/`](adr/).

## What this project is

A compiled static website documenting the chronology of **perennialism / the Traditionalist School as an intellectual movement** — works, journals, ideas and reception. The Maryamiyya ORDER is the sibling project `cronologia/tariqa`; events belong to exactly one of the two.
A single JSON file is the source of truth; a zero-dependency Node script
compiles it into static HTML served by GitHub Pages. See
[`adr/0001-project-architecture-and-boundaries.md`](adr/0001-project-architecture-and-boundaries.md).
The site is published in three locales — `docs/en/`, `docs/pt/`, `docs/es/`, with
`docs/index.html` as a redirect stub. English is authoritative and hand-written;
`pt`/`es` are machine-translated from the committed caches in `data/i18n/` and
carry a visible disclaimer. See
[`adr/0002-pt-es-locales.md`](adr/0002-pt-es-locales.md).

## Repository map

```
data/chronology.json          SOURCE OF TRUTH — meta, facts, events, figures, organizations,
                              disambiguation, lineage, branchTimeline, references (hand-edited)
data/archives.json            GENERATED — Wayback snapshots per reference URL (scripts/archive-refs.js)
data/glossary-terms.json      GENERATED — pinned copy of the cronologia/glossary term ids
data/i18n/{pt,es}.json        GENERATED — machine-translation caches keyed by the English
                              source string (scripts/translate.js); English is authoritative
src/styles.css                Stylesheet (copied into the build)
build.js                      Compiler: data/chronology.json -> docs/{en,pt,es}/ + root
                              redirect stub, sitemap.xml, robots.txt
scripts/validate-data.js      Schema check (runs in CI before the build)
scripts/translate.js          Translation-cache coverage/normalizer for data/i18n/ (--stats)
scripts/archive-refs.js       Wayback preservation for references[] (out-of-band / CI only)
scripts/check-links.js        Link-health checker (out-of-band / CI only; never edits data)
scripts/sync-glossary-terms.js  Refreshes data/glossary-terms.json from cronologia/glossary
test/                         node:test suites (helpers, data invariants, glossary links,
                              viz renderers, drift check)
KEYWORDS.md                   Search vocabulary + measured search traps. PART-GENERATED:
                              the block between the build-keywords.py markers comes from
                              data/chronology.json; the hand-written "## Search traps"
                              section outside it survives regeneration. A finding aid, not
                              a dataset — it makes no claims about the world
adr/                          Architecture decision records for this repo
.claude/skills/               GENERATED — vendored copy of cronologia/core skills/
.github/workflows/deploy.yml       CI: validate, test, build, drift check, Pages deploy (main + manual dispatch)
.github/workflows/wayback.yml      Scheduled Wayback capture -> data/archives.json
.github/workflows/link-health.yml  Scheduled link-health report (opens/updates an issue)
docs/                         COMPILED OUTPUT, served by GitHub Pages (committed).
                              docs/index.html is a redirect stub; the real pages live under
                              docs/en/, docs/pt/, docs/es/
```

`data/archives.json`, `data/glossary-terms.json`, `data/i18n/*.json`, `docs/` and
`.claude/skills/` are **generated**. Never hand-edit them: re-run the script that produces them and
commit the diff.

## Sourcing rules — read first

The family's five rules (cite-or-flag; attribute, don't assert; sources across
the spectrum; date time-sensitive statuses; testimony is a perspective, not a
fact source) are the **`sourcing-rules` skill**, vendored here at
[`.claude/skills/sourcing-rules/SKILL.md`](.claude/skills/sourcing-rules/SKILL.md).
Load it before touching `data/chronology.json` or writing site copy. It is
canonical in `cronologia/core/skills/sourcing-rules/`; fixes go there and are
re-synced, never edited locally.

## Which skills apply here, and when

Ten skills are vendored under `.claude/skills/`. The ones that matter for work in
this repo:

| Skill | Load it when |
|---|---|
| `sourcing-rules` | **Always**, before any data edit or site copy. |
| `data-edit` | Editing `data/chronology.json` — query first, then validate / test / build, and commit `docs/` in the same change. |
| `ingest-report` | Turning a research dossier into dataset rows: only verified-with-a-source items land, the report's attribution language is preserved verbatim, and what stayed out is said so on the ticket. |
| `net-access` | Any fetching — the access ladder, UA-filtering vs country-gating, 403/429/5xx = INCONCLUSIVE. Never route around the proxy. |
| `preserve-sources` | Archiving and link health: `refs --unarchived`, `scripts/archive-refs.js`, `scripts/check-links.js`, and what belongs in `cronologia/archive` rather than here. |
| `adopt-template` | Porting a renderer, validator rule or style from `cronologia/core/template` — additions stay data-driven and OPTIONAL so output is byte-identical when the key is absent. |
| `release-work` | Branching, fast-forwarding, committing and pushing; one repo, one committer, per wave. |
| `dossier-research` | Research passes that produce a report for later ingestion (this is a research repo). |
| `mine-video` | Mining a talk, interview or podcast — its claims are perspectives, not fact sources, until independently corroborated. Read [`KEYWORDS.md`](KEYWORDS.md) `## Search traps` first for the ASR manglings already observed, and record any new one you hit there. |
| `bootstrap-project` | Only when standing up a *new* family repo; not needed for work here. |

## Agent-side tooling (`cronologia/core/tools`, Python 3 stdlib)

Read-only, never run in CI, never edits a dataset. **Query before reading whole
files** — `data/chronology.json` is large, and reading it end to end spends the
context you need for judgement.

**Before you search anything — a corpus, a transcript, the dataset — read
[`KEYWORDS.md`](KEYWORDS.md), especially its `## Search traps` section.** The
obvious term is routinely the wrong one here: `Guenon` unaccented returns zero
prose hits in this dataset, `Traditionalist` returns zero across the whole
7.16M-word COF corpus (which writes `tradicionalismo`), bare `sufi` in a
Portuguese corpus is ~96% `suficiente`, and the auto-caption corpora mangle
every proper name in the subject (`Guénon` → `genon`/`Guinon`/`Ganon`, `Schuon`
→ `Shuan`/`Chuon`, `Coomaraswamy` → `Comarassame`). A negative result from an
unlisted search term is not evidence of absence. When a search teaches you
something new, add it to `## Search traps` — that section is the accumulated
record and is never regenerated away.

```bash
python3 core/tools/dataset-query.py perennialism stats             # collection sizes, year span, unarchived count
python3 core/tools/dataset-query.py perennialism find guenon       # locators, not whole-file reads
python3 core/tools/dataset-query.py perennialism event 1934        # a year or a year-year range
python3 core/tools/dataset-query.py perennialism refs --unarchived # preservation gaps
python3 core/tools/unverified-report.py perennialism --markdown    # every unverified flag, as a ticket checklist
python3 core/tools/mine-prep.py <transcript>                       # transcript -> candidate sheet with offsets
python3 core/tools/xref.py --repos tariqa,perennialism,fsspx       # cross-repo consistency on shared entities
python3 core/tools/sync-skills.py perennialism --check             # is .claude/skills/ stale?
python3 core/tools/build-keywords.py perennialism --out KEYWORDS.md  # refresh KEYWORDS.md's generated block
```

Run `xref.py` whenever you touch a figure or organization that `tariqa` or
`fsspx` also carry (Guénon, Schuon, Nasr, Evola, the orders). It flags
`CONTRADICTION` / `DIFFERS` for review and never auto-resolves — resolving one is
a sourcing decision backed by citations.

## The operational loop

```bash
node scripts/validate-data.js && node --test && node build.js
git add data/ docs/ && git commit      # data + regenerated docs in ONE commit
```

CI fails if `docs/` drifts from the data. A documentation-only change must leave
`docs/` byte-identical — verify with `git diff --stat -- docs/` after building.

The build is **network-free**. `archive-refs.js` and `check-links.js` hit the
live network and run only out-of-band or from their scheduled workflows.

## Working agreements

1. **Edit data, not output.** Change `data/chronology.json`, run
   `node build.js`, commit the regenerated `docs/` in the same change.
2. **Keep the build green.** `node scripts/validate-data.js`, `node --test`
   and `node build.js` must all pass; CI fails if `docs/` drifts.
3. **Cite every fact; flag every uncertainty; attribute every contested
   characterization.** The validator enforces non-empty `sources[]`.
4. **A merged PR is finished** — branch fresh from `main` for new work.
5. **One repo, one committer, per wave.** Exactly one agent owns this dataset at
   a time; serialize instead of racing.

## Data quality & sourcing rules

Beyond the family's five core rules (sourcing-rules skill in
`cronologia/core`, vendored at `.claude/skills/sourcing-rules/`):

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

## Where this repo sits in the family

The canonical map is **`cronologia/core/DEPENDENCIES.md`** — read it rather than
re-deriving it. In short: this repo consumes `core` (template, skills, tools —
all by copy, nothing fetched at build time), links into `cronologia/glossary`
with `[[term-id]]` markers validated offline against the pinned
`data/glossary-terms.json`, and cites shared sources vaulted in the private
`cronologia/archive` (reader-facing citations are always the original URL plus
its Wayback snapshot, never a raw archive URL).

This repo's own boundaries and cross-links:

- **`tariqa` (the order) ↔ `perennialism` (the ideas).** The standing split: the
  Maryamiyya's initiatic history, zawiyas, branch politics and the 1991
  Bloomington affair belong to tariqa; works, journals, doctrines, the Evola
  political adaptation, academic institutionalization and reception belong here.
  Shared figures (Schuon, Nasr, Burckhardt, Lings) legitimately appear in both —
  with *different facets* — and the two datasets must not contradict each other
  about them. An event belongs to exactly one repo; the other links.
- **`fsspx` (Catholic traditionalism) cross-links this repo** where a figure or
  claim touches both, and the three "traditionalisms" stay apart: Catholic
  traditionalism ≠ the Guénon–Schuon Traditionalist School ≠ Evola's political
  Traditionalism. Do not import fsspx's subject matter here.
- **The political-lineage attribution rule is a hard boundary, not a style
  preference.** Evola → the postwar radical right → Dugin → Bannon → the
  alt-right reading is a chain of *claims by named scholars and actors*
  (Sedgwick, Teitelbaum, and the reviewers who dispute them). Every link in it is
  written as someone's attributed, dated claim, with the dissent recorded
  alongside. The site's own voice never asserts influence, affiliation or
  descent.

## Searching the vaulted transcripts

The `corpus-search` skill indexes every collection in `cronologia/archive` —
including the general `transcripts/` set this project mines — and searches them
as one corpus. Use it before reporting that anything is absent: a sweep over one
collection reported as a corpus-wide zero is a false negative, and has been
published as a finding once. Search the CLAIM rather than the NAME (the ASR
destroys names), and quote the scope with any zero you report.
