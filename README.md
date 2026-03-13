# Evidentum Pro

> A browser-based systematic review and literature screening tool. No installation. No backend. Runs entirely in your browser.

**[→ Launch Evidentum Pro](https://chucknally2.github.io/pubmedsearch/)**

---

## What it does

Evidentum Pro helps clinicians and researchers conduct structured literature searches, screen papers against inclusion criteria, run reporting quality checklists, and flag retractions — all without leaving the browser. It searches seven databases simultaneously, fetches free full text where available, and optionally runs an AI agent to analyse each paper in depth.

---

## Features

### Multi-database search
Searches up to seven sources in parallel with live status indicators and automatic deduplication:

| Database | Notes |
|----------|-------|
| PubMed | Free, no key required |
| Cochrane Library | Via Europe PMC filter |
| Europe PMC | Free, no key required |
| Semantic Scholar | Free, no key required |
| OpenAlex | Free, no key required |
| Scopus | Requires Elsevier API key + institutional token |
| ClinicalTrials.gov | Free, no key required |

### Full-text retrieval
After search results load, the app automatically attempts to retrieve free full text for each paper via PubMed Central (PMC) and open-access PDF links (Unpaywall, CORE, OpenAlex). Papers are re-scanned against inclusion criteria once full text is available.

### Inclusion criteria scanning
Every paper is scanned against a configurable set of inclusion/exclusion criteria. Results are shown as pass/fail chips on each card with a composite score. Criteria are editable in the Scan Criteria tab.

### Quality checklists
Six reporting guidelines are built in, auto-selected by study type, and applied to each paper's full text:

- **CONSORT 2010** — Randomised controlled trials
- **PRISMA 2020** — Systematic reviews and meta-analyses
- **STROBE** — Observational studies
- **RoB2** — Cochrane Risk of Bias (always assessed alongside the matched checklist)
- **CARE** — Case reports
- **AGREE II** — Clinical guidelines

Each checklist item is keyword-matched against the full text as a starting point. All items can be reviewed and adjusted manually within the expanded paper view.

### AI agent analysis
With an OpenAI API key, the agent analyses each paper sequentially using GPT-4o (for PDFs via URL) and GPT-4o-mini (for full text/abstract). It produces:
- Detailed per-item checklist responses with reasoning
- RoB2 domain-level risk judgements (Low / Some concerns / High)
- An overall risk of bias assessment
- Agent-based re-ranking of results by composite quality score

Agent analysis is locked until the initial search and full-text retrieval phase is complete, ensuring the agent has access to all available text before starting.

### RoB2 visualisation
After agent analysis, generates robvis-style Risk of Bias charts:
- **Traffic light plot** — per-domain judgements for each study
- **Summary bar chart** — proportion at each risk level across all studies
- Cochrane and colourblind-friendly palette options
- Downloadable as PNG

### Retraction checking
Checks all papers with DOIs against Retraction Watch via the Crossref API. Retracted papers are flagged in the results with retraction date and reason. No API key required.

### Checklist Uploader
Upload any PDF directly and run a quality checklist against it without running a database search. The PDF is read locally in your browser — nothing is sent to any server.

### Search history and alerts
Searches are saved locally and can be re-run as alerts to track new publications on a topic.

---

## Getting started

**No installation required.** Download or clone the repository and open `index.html` in any modern browser, or use the hosted GitHub Pages version linked above.

```bash
git clone https://github.com/chucknally2/pubmedsearch.git
cd evidentum-pro
open index.html   # or just drag the file into your browser
```

### Optional: OpenAI API key
Required only for AI agent analysis. Enter your key in the Search tab — it is stored in your browser's localStorage and never sent anywhere except directly to the OpenAI API from your browser.

Get a key at [platform.openai.com/api-keys](https://platform.openai.com/api-keys).

### Optional: Scopus API key
Required only to include Scopus in searches. Requires an Elsevier API key and an institutional token. Register at [dev.elsevier.com](https://dev.elsevier.com).

### Optional: LibKey institution ID
Enables one-click library access links on each result via [LibKey](https://libkey.io). Enter your institution's ID in the Search tab settings.

---

## Privacy and data handling

- **All processing is local.** Search queries, paper data, notes, and checklist responses are stored only in your browser's localStorage.
- **No backend server.** There is no server, no database, and no user accounts.
- **API keys stay in your browser.** OpenAI, Scopus, and LibKey credentials are stored in localStorage and sent only to their respective APIs from your browser — never to any intermediary.
- **CORS proxies.** Some database fetches (particularly Cochrane via Europe PMC and certain PDF retrieval attempts) may route through public CORS proxies (allorigins.win, corsproxy.io, codetabs.com) when direct browser requests are blocked by CORS policy. Be aware that your search queries may pass through these third-party services in those cases.
- **AI analysis.** When using the OpenAI agent, paper abstracts and full text are sent to the OpenAI API under your account and subject to [OpenAI's data usage policies](https://openai.com/policies/api-data-usage-policies).
- **PDF extraction.** PDFs are parsed locally using [pdf.js](https://mozilla.github.io/pdf.js/) — the file content is not uploaded anywhere.

---

## Browser compatibility

Tested in Chrome, Firefox, and Safari (desktop and iOS). Requires a modern browser with ES6 and Fetch API support. PDF.js full-text extraction works best in Chromium-based browsers.

---

## Dependencies

All loaded from CDN — no build step or npm required.

| Library | Version | Purpose |
|---------|---------|---------|
| [pdf.js](https://mozilla.github.io/pdf.js/) | 3.11.174 | In-browser PDF text extraction |
| [IBM Plex Mono / Sans](https://fonts.google.com/specimen/IBM+Plex+Mono) | — | UI typography |

---

## Security

A full security audit was conducted prior to the initial GitHub release. Key findings and fixes:

- All external API data rendered via `esc()` HTML entity encoding
- All URLs validated via `safeUrl()` / `safeDoi()` before use in `href` attributes (blocks `javascript:` and `data:` URIs)
- ClinicalTrials.gov IDs validated against `/^NCT\d+$/i` before URL construction
- All `target="_blank"` links patched with `rel="noopener noreferrer"` at runtime

See [`SECURITY.md`](./SECURITY.md) for the full report and disclosure policy.

> **Note:** API keys are stored in browser localStorage. Do not use Evidentum Pro on shared or public computers with sensitive API credentials.

---

## Acknowledgements

- [PubMed / NCBI](https://pubmed.ncbi.nlm.nih.gov/) — NLM Entrez API
- [Europe PMC](https://europepmc.org/) — including Cochrane Database access
- [Semantic Scholar](https://www.semanticscholar.org/) — Allen Institute for AI
- [OpenAlex](https://openalex.org/) — OurResearch
- [ClinicalTrials.gov](https://clinicaltrials.gov/) — U.S. National Library of Medicine
- [Crossref](https://www.crossref.org/) — retraction data via Retraction Watch
- [Unpaywall](https://unpaywall.org/) / [CORE](https://core.ac.uk/) / OpenAlex — open access PDF discovery
- [robvis](https://mcguinlu.github.io/robvis/) — inspiration for RoB2 visualisation style
- [Cochrane RoB2](https://methods.cochrane.org/risk-bias-2) — Risk of Bias assessment framework

---

## Licence

MIT — see [LICENSE](./LICENSE) for details.
