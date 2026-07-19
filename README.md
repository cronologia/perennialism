# Perennialism / Traditionalist School — Cronologia

A **compiled static website** documenting the chronology of **perennialism as
an intellectual movement** — the Traditionalist School of René Guénon, Ananda
Coomaraswamy and Frithjof Schuon: the books, the journals, the ideas, and
their reception, from the 1920s to the school's unexpected afterlife in
21st-century politics.

Part of the [Cronologia](https://cronologia.github.io) project family; built
from the [`cronologia/core`](https://github.com/cronologia/core) template.

## Boundary with `cronologia/tariqa`

Two sibling projects split one world:

- **[`tariqa`](https://github.com/cronologia/tariqa)** — the **order**: the
  Maryamiyya's initiatic history, branches, and personal-connection threads.
- **`perennialism`** (this repo) — the **school of ideas**: published works,
  journals, doctrines, the Evola political adaptation, academic
  institutionalization, and the movement's reception.

Events belong to exactly one of the two; each site links the other rather
than duplicating it.

## How it works

A single JSON file is the source of truth; a zero-dependency Node script
compiles it into plain HTML served by GitHub Pages.

```bash
node scripts/validate-data.js && node --test && node build.js
python3 -m http.server -d docs 8000
```

### Publish (GitHub Pages)

Settings → Pages → Source: **GitHub Actions**, plus the Actions variable
**`ENABLE_PAGES=true`** (with `main` as default branch). Deploys on push to
`main`; manual dispatch supported.

## Data quality

A work in progress about a contested intellectual lineage whose political
receptions are actively disputed. The family's rules apply (`AGENTS.md`):
every fact cited, uncertain dates flagged, every characterization — above all
about the Evola line, the alt-right reception, and living figures — attributed
to its author, never asserted in the site's own voice.

## License

[MIT](LICENSE)
