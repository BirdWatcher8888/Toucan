# brimova

Drug-name screening for everyone. A free, open, client-side screener for candidate
generic (INN/USAN) and brand (proprietary) drug names: written naming rules, a
POCA-flavored look-alike/sound-alike similarity screen, and a density metric.
A screen, never a certification.

**Live site:** https://brimova.org

## What's here

| file | purpose |
|---|---|
| `index.html` | The entire site and engine. No build step, no dependencies, no server. |
| `brimova-mark.svg` | The mark, standalone, for reuse (social, print, favicon source). |

## Deploy to GitHub Pages under brimova.org

1. Create a public repo (e.g. `brimova`), push these files to the default branch.
2. Repo **Settings → Pages** → Source: *Deploy from a branch* → select the branch, `/ (root)`.
3. Still in Pages settings, set **Custom domain**: `brimova.org` (GitHub commits a `CNAME` file).
4. At 101domain, in DNS for `brimova.org`:
   - Four **A** records on the bare domain → `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   - **CNAME** on `www` → `<your-username>.github.io`
5. In your GitHub **account** settings → Pages → *Verified domains*: add `brimova.org` and publish the TXT record it gives you (prevents domain takeover).
6. Back in the repo's Pages settings, tick **Enforce HTTPS** once the certificate provisions (minutes to an hour).

## Editing the corpus and rules

Everything lives in one `<script>` block at the bottom of `index.html`:

- `CORPUS` — the demonstration name list (~300 names, data date Aug 2026). One name per whitespace-separated token; duplicates are removed automatically. **This is a demonstration corpus**, a stand-in for Drugs@FDA / WHO INN / EMA / national registries — swapping in fuller public data is the single highest-value contribution.
- `STEMS` / `END_STEMS` — a representative subset (37) of the ~600 USAN/INN pharmacological stems.
- `CLAIMS` — flagged claim/route/anatomy fragments for brand names.
- Thresholds: hard-fail similarity at 0.62 (generic) / 0.60 (brand); density radius 0.45.

If you change coverage, update the **Coverage and limits** plate in the HTML — the
honesty architecture is the product.

## Roadmap

- Fuller public corpora (Drugs@FDA, WHO INN cumulative list, EMA, Health Canada DPD, ARTG)
- Optional semantic layer (multilingual connotation, hidden-anatomy catch) as bring-your-own-key
- Candidate generation biased toward sparse regions of the namespace

## Contact

- Collisions this screen should have caught: **collisions@brimova.org** (the most valuable mail this project can receive)
- Security: **security@brimova.org**
- Everything else: **nomenclator@brimova.org**

## License

MIT for the code. The screening output carries no warranty and is not legal,
regulatory, or trademark advice — see the disclaimer in the site footer.

*Brimova is not a medicine.*

## v2 — the contender generator

The tool now generates as well as screens. The intake mirrors USAN/FDA application
semantics in under ten fields: name tier (generic/brand), pharmacologic class (the
stem selector, all 37 with plain-language labels), route of administration (arms the
route-word ban), indication (its words are extracted and banned from candidates),
paired generic (brand mode, for the shared-letters rule), and a tone dial — executive
& clinical vs. gentle & soothing — implemented as sound-symbolism syllable banks
(plosives/closed syllables vs. liquids/open vowels). The generator composes ~1,400
law-abiding candidates per press, screens every one through the same three passes,
and returns the eight born farthest from every existing name, ranked most-isolated
first. Each result has a "full screen" button that runs the complete verdict.
Everything remains client-side; nothing typed leaves the page.

Inspiration slots: up to three optional words the name may echo. Only the opening
sound (initial consonant cluster plus first vowel) and the word's vowel palette are
borrowed — never the word itself — and for generic names any INN-hostile letters in
the borrowed sounds are transliterated to legal equivalents (k→c, w→v, y→i, j→g).
All screens still apply; rules always beat inspiration.
