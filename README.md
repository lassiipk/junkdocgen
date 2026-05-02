# [JunkDocGen](https://lassiipk.github.io/junkdocgen)

> Synthetic document generator for academic research and benchmarking.

**Fully stateless. No backend. No storage. No tracking. Deploy in 60 seconds.**

---

## What It Does

JunkDocGen generates realistic-looking fake documents — legal contracts, financial reports, resumes, news articles, and more — entirely in the browser. Every document is algorithmically synthesized. Nothing is sent to a server. Nothing is stored.

Built for researchers, NLP engineers, and dataset curators who need bulk synthetic documents without setting up a pipeline.

---

## Live Demo

**[lassiipk.github.io/junkdocgen](https://lassiipk.github.io/junkdocgen)**

---

## Document Types

| Category | Sub-types |
|---|---|
| Legal | NDA, Employment Contract, Court Filing, Affidavit, Power of Attorney, Lease Agreement, Terms of Service, Privacy Policy |
| Financial | Invoice, Bank Statement, Balance Sheet, Audit Report, Profit & Loss, Tax Return, Purchase Order, Financial Forecast |
| Resume / CV | Chronological, Functional, Academic, Technical, Executive Bio |
| Article / Blog | News Report, Opinion Editorial, How-to Guide, Listicle, Research Abstract, Press Release |
| Lorem Ipsum | Paragraphs, Sentences, Words, Cicero Original |
| Wikipedia-style | General Article, Scientific Topic, Historical Event, Geography, Biography |
| Mixed | Random combination of the above |

---

## Output Formats

`HTML` `TXT` `Markdown` `JSON` `CSV` `PDF`

All generated and downloaded client-side. No server involved.

---

## Parameters

| Parameter | Type | Range / Options | Default |
|---|---|---|---|
| `doc_type` | enum | legal, financial, resume, article, lorem, wikipedia, mixed | mixed |
| `doc_subtype` | string | Varies per type | Random |
| `output_format` | enum | html, txt, md, json, csv, pdf | html |
| `word_count` | integer | 100 – 3000 | 500 |
| `language` | enum | en, fr, de, es, ur, ar | en |
| `realism_level` | enum | low, medium, high | medium |
| `randomness` | float | 0.0 – 1.0 | 0.5 |
| `seed` | integer | Any integer | Auto |

### Seed-Based Reproducibility

Same seed + same parameters = identical document, every time. The seed is displayed in the UI and embedded in downloaded filenames:

```
jdg_nda_seed42.pdf
jdg_invoice_seed1337.json
```

---

## Deploy Your Own

JunkDocGen is a single HTML file. No build step. No dependencies to install.

### GitHub Pages (recommended)

```bash
git clone https://github.com/lassiipk/junkdocgen
cd junkdocgen
```

1. Go to your repo → **Settings → Pages**
2. Source: `main` branch, `/docs` folder
3. Save. Live at `https://lassiipk.github.io/junkdocgen`

### Local

```bash
# Just open the file
open docs/index.html

# Or serve it
npx serve docs/
python3 -m http.server 8080 --directory docs/
```

No installation required. Works offline once loaded.

---

## Repository Structure

```
junkdocgen/
├── docs/
│   └── index.html        # Entire application — single file
├── PRD.md                # Product Requirements Document
├── ATTRIBUTION.md        # Wikipedia CC BY-SA 4.0 policy
├── LICENSE               # MIT
└── README.md
```

---

## Legal & Attribution

### Generated Content

All content produced by JunkDocGen is **entirely synthetic**. It is generated algorithmically using seeded randomness and template logic. No real personal data, no real company data, no real financial figures.

**Generated documents are not legally binding. They do not represent real individuals, companies, or events.**

### Wikipedia-Style Documents

Wikipedia-style output uses the structure and style of encyclopedia articles with fully synthetic body text. When Wikipedia's API is used for topic seeding, generated documents include mandatory attribution:

> *Source: Wikipedia contributors. "[Article Title]." Wikipedia, The Free Encyclopedia. Licensed under CC BY-SA 4.0.*

Derivative works containing Wikipedia content inherit the **CC BY-SA 4.0** license. See [`ATTRIBUTION.md`](./ATTRIBUTION.md) for full policy.

### Tool License

The JunkDocGen tool itself is licensed under **MIT**. See [`LICENSE`](./LICENSE).

```
MIT License — Copyright (c) 2026 lassiipk
```

---

## Intended Use

JunkDocGen is built for:

- Academic research in NLP, document AI, and information retrieval
- Benchmark dataset construction (classification, extraction, parsing)
- QA and stress-testing of document processing pipelines
- Synthetic corpus generation for model training

**It is not intended for** fraud, impersonation, disinformation, or any use that presents synthetic content as real.

---

## Roadmap

- [x] Core generation engine
- [x] 7 document types, 20+ sub-types
- [x] 6 output formats (HTML, TXT, MD, JSON, CSV, PDF)
- [x] Multilingual output (EN, FR, DE, ES, UR, AR) with RTL support
- [x] Seed-based reproducibility
- [x] GitHub Pages deployment
- [ ] REST API (Cloudflare Workers)
- [ ] DOCX output format
- [ ] Bulk generation + ZIP download
- [ ] OpenAPI spec + embedded Swagger UI
- [ ] BEIR / TREC compatible export format

---

## Author

**lassiipk** — [github.com/lassiipk](https://github.com/lassiipk)

---

*JunkDocGen — Synthetic documents for real research.*
