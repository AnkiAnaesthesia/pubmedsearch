# PubMed Lab Assistant

A self-contained, no-install browser tool for reproducible literature searches in medical research. Built for clinical research teams who need a fast, structured way to search PubMed, screen papers against defined criteria, and export ranked results.

---

## What it does

**Search** — Query PubMed directly using standard Boolean syntax. Filter by study type, date range, and number of results before you search.

**Document Scanning** — Every retrieved paper is automatically scanned against a set of criteria you define. Each criterion checks the title and abstract (or full text where available) for keywords you specify. Papers are scored 0–100% based on how many criteria they pass.

**Full Text Scanning** — For papers available on PubMed Central (PMC), the tool automatically fetches and scans the complete article text — methods, results, discussion — not just the abstract. Papers are badged as **Full text scanned** or **Abstract only** so you always know the depth of the scan.

**Free PDF Links** — After each search, the tool checks [Unpaywall](https://unpaywall.org) and [Open Access Button](https://openaccessbutton.org) for legally free full text versions of each paper and links to them directly.

**Export** — All results export to CSV in ranked order (best to worst score), with one column per criterion showing PASS/FAIL, plus your notes, tags, and free PDF links.

---

## How to use it

### Option A — Live web app (recommended)
Open directly in your browser — no download needed:

👉 **[https://chucknally2.github.io/pubmedsearch/pubmed-lab-assistant.html](https://chucknally2.github.io/pubmedsearch/pubmed-lab-assistant.html)**

Works on desktop and mobile (Chrome, Firefox, Edge, Safari). Requires an internet connection to reach PubMed.

### Option B — Download and run locally
1. Download `pubmed-lab-assistant.html`
2. Double-click the file — it opens in your browser
3. No installation, no accounts, no software required

---

## Tabs

| Tab | What it does |
|---|---|
| **Search** | Enter your PubMed query and filters. Preview your active scan criteria before searching. |
| **Results** | Browse papers ranked by criteria score. Filter by study type, tag, or score threshold. Expand any paper to read its abstract and full scan breakdown. |
| **Scan Criteria** | Define what each paper must (or must not) contain. Edit keywords, add new criteria, adjust rule types. Also manage your manual workflow tags here. |
| **History** | Every search is logged with its full query string, date, and result count. One-click re-run for reproducibility. |

---

## Scan Criteria

Each criterion has:
- **A label** — what you're checking for (e.g. "Blinded / Masked")
- **Keywords** — comma-separated terms to look for (e.g. `double-blind, blinded, masking, allocation concealment`)
- **Rule type** — either **Must contain** (pass if any keyword found) or **Must NOT contain** (pass if no keywords found — useful for exclusions like animal studies)

### Default criteria (pre-loaded)
| Criterion | Rule |
|---|---|
| Blinded / Masked | Must contain |
| Randomised | Must contain |
| Human subjects | Must contain |
| Exclude animal studies | Must NOT contain |
| Sample size > 50 | Must contain |
| Reports primary outcome | Must contain |
| Exclude case reports | Must NOT contain |
| Registered clinical trial | Must contain |

All criteria are fully editable. Changes take effect on your next search.

---

## Scoring

Each paper receives a **criteria score (%)** calculated as:

```
Score = (number of criteria passed / total criteria) × 100
```

Papers are sorted highest to lowest by default. The score is shown as a colour-coded percentage on each card:
- 🟢 ≥ 80% — strong match
- 🟡 ≥ 50% — partial match
- 🔴 < 50% — weak match

---

## Manual Tags

Separate from scoring, manual tags are for your team's workflow. Default tags:

`Include` · `Exclude` · `Unsure` · `Data Extracted` · `High Priority`

Click any tag on a paper card to toggle it on or off. Tags are included in the CSV export and can be used to filter the results view. Add or remove tags in the **Scan Criteria** tab.

---

## CSV Export

Exports all papers in ranked order (highest score first) with the following columns:

- PMID, Title, Authors, Year, Journal, DOI, Study Type
- Criteria Score %, Criteria Passed, Scan Depth
- One PASS/FAIL column per criterion
- Manual Tags, Notes, Abstract
- PubMed Link, Free Full Text URL, Full Text Source

---

## Search tips

The search query uses standard PubMed syntax:

```
"propofol" AND "postoperative outcomes"
"spinal anaesthesia" AND "caesarean section" AND pain
"airway management" AND ("difficult airway" OR videolaryngoscopy)
```

Adding `[MeSH]` terms improves precision and reproducibility:
```
"Anesthesia, General"[MeSH] AND "Postoperative Complications"[MeSH]
```

Search history is saved for the session — use the **History** tab to re-run any previous search exactly.

---

## Notes

- **Session only** — notes, tags, and scores are not saved when you close the browser. Export to CSV before closing if you want to keep your work.
- **Internet required** — the tool calls PubMed, Unpaywall, and Open Access Button APIs directly from the browser.
- **No data is stored** — nothing is sent to any server other than the public APIs above. All processing happens in your browser.
- **Mobile compatible** — tested on iOS Safari and Android Chrome.

---

## Built with

- [NCBI E-utilities API](https://www.ncbi.nlm.nih.gov/books/NBK25497/) — PubMed search, summary, and full text
- [Unpaywall API](https://unpaywall.org/products/api) — free full text discovery
- [Open Access Button API](https://openaccessbutton.org/api) — open access fallback
- Vanilla HTML, CSS, and JavaScript — no frameworks, no dependencies

---

## Feedback & contributions

This tool was built for clinical research use in anaesthesiology and perioperative medicine. If you find bugs, have suggestions, or want to contribute improvements, open an issue or pull request.
