# KEYWORDS — Perennialism — Cronologia

**What this file is.** A finding aid for *searching sources* about this
project's subject — the strings that actually appear in corpora, transcripts,
tickets and this repo's own dataset, plus the ones that look obvious and return
nothing. Part of it is generated from `data/chronology.json` and is regenerable
(`python3 core/tools/build-keywords.py perennialism --out KEYWORDS.md`); the
`## Search traps` section below it is hand-written from measurements and
survives regeneration. **What this file is not:** a dataset, a bibliography, or
a claim about the world. Listing a term asserts nothing — `islamização do
Ocidente` and `proto-fascist` appear here because hostile and critical sources
use those words and their pages have to be *findable*, not because the site
endorses them. Every claim about the subject lives in `data/chronology.json`,
attributed to whoever makes it and cited (`sourcing-rules` #1, #2). Nothing
below was written from memory: every string was read out of this repo's
dataset, out of the vaulted sources in `cronologia/archive`, out of this repo's
tickets, or measured in this session — and each entry says which.

## Search traps

The generator cannot know any of this. Each entry is a **trap** (what a naive
search gets wrong) and the **term that actually works**. Counts are from
measurements recorded here or on the tickets cited; re-measure before trusting
a number that carries a date.

### The corpus is incomplete — a zero can mean "not transcribed"

A zero in the COF corpus is only evidence of absence if that aula was
transcribed in full, and **31 of 585 were not**. Measured 2026-07-26 by comparing
each transcript's word count against the archived audio duration
(`archive/webcaptures/cof-audio-durations.json`, from the archive.org metadata
API):

| estimated coverage | aulas |
|---|---|
| 90–115% — effectively complete | **419** |
| 75–90% | 107 |
| 50–75% | 24 |
| **below 50%** | **7** |

The 31 below 75%, worst first:

`COF513`(18%) `COF534`(34%) `COF154`(36%) `COF514`(38%) `COF157`(46%)
`COF254`(47%) `COF571`(48%) `COF351`(52%) `COF523`(57%) `COF034`(58%)
`COF525`(61%) `COF517`(61%) `COF462`(63%) `COF526`(63%) `COF528`(63%)
`COF542`(63%) `COF453`(64%) `COF529`(65%) `COF389`(65%) `COF498`(65%)
`COF175`(66%) `COF535`(66%) `COF543`(67%) `COF530`(69%) `COF267`(69%)
`COF079`(70%) `COF524`(70%) `COF545`(70%) `COF516`(70%) `COF473`(71%)
`COF531`(72%)

**This qualifies every "measured zero" already published in this family** —
`FSSPX` = 0, `Renovação Carismática` = 0, `Sociedade de São Pio X` = 3 and the
rest. Those counts are over a corpus that is materially incomplete in at least
31 places. Re-state such a result as *"not found in the transcribed portion"*
rather than as absence, and check whether any low-coverage aula is in scope
before concluding a topic is missing.

**The reverse case exists too.** 28 aulas exceed 115%, which is impossible —
there the *audio* is short, not the transcript. `COF229` is the extreme: 13,042
transcript words against 41 minutes of archived audio, while the community index
duration (1:38:35) agrees with the transcript and both archive.org mirrors carry
the same truncated file. A preservation gap, not a transcription gap.

**Method, stated because the numbers invite over-reading.** Coverage is
`words / (audio minutes × 130)`, where 130 wpm is the *observed median* across
585 aulas (stdev 19), not a constant — real rate varies with format (reading
aloud, Q&A, dictation). **Treat 0.75–1.15 as within noise.** Only the extremes
are robust: 18% and 239% cannot be explained by delivery style.

**Directly relevant to this repo:** `COF389`, cited on #32 for the Guénon
sequence behind ticket #5, sits at **65%**. The reading recorded there stands on
what was transcribed; a third of that lecture is not in the corpus.


### The two traps that made the 2026-07 COF mining wave necessary

Both were established on `archive#14` and are restated here with the numbers
re-measured in this repo on 2026-07-26.

- **The entity index is not a mention index — it misses 88% of the corpus.**
  `cof/index.json` ranks each aula's *distinctive* multiword proper nouns, so a
  lecture that discusses someone without dwelling on him never appears.
  `René Guénon` is a distinctive entity in **15** aulas (`cof-xref.py`, alias
  map applied) and is present in the full text of **123** (`Gu[eé]n[oó]n`,
  case-insensitive, whole corpus). `Frithjof Schuon` is distinctive in **1**
  aula (COF094) against 231 occurrences of `Schuon` in `revisadas` alone.
  **Treating `cof-xref.py` output as coverage misses 108 of 123 Guénon aulas.**
  Measured on the 35-aula seam mined for #32 in this session: the entity index
  flags **2** of them (COF005, COF131); the full text carries `Guénon` in **30**
  and `Guénon` or `Schuon` in **35 of 35**. `cof-xref.py` is a lead generator,
  never a coverage measure — grep the full text before concluding absence.
- **The `Olavo de Carvalho` byline trap, and a correction to its size.** A raw
  file count returns **291 of 589**, but the string is mostly the transcription's
  own credit line. `archive#14` measured **218 of 257** `revisadas` carrying a
  standalone byline line of exactly `Olavo de Carvalho`. Re-measured here, the
  byline has **two** forms and they are disjoint: **218** files write the bare
  string on its own line and a further **31** write
  `[Olavo de Carvalho]{.smallcaps}` — **249 of 257**, with only 8 carrying
  neither. In **157** of those the byline is the *only* occurrence of the name
  in the file. Counting only the bare form undercounts the boilerplate by 31
  files. **Strip both byline forms (and the four-line credit blockquote of `>`
  lines that follows the header) before counting anything.** His own speech in
  the transcriptions is separately prefixed `Olavo:`.

### Terms that return nothing

- **`Guenon` (unaccented, capitalized) — ZERO hits in `data/chronology.json`.**
  The prose writes `Guénon` 84 times and never the ASCII form. Case-insensitive
  `guenon` returns 32, but *all* of them are reference ids and URL slugs
  (`wikipedia-guenon`, `cesnur-guenon`, `frwiki-guenon-homme`) — no prose. **Search
  `Guénon` for content, `guenon` for reference ids.** The vaulted transcripts
  invert this: they are auto-captions and mostly write `guenon` unaccented.
- **`Etudes Traditionnelles` (unaccented) — ZERO in the dataset**; `Études
  Traditionnelles` returns 8. Same for the journal name in French sources.
- **`Traditionalist` — ZERO across the whole COF corpus** (589 files,
  7,156,995 words; measured this session). The corpus is Portuguese and writes
  `tradicionalismo` (20 files). Likewise **`Temenos` = 0 files**, **`Northbourne`
  = 0**, **`Steuco` = 0** in COF: those subjects are simply not in that corpus,
  and a zero there is a finding, not a search failure.
- **`Crisis of the Modern World`, `Man and His Becoming`, `Spiritist Error`,
  `Règne de la Quantité`, `Rivolta contro il mondo moderno`, `Unité
  transcendante des religions` — ZERO in the dataset.** See the mixed-language
  title rule below.
- **`zawiya` = 0, `shaykh` = 0, `1991` = 0 in this repo's dataset** (measured
  this session). In `cronologia/tariqa` the same strings return 5, 14 and 8. That
  is the ideas/order boundary working as designed — see *Where else to look*.
  (`bay` is 0 in *both* datasets: the term is glossed in `cronologia/glossary`
  but not yet used in either chronology's prose.)
- **`Guénon` = 0, `Schuon` = 0, `Sufi` = 0, `tradicionalismo` = 0** in
  `archive/transcripts/transcript-64-paz-e-bem-rama-e-os-ocultistas.txt`,
  despite its title promising the Dugin/Olavo/Bannon lineage (measured on
  perennialism#18). It draws a *different* genealogy — Fabre d'Olivet and
  Saint-Yves d'Alveydre → Evola — and the reverse also holds: `Fabre`,
  `Olivet`, `Saint-Yves`, `Alveydre`, `Papus`, `Blavatsky`, `Theosoph*` are all
  ZERO in both scholarly anchors (transcripts 48 and 119, re-measured this
  session). Two non-overlapping accounts; do not merge them.

### The measured negative on the political-reception side

`archive/transcripts/transcript-119-teitelbaum-fascism-war-for-eternity.txt`
(13,190 words) contains **`Schuon` = 0, `Sufi` = 0, `Maryamiyya` = 0**, against
`Bannon` = 18, `Traditionalis*` = 45, `Evola` = 4, `Dugin` = 4, `Guénon` = 1
(counted on perennialism#21, re-counted this session). It is **purely
political-reception material** and cannot source anything on the
religious-initiatic line. This generalizes: **the Evola / Dugin / Bannon search
space and the Schuon / Sufi / Maryamiyya search space barely intersect.** A
searcher in the wrong one finds nothing and wrongly concludes the material does
not exist. In this dataset the split reads `Bannon` 21 / `Evola` 53 / `Dugin` 10
against `Sufi` 2 / `shaykh` 0; in `tariqa` it inverts.

### Naming variants across languages

- **Guénon's works are cited by their FRENCH titles, Schuon's and Evola's by
  their ENGLISH ones — in the same dataset.** `L'Erreur spirite`, `Orient et
  Occident`, `L'Homme et son devenir selon le Vêdânta`, `La Crise du monde
  moderne`, `Autorité spirituelle et pouvoir temporel` are French; but `The
  Reign of Quantity`, `The Transcendent Unity of Religions`, `Revolt Against
  the Modern World` are English. Search **both** forms of any title before
  concluding a work is absent.
- **`perenialismo` (PT, one *n*) vs `perennialismo` (ES, two *n*s).** The
  committed i18n caches render `Perennialism — Cronologia` as `Perenialismo —
  Cronologia` (pt) and `Perennialismo — Cronologia` (es). COF, a Portuguese
  corpus, has `perenialismo` in 2 files and the English `perennialism` in 2
  others — both single-digit. `filosofia perene` appears in 3 files,
  `philosophia perennis` in exactly 1.
- **`Évola` (accented) outnumbers `Evola` in Portuguese sources.** In COF:
  `Évola` 39 occurrences vs `Evola` 34, across 29 files combined. COF also
  writes the French first name — `Jules Évola` — once. Search the accented form.
- **`Tradicionalismo guenoniano`** is the PT/ES rendering of *Guénonian
  Traditionalism* used in `data/i18n/{pt,es}.json`.
- **Schuon's German first book is `Leitgedanken zur Urbesinnung` (1935)** — the
  only German title in the dataset, and it will not be found by any English or
  French search.
- **Figures with a second, Muslim name.** Martin Lings appears in the vaulted
  sources under his too — COF renders it `Sid Abubak Sirajedin` (auto-caption for
  *Sidi Abu Bakr Siraj ad-Din*), in a passage that never says "Lings" nearby.
  Searching only the English surname misses those passages.

### ASR manglings observed in the vaulted transcripts

These were **observed**, not guessed — each is quoted from a file in
`cronologia/archive/transcripts/` or `cronologia/archive/cof/`, and the ones
attributed to a ticket were recorded in that ticket's mining report. Auto-caption
text is never quotable without verification against audio (`sourcing-rules` #5);
these forms exist so the passages can be **located**.

- **Guénon** → `genon`, `rune genon`, `gaynor`, `Enon`, `forgano` (transcript-48,
  perennialism#10) · `renon`, `grenon`, `Renê guenon`, `Renê guon` (transcript-43)
  · `Renegenon`, `Negenon`, `Guinon`, `Guenom`, `Guen`, `Chuanon` (transcript-121)
  · `Rene ganon`, `Rene Ganon` (transcript-56) · `Renê ganon` (transcript-51) ·
  in COF: `Genon` (17), `Ganon` (4), `Genoid` (2), `Reneginon`.
- **Schuon** → `Shuan` (32×, transcripts 56 and 57) · `Chuon`, `chuon`, `Chuão`,
  `Schuom`, `Shum` (transcript-121) · `frischoff schwarn`, `sure` (transcript-48)
  · `frithjofshuan`, `frithyashuan`, `fritov`, `frithav`, `frithyaf`, `fridge`
  (transcript-57) · in COF: `Chuon` (19), `Chuom`, `Chouhon`.
- **Coomaraswamy** → `Comar`, `Comarom`, `Comaram`, `Comaruami`, `Comaruame`,
  `Comaroma`, `Comarasami`, `Comarasvam`, `Comaraça`, `Comarov`, `comaraçon`,
  `comasama` (transcript-121) · `comaruam` (transcript-7) · in COF:
  `Comarassoume`, `Comarassame`, `Comarassome`, `Comarassuame`, `Comarassão`.
- **Burckhardt** → `Burkart`, `Burkhart`, `Burkhan`, `Burkard`, `Burkle`,
  `Burkher`, `Tito Zumbur`, `Tito Bucke` (COF; also `Burkhart` in transcripts
  56/57). **`Burke` (7×, transcript-4) is NOT Burckhardt** — it is Cardinal
  Raymond Burke, in an SSPX discussion. See also the **Jacob-vs-Titus** split
  under *Ambiguous referents* below: the manglings attach to both men, so the
  variant set widens the search and does not disambiguate it.
- **Lings** → `Martin Ling` (singular), `Martin Lynch` (COF). **`Martin Luther
  King` is a false hit on any `Martin L` pattern** (COF).
- **Teitelbaum** → `Titlebaum`; **Bannon** → `Band-Aid`, `Band-Aidism`
  (transcript-119, perennialism#21); **Jean Wyllys** → `Jean Willis`.
- **Evola** → `Avila`, `Evelyn`, `avalor` (transcript-48) · `Júlio Évora`
  (transcript-64). **Dugin** → `Dugan` (9×), `Duggan` (transcript-48) ·
  `Dublin` (transcript-64) · `duguin`, `duguinismo` (transcripts 66, 64).
  **Bannon** in Portuguese ASR → `bebê não`, `estive bem não` (transcript-64).
- **Seyyed Hossein Nasr** → transcript-66 renders him `seed jossei Nasser` and
  `seed hosi Nasser` in consecutive lines: every element of the name is mangled
  and the surname collides with **Gamal Abdel Nasser**. `nasser` also appears in
  transcript-57 and `NASA` there is a further false hit. Locate him by the
  first name (`jossei`, `hosi`, `Hossein`) plus context, never by `Nasr` alone.
- **Where the manglings live.** In COF the mangled forms are confined to the
  **unreviewed half**: `Genon`/`Ganon`/`Chuon` occur 0 times in `cof/revisadas/`
  and 40 times in `cof/revisao_pendente/`. Conversely `Schuon` is 231 in
  `revisadas` but only 9 in `revisao_pendente`. **Searching the clean spelling
  alone silently drops the 332 unreviewed files.**

### Ambiguous referents and false friends

- **Ananda K. Coomaraswamy (1877–1947), the art historian, is THIS repo's
  figure. Rama P. Coomaraswamy (his son) belongs to `tariqa` and `fsspx`.** A
  bare surname search across the family returns mostly the son: this dataset has
  `Ananda` 11 / `Rama` 1; `tariqa` has `Rama` 15 / `Ananda` 3; `fsspx` has
  `Rama` 10 / `Ananda` 1. COF distinguishes them explicitly ("os dois
  Comarassame"). perennialism#28 states the rule for this repo.
- **A THIRD Rama.** In `transcript-64` (Paz e Bem #1294) "Rama" is the
  **mythical Vedic figure** of French occultist historiography (Saint-Yves
  d'Alveydre, Schuré) — neither Coomaraswamy. Now carried in the dataset as
  `disambiguation.items[8]`, so this is no longer a gap.
  **Correction, measured this session:** the earlier claim here that "Rama"
  occurs **27 times** in that file was a *substring* count and is wrong. As a
  whole word it occurs **4 times** (3 lowercase, 1 capitalised), plus
  `Ramayana` once. The other 23 were `programa` (13), `grama` (6), `drama` (2),
  `derrama`/`derramando`, `programaç…` and one ASR blob `etirama`. **`rama` is
  itself a Portuguese word ("branch") and a substring of the commonest word in
  a broadcast transcript.** Search `\brama\b`, never bare `rama` — the same
  failure mode as `sufi`/`suficiente`, in the very file that documents it.
- **"Perennial philosophy" is a name collision with four careers** —
  Steuco (1540), Leibniz, Huxley (1945), and the Guénonian school — kept apart
  in `[[philosophia-perennis]]` and in `disambiguation.items[0]`/`[1]`. A hit on
  the phrase tells you nothing until you know which. Huxley's is the one most
  search engines surface.
- **"Traditionalist School" (capitalized) ≠ "traditionalism" (lower-case).**
  Three different things share the word: Catholic traditionalism (the `fsspx`
  project), the Guénon–Schuon Traditionalist School (here), and Evola's
  politicized "Traditionalism". `[[traditionalist-school]]` is the glossary
  anchor. In COF, `tradicionalismo` (20 files) mixes all three and must be read
  in context.
- **`sufi` as a substring is worthless in a Portuguese corpus.** In COF it
  matches 475 files, of which ~96% are `suficiente` (872), `suficientemente`
  (194), `suficientes` (109), `insuficiente` (38). The real signal is
  `sufismo` — 39 occurrences in 21 files. Recorded on perennialism#32 and
  `archive#13` §2; re-measured this session. **Search `sufismo`, `sufis\b`, or
  `sufi\b` — never bare `sufi`.**
- **`Burckhardt` in COF is two different men, and the Traditionalist is the
  minority of neither.** Measured across the corpus 2026-07-26 with the variant
  set `Bur[ck]{1,2}h?ar[dt]t?`: **30 files**, of which **19** name *Titus*
  Burckhardt (also written `Tito`) and **12** name *Jacob* Burckhardt, the Swiss
  historian of the Renaissance — a standing reference of Olavo's political
  lectures, cited for *Reflexões sobre a História Universal*. One file (COF250)
  has both, and no file leaves the surname unqualified. **A bare surname search
  is ~40% wrong here**, and the ASR manglings do not help: `Burkart`/`Burkard`/
  `Burkhart` occur for Jacob (COF320, COF510) as well as for Titus. Search the
  first name — `Titus`/`Tito` for this repo's figure, `Jacob` for the historian.
- **`Agartha` cannot be searched as a substring in Portuguese: `agarta` is
  inside `lagarta` ("caterpillar").** Measured: `agarta` as a substring matches
  4 COF files; as a whole word (`\bagarth?a\b`) it matches exactly **2**
  (COF041, COF414). COF049 appears to be a 12-hit Agartha file and is in fact a
  lecture about caterpillars becoming butterflies. Same failure mode as
  `sufi`/`suficiente` and `rama`/`programa`; use word boundaries.
- **`Sophia` has three referents inside this dataset**: *Sophia Perennis* the
  publisher (1×), the *journal Sophia* of the Foundation for Traditional Studies
  (4×), and the Latin `sophia perennis` (3×). Disambiguate before citing.
- **`Temenos` has two**: the *Temenos* review launched 1980 (Kathleen Raine and
  colleagues) and the **Temenos Academy** founded 1990. Ten years apart, same
  word, both in this repo.
- **`Éditions Traditionnelles` (the Paris publisher) ≠ `Études
  Traditionnelles` (the journal).** Both appear in this dataset — the publisher
  once, on Schuon's *Logique et transcendance* (1970). The single
  `Traditionnelles` hit in the whole COF corpus is the **publisher**, in a
  bibliography of an astrology book — not the journal.
- **`Bloomington` in this repo is World Wisdom's business address**, one
  occurrence, in the publisher's `organizations[]` record. In `tariqa` the same
  string returns 30 and means the **1991 affair**. Searching `Bloomington` here
  to find that story fails by design.
- **`Guinô` in `transcript-92` is São Luís *Grignion* de Montfort**, not
  Guénon — an ASR near-collision across two entirely different subjects.
- **`Lefebvre`** conflates Marcel (SSPX) with Henri (the Marxist philosopher) in
  COF; recorded on `archive#13` §2. Relevant here because COF is a shared corpus.

### Terms of art that must not be translated when searching

Search these in the source language; the English gloss will not appear in the
sources. Glossary ids link to `cronologia/glossary`.

| Term | Glossary id | Note |
|---|---|---|
| `philosophia perennis` | `[[philosophia-perennis]]` | Latin. Four distinct lineages under one phrase; do **not** normalize to "perennial philosophy" when searching, and do not translate it into PT/ES. |
| `Traditionalist School` | `[[traditionalist-school]]` | Capitalization is load-bearing. PT/ES sources write `tradicionalismo guenoniano`. |
| `tariqa` | `[[tariqa]]` | Arabic; PT/ES sources write `tarica`/`tarika` (COF: `tarica` 268, `tariqa` 111). Owned by `cronologia/tariqa`. |
| `silsila` | `[[silsila]]` | Arabic. Never "lineage" — the English word will not find it. |
| `muqaddam` | `[[muqaddam]]` | Arabic. Also a civil/military rank — check context. |
| `khalifa` | `[[khalifa]]` | Arabic; `khilafat` for the office. |
| `zawiya` | `[[zawiya]]` | Arabic; also `zaouia`, `zāwiya`; the Persianate `khanqah` and Turkish `tekke` name the same institution. |
| `dhikr` | `[[dhikr]]` | Arabic; also `zikr`. |
| `bay'ah` | `[[bayah]]` | Arabic; also `baya`, `bay'at`. In this family, "took bay'ah" is a **membership** claim. |

Only `philosophia-perennis` and `traditionalist-school` are used as
`[[term-id]]` markers in *this* dataset (validated offline against
`data/glossary-terms.json`); the Sufi terms are listed because sources about
this subject use them and a searcher here needs to recognize them.

### Unexpected vocabulary that IS in this repo

- **`organic farming`** (5 hits) — Lord Northbourne coined the phrase in *Look
  to the Land* (1940). Nobody searching "perennialism" would try it.
- **`Gifford Lectures`** — Nasr, 1981.
- **`Regnabit`**, **`Charbonneau-Lassay`**, **`Hiéron du Val d'Or`** — Guénon's
  Catholic decade (perennialism#2).
- **`As garras da Esfinge`** — the Brazilian "islamization" reading, 2016.
- **`Box Essencial Guénon`** — the 2022 Brazilian publishing event.
- **`Lord Northbourne` and `Walter James` are the same man**; both forms are in
  the dataset.

### Where else to look

- **`cronologia/tariqa` owns the ORDER.** Initiations, the silsila, zawiyas,
  branch politics, the 1991 Bloomington affair, Schuon's biography as shaykh,
  Rama P. Coomaraswamy's membership, the Brazilian tariqa scene and Olavo de
  Carvalho's years in it. If your query contains `zawiya`, `bay'ah`,
  `muqaddam`, `Maryamiyya`, `1991`, or `Bloomington`, you are in the wrong repo.
- **`cronologia/fsspx` owns Catholic traditionalism** — Lefebvre, the SSPX,
  sedevacantism, and Rama P. Coomaraswamy's SSPX seminary years. Its
  "traditionalism" is not this one.
- **`cronologia/glossary` owns the term definitions** and is where a
  `[[term-id]]` resolves.
- **`cronologia/archive`** holds the vaulted sources: `transcripts/` (121 docs,
  manifest in `transcripts/index.json` — filter on `projects` containing
  `perennialism`), `webcaptures/`, and the **COF corpus** (589 files,
  7,156,995 words, `cof/index.json`; admitted as a corpus under `archive`
  ADR-0005 — a search base, not a citable bulk source).
- **Which COF files are densest** on this subject, by `Guénon`+`Schuon`
  occurrences per 10k words (measured this session, files ≥3,000 words):
  `COF209` (106.7 per 10k; 79 Guénon + 35 Schuon in 10,684 words), `COF223`
  (49.6), `COF344` (43.1 — but Schuon = 1, so it is a Guénon-only file),
  `COF094` (30.6), `COF041` (28.8), `COF218` (26.4). Ranking by **raw count**
  instead promotes `COF041` (22,564 words) over `COF223`; rank by density.
  Per perennialism#32, `COF138`, `COF223` and `COF241` are shared with
  `tariqa#30` and `fsspx#49` — mine once.
- **A COF header date is not a verified date.** `cof/index.json` marks all 257
  `revisadas` `dateVerified: true` because a header said so. `archive#20`
  measured **14 headers that contradict both their dated neighbours**, five of
  them probable whole-year typos (COF027, COF079, COF089, COF143, COF149) and
  the rest ordering anomalies (COF018/019, COF047, COF076, COF105/106, COF115,
  COF176, COF222). **Check that list before citing an aula**, say so in the
  reference when the aula is on it, and never "correct" a header — the header is
  the evidence, an inference is not. This dataset cites COF105 and COF106 with
  that caveat attached.
- **Guénon's works are cited in COF by their PORTUGUESE titles**, which none of
  the French or English forms will find: `O Reino das Quantidades e os Sinais
  dos Tempos`, `O Homem e seu devir segundo o Vedanta`, `Os Estados Múltiplos do
  Ser`, `Os Princípios do Cálculo Infinitesimal`, `Metafísica Oriental`,
  `Teosofismo` / `Le Théosophisme, a História de uma Pseudoreligião`, and the
  symbolism collection referred to as *os símbolos da ciência sagrada*. Titus
  Burckhardt appears as `Ciência Moderna e Sabedoria Tradicional` and
  `A Civilização Hispano-Árabe`. Two secondary works appear only in Portuguese
  form too: Gaston Georgel's `Les quatre âges de l'humanité` (glossed *As quatro
  eras da humanidade*) and Jean-Marc Allemand's `René Guénon e as Sete Torres do
  Diabo`.
- **`Michel Veber` and `Michel Weber` are the same person in COF** — Olavo's
  Chinese-martial-arts teacher and the author of commentaries on Guénon's
  *Metafísica Oriental*. Aula 105 writes `Veber`, aula 141 writes `Weber`.
  Search both; neither is Michel Weber the process-philosophy scholar.
- **Aula number is not a date, and the two do not track each other.** Measured
  on `cof/index.json` this session: dated aulas run **COF001 (2009-03-14) →
  COF293 (2015-05-23)**, and the *highest aula number carrying any date is
  293*. Everything above it — including `COF344`, `COF389`, `COF414`, `COF421`
  identified on perennialism#32 — is `revisao_pendente` with **no date at all**.
  Numbering is also not strictly chronological (aula 89 carries 2011-11-22,
  between aulas 131 and 132). **Never date an aula from its neighbours.**
- **Adding the ASR manglings changes the COF file counts.** perennialism#32
  measured `Guénon`/`Guenon` in **121 files** and `Schuon` in **47**. Searching
  the mangled forms too (`genon|ganon|guenom|guinon|reneguenon|renegenon|
  negenon|genoid|reneginon|chuanon` and `chuon|chuom|chouhon|shuan|schuom`)
  gives **129** and **59** files respectively, **145** with either — measured
  this session. The extra files are almost entirely `revisao_pendente`.
- **Do NOT put `chão` in a Schuon pattern.** Accent-stripped, the Portuguese
  word for "floor/ground" becomes `chao`, and `chon`/`chao` in a Schuon regex
  silently inflates every count in a Portuguese corpus. Measured: including
  them added 12 spurious "Schuon" files. The safe variant set is the one above.
- **Fingerprinting a clip against COF works, and it works on rare content
  words, not on names.** Method used on perennialism#32: normalise (lowercase,
  strip accents, tokens ≥3 chars), take the clip's tokens whose corpus document
  frequency is ≤40, score every 500-token window of every aula by summed idf.
  A true match separates cleanly — `transcript-44` → `COF015` scored **109.6**
  against **21.8** for the runner-up. A score in the **8–30** band means shared
  *topic*, not shared *passage*; do not read it as an identification.

<!-- BEGIN GENERATED build-keywords.py -->
<!-- Generated by core/tools/build-keywords.py from perennialism/data/chronology.json (meta.lastUpdated 2026-07-30).
     Regenerate: python3 core/tools/build-keywords.py perennialism --out KEYWORDS.md
     Edits INSIDE this block are lost on regeneration; everything outside it is kept. -->

## How to use this list

**This block is a finding aid, not a dataset.** It lists strings worth
searching for — names, aliases, acronyms, spellings, and the vocabulary of
sources across the spectrum, hostile ones included. Listing a term asserts
nothing about the world: `schism` appearing in a search list does not claim
anyone is schismatic, and a critical source's word for something is listed so
its pages can be *found*, not endorsed. Every claim about the subject lives in
`data/`, attributed to whoever makes it and cited (`sourcing-rules` #1, #2).

Every string below was read out of this repo's dataset by the generator.
Nothing here is inferred or remembered. Variants seen elsewhere — in a corpus,
a transcript, an auto-caption — and terms that return **zero** hits belong in
the hand-written section outside this block, with a note on where they were
seen or searched.

## Subject names (2)

What the subject is called, from `meta` — plus every name the description puts in parentheses (acronyms, native-language forms, and whatever else it names in passing; the source field is on each line). A corpus may use exactly one of these and none of the others.

- `Perennialism — Cronologia` — meta.title
- `Perennialism` — meta.title

## People (14)

Every `figures[]` name, with the aliases and both sides of an `A — B` name. An `id` is that figure's own page — a permanent URL and a searchable handle.

- `René Guénon` · figures[0]
- `Ananda K. Coomaraswamy` · figures[1]
- `Frithjof Schuon (as author)` · figures[2] · also: `Frithjof Schuon`
- `Julius Evola` · figures[3]
- `Seyyed Hossein Nasr` · figures[4]
- `Martin Lings (as author)` · figures[5] · also: `Martin Lings`
- `Titus Burckhardt` · figures[6]
- `Huston Smith` · figures[7]
- `Marco Pallis` · figures[8]
- `Lord Northbourne (Walter James)` · figures[9] · also: `Lord Northbourne`, `Walter James`
- `Jean-Louis Michon` · figures[10]
- `Aldous Huxley` · figures[11]
- `Mircea Eliade` · figures[12]
- `Mark Sedgwick & Wouter Hanegraaff` · figures[13]

## Organizations (6)

Every `organizations[]` name and alias. Acronym and full name are listed separately: sources use one or the other, rarely both.

- `Études Traditionnelles (journal)` · organizations[0] · also: `Études Traditionnelles`
- `Sophia Perennis (publisher)` · organizations[1] · also: `Sophia Perennis`
- `World Wisdom (publisher)` · organizations[2] · also: `World Wisdom`
- `Fons Vitae (publisher)` · organizations[3] · also: `Fons Vitae`
- `Temenos Academy` · organizations[4]
- `Foundation for Traditional Studies / journal Sophia` · organizations[5]

## Terms of art (2)

Glossary ids used in this dataset (`[[term-id]]` markers), with the visible text authors actually typed. These are *vocabulary*, including contested vocabulary — see the note at the top.

- `[[philosophia-perennis]]` · **Philosophia perennis** · used 1× · variants: perennial philosophy; perennialism · as written: `Perennial philosophy` · https://cronologia.github.io/glossary/philosophia-perennis/
- `[[traditionalist-school]]` · **Traditionalist School** · used 1× · variants: Guénonian Traditionalism; cf. Catholic traditionalism, Evolian 'Traditionalism' · as written: `Traditionalist School` · https://cronologia.github.io/glossary/traditionalist-school/

## Places (35)

Place strings exactly as the dataset writes them, most-used first. Search a component (`Écône`) as well as the full string.

- `Paris, France` — 13× (events.place,organizations.place)
- `Brazil` — 12× (events.place)
- `London, England` — 5× (events.place,organizations.place)
- `England` — 4× (events.place,figures.country)
- `Italy` — 3× (events.place,figures.country)
- `Cairo, Egypt` — 2× (events.place)
- `New York, USA` — 2× (events.place)
- `Oxford, England` — 2× (events.place)
- `Rome, Italy` — 2× (events.place)
- `United States` — 2× (events.place)
- `Amsterdam / Cambridge` — 1× (events.place)
- `Betteshanger, Kent, England` — 1× (events.place)
- `Bloomington, Indiana, USA` — 1× (organizations.place)
- `Boston, USA` — 1× (events.place)
- `Ceylon / UK / USA` — 1× (figures.country)
- `Edinburgh, Scotland` — 1× (events.place)
- `Edmonton, Alberta, Canada` — 1× (events.place)
- `England / USA` — 1× (figures.country)
- `France / Egypt` — 1× (figures.country)
- `France / Switzerland` — 1× (figures.country)
- `Iran / USA` — 1× (figures.country)
- `London / Paris` — 1× (events.place)
- `Louisville, Kentucky, USA` — 1× (organizations.place)
- `Milan, Italy` — 1× (events.place)
- `Moscow, Russia` — 1× (events.place)
- `Needham, Massachusetts, USA` — 1× (events.place)
- `New York State, USA` — 1× (organizations.place)
- `Paris / Lausanne` — 1× (events.place)
- `Romania / USA` — 1× (figures.country)
- `Switzerland` — 1× (figures.country)
- `Switzerland / USA` — 1× (figures.country)
- `UK-Denmark / Netherlands` — 1× (figures.country)
- `USA` — 1× (figures.country)
- `Washington / New York, USA` — 1× (events.place)
- `Washington, DC, USA (to verify)` — 1× (organizations.place)

## Dates coverage

The window this dataset spans. A source outside it is not necessarily irrelevant — it is not yet covered here.

| scope | records | years | note |
|---|---|---|---|
| events | 53 | 1540–2023 | 0 with dateVerified:false |
| figures.dates | 14 | 1877–2016 | years parsed from the field text |
| organizations.founded | 6 | 1936–1990 | years parsed from the field text |
| dataset (all of the above) | - | 1540–2023 | meta.lastUpdated 2026-07-30 |

<!-- END GENERATED build-keywords.py -->
