# Project context

Domain background for this repository. Pair with [`AGENTS.md`](AGENTS.md) (how
to work here) and [`adr/`](adr/) (why the repo is built the way it is); the
family's method lives in `cronologia/core`, vendored at `.claude/skills/` —
load `sourcing-rules` first.

## The subject

**Perennialism / the Traditionalist School**: the current of ideas founded by
**René Guénon** (corpus 1921–1951), developed by **Ananda Coomaraswamy** (the
metaphysical turn, 1932) and **Frithjof Schuon** ('The Transcendent Unity of
Religions', 1948), holding that all orthodox religions transmit one perennial
truth and reading modernity as its loss. House journal: *Études
Traditionnelles* (Paris, 1936–1992). Academic high-water mark: Nasr's 1981
Gifford Lectures (first Muslim Gifford lecturer, per the Gifford records).
Unexpected exports: Lord Northbourne coined "organic farming" (1940); the
Temenos Academy carries the arts wing under royal patronage.

**This repo covers the school of ideas.** The initiatic/organizational
history of Schuon's order is the sibling `cronologia/tariqa` — events belong
to exactly one of the two projects.

## Who it is for

Readers trying to get the lineage straight: students and researchers of
twentieth-century esotericism and religious studies, journalists reaching for
"Traditionalism" after a Bannon or Dugin news cycle, and readers of Guénon,
Schuon or Nasr who want dates and first editions they can check. The site
states dated, cited facts and attributes every contested reading; it does not
argue for or against the school.

## The contested terrain

- **Evola's political adaptation** ('Revolt Against the Modern World', 1934;
  the postwar Italian radical right; Dugin's Fourth Political Theory; the
  2017 Bannon–Evola news cycle; Teitelbaum's 'War for Eternity' 2020 with its
  disputed network thesis) — every lineage claim attributed; Sedgwick and
  Teitelbaum are the scholarly anchors, and reviewers' skepticism about the
  'network' is recorded too. The lineage visualization keeps this as a
  **separate tree**, not a branch of the Guénonian one.
- **Name collisions**: Guénonian Traditionalism ≠ Huxley's 'Perennial
  Philosophy' (1945) ≠ Steuco/Leibniz's philosophia perennis ≠ Catholic
  traditionalism (the fsspx project).
- **Contested affiliations**: Eliade (Sedgwick's 'soft Traditionalist' vs the
  'myth of affinity' critics — present the debate); King Charles III
  (documented Temenos patronage; everything beyond it is commentators'
  attributed reading, never membership).
- **Living figures** (Nasr and others): BLP-grade care, public roles and
  published works only.

## State of the dataset

`data/chronology.json` currently holds **41 events (1540–2023)**, 14 figures,
6 organizations, 6 summary facts, a 9-item disambiguation block and **64
references** — encyclopedia, academic, primary, analysis, official sites, news,
plus the mined-source types added in July 2026 (*academic interview*, *primary
footage*, *commentary*, *testimony*). Every event carries `dateVerified: true`
and a non-empty `sources[]`; six fields are flagged as unverified
(`python3 core/tools/unverified-report.py perennialism`) — the Box Guénon
launch date, the Foundation for Traditional Studies' place and second date, the
filming date of the World Wisdom Schuon footage, and the publication date of
the Teitelbaum podcast episode. Refresh these numbers with
`dataset-query.py perennialism stats` rather than trusting this paragraph.

**Visualizations** (rendered by `build.js` from optional top-level keys, so the
page degrades cleanly if a key is absent):

- `lineage` — an intellectual lineage tree in three parts: the Guénonian line,
  the political adaptation as a *separate* line (Evola), and the Huxley name
  collision marked as **not this lineage**. Edges are typed — solid for direct
  discipleship or collaboration, dashed for influence / reception / cross-link,
  which is also how the cross-links to `tariqa` are drawn.
- `branchTimeline` — a works timeline by author, 1921–2024: a "Traditionalist
  corpus" trunk with six branches (Guénon, Coomaraswamy, Evola's political
  line, *Études Traditionnelles*, Schuon, Nasr).
- `disambiguation` — the nine standing confusions, stated as such (school ≠
  order; Guénonian ≠ Huxley; ≠ academic *philosophia perennis*; Evola's line ≠
  Schuon's; Eliade is a debate, not a datum; the royal reception is attributed
  commentary; 'founder' vs 'developer' for Coomaraswamy; a partisan outlet's
  headline ≠ the scholar it interviews; and the French occultist genealogy that
  runs Fabre d'Olivet → Saint-Yves d'Alveydre → Evola is a *different* account
  from the one charted here, with a *third* Rama in it).

**Glossary cross-links.** Prose carries inline `[[term-id]]` markers that render
as links into `cronologia/glossary` (currently `philosophia-perennis` and
`traditionalist-school`). They are validated **offline** against the pinned
`data/glossary-terms.json`, refreshed by `node scripts/sync-glossary-terms.js`,
and covered by `test/glossary-links.test.js`.

**Preservation.** `scripts/archive-refs.js` records Wayback snapshots into
`data/archives.json` (40 of 48 references snapshotted at last run), which
`build.js` renders as an "archived" fallback link beside each source;
`scripts/check-links.js` reports rot without ever editing data. Both run
out-of-band or from `.github/workflows/wayback.yml` and `link-health.yml` — the
build itself is network-free. Current gaps:
`dataset-query.py perennialism refs --unarchived`.

## Known source disagreements

Recorded in the data as attributed, dated prose rather than silently resolved:

- **'L'Homme et son devenir selon le Vêdânta' — 1925, not 1927.** Settled
  against the French bibliographic record and Zoccatelli's CESNUR study; the
  '1927' of a derived source was an error, and the entry says so.
- **Fons Vitae's founding** — the publisher's own account, an infobox giving
  1996 and IRS records giving a 2000 formation year for "Fons Vitae of
  Kentucky" are reported side by side, not averaged.
- **Huxley, US 1945 / UK 1946** — both imprints and dates stated.
- **Schuon's French first editions** (1953 / 1961 / 1970) still await the
  standard bibliography; the official `fschuon.info` bibliography page is
  cited and remains unsnapshotted.
- **The Eliade debate** and **Teitelbaum's stronger claims** are presented as
  live scholarly disputes, with the dissenting reviews cited alongside.

## Searching the sources

The vocabulary problem is severe enough here to have its own file:
[`KEYWORDS.md`](KEYWORDS.md). It carries the generated inventory of names,
organizations, terms of art and places from the dataset, plus a hand-written
`## Search traps` section recording what a naive search gets *wrong* — the
terms that return nothing despite the subject being present, the PT/ES/FR/Latin
naming variants, the auto-caption manglings observed in the vaulted transcripts,
and the false friends (three Ramas, four "perennial philosophies", two
Temenoses, `sufi` vs `suficiente`). It is a finding aid: it makes no claims
about the world, and listing a hostile source's vocabulary is not endorsement.
Read it before searching a corpus or mining a transcript, and add to it when a
search teaches you something.

## Key sources

Sedgwick ('Against the Modern World' 2004, 'Traditionalism' 2023, the
Traditionalists blog) · Hanegraaff ('Esotericism and the Academy' 2012) ·
Wikipedia EN/FR for the corpus scaffolding · Gifford records · the IAPSOP
digitized run of Études Traditionnelles · CESNUR on the Regnabit episode ·
Paull on Northbourne · Marino in *Aries* (Brill) and the Scagno volume on the
Eliade question · labeled perspective sources (Gornahoor as
Evolian-sympathetic; Foreign Affairs' skeptical Teitelbaum review).
