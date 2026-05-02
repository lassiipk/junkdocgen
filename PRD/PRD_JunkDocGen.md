# Product Requirements Document (PRD)
## Junk Document Generator (JDG)

| Field | Detail |
|---|---|
| **Author** | lassiipk |
| **Version** | 1.0.0 |
| **Status** | Draft |
| **Created** | 2026-05-02 |
| **Deployment Target** | GitHub Pages (Static Frontend) + Public REST API |

---

## 1. Overview

### 1.1 Product Summary

The **Junk Document Generator (JDG)** is a fully stateless, open-source web tool and API that generates synthetic, fake, and randomized documents for academic research and benchmarking purposes. Documents are generated on-demand, downloaded directly to the user's browser, and never stored server-side.

### 1.2 Problem Statement

Academic researchers, NLP practitioners, and benchmark designers frequently require large volumes of realistic-looking fake documents — legal filings, financial reports, resumes, articles — to test classifiers, stress-test parsers, evaluate document understanding models, or establish baselines. Existing tools are either too simplistic (Lorem Ipsum generators), locked behind paywalls, or require heavy local setup. JDG fills this gap with a zero-dependency, browser-accessible tool that is reproducible, configurable, and multi-format.

### 1.3 Target Users

- Academic researchers in NLP, IR, and document AI
- ML engineers building document classification/extraction benchmarks
- QA engineers needing bulk synthetic test documents
- Dataset curators building synthetic corpora

---

## 2. Goals & Non-Goals

### 2.1 Goals

- Generate syntactically realistic but semantically fake documents across multiple categories
- Support 6 output formats: PDF, DOCX, TXT, JSON/CSV, HTML, Markdown
- Provide both a UI (browser-based) and a REST API
- Enable seed-based reproducible generation (same seed → same document)
- Support multilingual output
- Allow fine-grained control over document length, realism level, and randomness
- Be fully stateless — no server-side storage of any generated content or user data
- Use only freely licensed or algorithmically generated content (no unlicensed scraping)

### 2.2 Non-Goals

- No user authentication or accounts
- No server-side document storage or history
- No real personal data generation (no actual PII)
- No content intended for fraud, impersonation, or deception
- No reproduction of copyrighted full texts (Wikipedia content used under CC BY-SA 4.0 with attribution)

---

## 3. Legal & Licensing Constraints

This is non-negotiable and must be baked into the architecture, not bolted on.

| Content Source | License | Usage Rule |
|---|---|---|
| Algorithmically generated text | None (original) | Freely usable |
| Wikipedia excerpts | CC BY-SA 4.0 | Must attribute source URL; derivative outputs inherit CC BY-SA |
| Faker/synthetic libraries | MIT / Apache 2.0 | Check per-library |
| User-supplied templates | User's own | User's responsibility |

**Implementation requirement:** Every document generated from Wikipedia data must embed a footer or metadata field stating:
> *"Contains content from Wikipedia, licensed under CC BY-SA 4.0. Source: [URL]"*

All other content must be fully synthetic — generated algorithmically, not scraped from unlicensed sources.

---

## 4. Document Types

### 4.1 Supported Categories

| Category | Description | Example Fields |
|---|---|---|
| **Fake Legal Document** | Contracts, NDAs, court filings, affidavits | Parties, clauses, jurisdiction, date, case number |
| **Fake Financial Document** | Balance sheets, invoices, bank statements, audit reports | Amounts, accounts, fiscal period, entity names |
| **Fake Resume / Profile** | CV, LinkedIn-style profiles | Name, skills, employment history, education |
| **Fake Article / Blog Post** | News articles, opinion pieces, listicles | Headline, byline, body paragraphs, publication date |
| **Random / Lorem Ipsum** | Pure filler text, unstructured | Paragraphs, sentences, words |
| **Wikipedia-style Summary** | Encyclopedia-style entry on a fake or real topic | Title, intro, sections, references (attributed) |
| **Mixed / Random** | Randomly combines the above categories | Varies |

### 4.2 Document Sub-types (Per Category Examples)

- Legal: NDA, Employment Contract, Court Summons, Affidavit, Power of Attorney
- Financial: Invoice, Bank Statement, Balance Sheet, Profit & Loss, Audit Report
- Resume: Chronological CV, Functional CV, Academic CV
- Article: News report, Opinion editorial, How-to guide, Listicle

---

## 5. Output Formats

| Format | MIME Type | Notes |
|---|---|---|
| PDF | `application/pdf` | Client-side generation via `jsPDF` or `pdf-lib` |
| DOCX | `application/vnd.openxmlformats-officedocument.wordprocessingml.document` | Via `docx.js` |
| TXT | `text/plain` | Plain UTF-8 text |
| JSON | `application/json` | Structured fields (key-value) |
| CSV | `text/csv` | Tabular form of document fields |
| HTML | `text/html` | Styled HTML document |
| Markdown | `text/markdown` | GitHub-flavored Markdown |

All formats generated client-side (in-browser) or at the API layer. No server storage.

---

## 6. User Customization Parameters

Every document generation request (UI or API) accepts the following parameters:

| Parameter | Type | Description | Default |
|---|---|---|---|
| `doc_type` | `enum` | Document category (legal, financial, resume, article, lorem, wikipedia, mixed) | `mixed` |
| `doc_subtype` | `string` | Sub-type within the category (e.g. `nda`, `invoice`) | Random |
| `output_format` | `enum` | pdf, docx, txt, json, csv, html, md | `pdf` |
| `word_count` | `integer` | Target word count (100–10,000) | `500` |
| `language` | `enum` | Output language (en, fr, de, es, ar, zh, ur, hi, etc.) | `en` |
| `realism_level` | `enum` | `low` (obvious fake), `medium` (plausible), `high` (realistic-looking) | `medium` |
| `randomness` | `float` | 0.0 (fully template-driven) to 1.0 (fully random) | `0.5` |
| `seed` | `integer` | RNG seed for reproducibility. Same seed = same document | Random |
| `quantity` | `integer` | Number of documents to generate in one request (1–100) | `1` |

---

## 7. UI Requirements

### 7.1 Technology Stack

- **Frontend:** Vanilla HTML/CSS/JS or React (single-page, static)
- **Deployment:** GitHub Pages (no server-side runtime)
- **Document Generation:** Client-side JavaScript libraries only

### 7.2 UI Pages / Views

#### 7.2.1 Home / Generator Page

- Form with controls for all 9 parameters in Section 6
- "Generate" button that triggers client-side document creation
- Live preview panel (HTML/Markdown rendered inline; PDF/DOCX shown as download)
- Download button per format
- Seed display (auto-generated or user-specified) with copy button
- "Bulk Generate" option for `quantity > 1` with ZIP download

#### 7.2.2 API Docs Page

- Interactive API reference (OpenAPI / Swagger UI embedded)
- Copy-ready curl/fetch examples per endpoint
- Parameter table with types, defaults, and constraints

#### 7.2.3 About / Legal Page

- Purpose statement (academic/research use only)
- License information for generated content
- Wikipedia attribution policy
- GitHub link and contribution guide

### 7.3 UI/UX Constraints

- Must be fully functional with JavaScript enabled (no server dependency)
- Mobile-responsive layout
- Dark mode support
- No cookies, no analytics, no tracking of any kind

---

## 8. API Requirements

### 8.1 API Architecture

Since GitHub Pages is static, the REST API must be hosted separately. Options:

| Option | Notes |
|---|---|
| **Cloudflare Workers** | Free tier, zero cold start, globally distributed — recommended |
| **Vercel Serverless Functions** | Free tier, good DX |
| **AWS Lambda + API Gateway** | More complex, more control |

The UI GitHub Pages frontend calls this API. The API itself is also open-source (same repo, deployed separately).

### 8.2 Endpoints

#### `POST /generate`

Generate one or more documents.

**Request Body (JSON):**
```json
{
  "doc_type": "legal",
  "doc_subtype": "nda",
  "output_format": "pdf",
  "word_count": 800,
  "language": "en",
  "realism_level": "high",
  "randomness": 0.4,
  "seed": 42,
  "quantity": 1
}
```

**Response:**
- Single doc: Binary file with appropriate `Content-Type` and `Content-Disposition: attachment`
- Multiple docs: `application/zip`

#### `GET /generate`

Same as POST but parameters passed as query string. For quick testing and curl use.

#### `GET /schema`

Returns JSON schema of all supported `doc_type`, `doc_subtype`, `language`, and format values.

#### `GET /health`

Returns `{ "status": "ok", "version": "1.0.0" }` for uptime monitoring.

### 8.3 Rate Limiting

- 60 requests/minute per IP (unauthenticated)
- Bulk `quantity` capped at 100 documents per request
- Max `word_count` per document: 10,000 words

### 8.4 API Response Headers

All responses must include:
```
X-JDG-Seed: <seed_used>
X-JDG-Version: 1.0.0
X-JDG-License: CC-BY-SA-4.0 (if Wikipedia content used) or Generated
```

---

## 9. Content Generation Architecture

### 9.1 Generation Strategy

| Realism Level | Strategy |
|---|---|
| `low` | Random word sequences from dictionary; obvious placeholders (`PARTY_A`, `AMOUNT_X`) |
| `medium` | Template-based with Faker-generated values; grammatically correct but semantically hollow |
| `high` | Template-based with contextually coherent Faker values + optional Wikipedia section injection |

### 9.2 Seed-Based Reproducibility

- All randomness (Faker, template selection, word choice, layout) must be seeded via a single integer seed
- Use a seedable PRNG (e.g., `seedrandom` in JS) injected at generation start
- Seed is logged in output metadata and response headers

### 9.3 Wikipedia Integration

- Use the **Wikipedia REST API** (`https://en.wikipedia.org/api/rest_v1/`) — free, no key required
- Fetch random article summaries or sections for `wikipedia` doc type
- Strip and reformat content; never reproduce full articles verbatim
- Mandatory attribution footer on every document using Wikipedia content:
  > *Source: Wikipedia contributors. "[Article Title]." Wikipedia, The Free Encyclopedia. [URL]. Licensed under CC BY-SA 4.0.*

### 9.4 Language Support (Phase 1)

Priority languages for v1.0: `en`, `fr`, `de`, `es`, `ur`, `ar`

Faker.js supports all of the above natively. Wikipedia API supports all languages via subdomain (`ur.wikipedia.org`, etc.).

---

## 10. Non-Functional Requirements

| Requirement | Specification |
|---|---|
| **Stateless** | Zero server-side storage. No logs of generated content. |
| **Privacy** | No PII collected. No cookies. No analytics. |
| **Performance** | Single document generation < 3 seconds client-side |
| **Availability** | GitHub Pages SLA (~99.9%). API on Cloudflare Workers (~99.99%). |
| **Reproducibility** | Same seed + same params = byte-identical output |
| **Open Source** | MIT License for all tool code |
| **Accessibility** | WCAG 2.1 AA compliance for UI |

---

## 11. Repository Structure

### 11.1 Recommended Repo Name

```
junkdocgen
```

**GitHub URL:** `https://github.com/lassiipk/junkdocgen`  
**GitHub Pages URL:** `https://lassiipk.github.io/junkdocgen`

Rationale: Short, descriptive, no ambiguity, easy to type, searchable.

Alternative names considered:

| Name | Verdict |
|---|---|
| `fake-doc-factory` | Too long |
| `synthetic-docs` | Too generic |
| `docforge` | Doesn't signal "fake/junk" clearly enough |
| `junkdocgen` | ✅ Best balance of clarity and brevity |

### 11.2 Repo Structure

```
junkdocgen/
├── README.md
├── LICENSE                        # MIT
├── ATTRIBUTION.md                 # CC BY-SA 4.0 Wikipedia attribution policy
│
├── docs/                          # GitHub Pages source
│   ├── index.html                 # Generator UI
│   ├── api.html                   # API docs (Swagger UI)
│   ├── about.html                 # Legal / about page
│   ├── assets/
│   │   ├── css/
│   │   ├── js/
│   │   │   ├── main.js            # UI logic
│   │   │   ├── generator.js       # Core generation engine
│   │   │   ├── formats/           # Per-format exporters
│   │   │   │   ├── pdf.js
│   │   │   │   ├── docx.js
│   │   │   │   ├── txt.js
│   │   │   │   ├── json.js
│   │   │   │   ├── csv.js
│   │   │   │   ├── html.js
│   │   │   │   └── markdown.js
│   │   │   ├── templates/         # Document templates per type
│   │   │   │   ├── legal/
│   │   │   │   ├── financial/
│   │   │   │   ├── resume/
│   │   │   │   ├── article/
│   │   │   │   └── lorem/
│   │   │   └── wikipedia.js       # Wikipedia API integration
│
├── api/                           # Cloudflare Worker / serverless API
│   ├── worker.js                  # API entry point
│   ├── routes/
│   │   ├── generate.js
│   │   ├── schema.js
│   │   └── health.js
│   └── wrangler.toml              # Cloudflare config
│
├── PRD.md                         # This document
└── openapi.yaml                   # API specification
```

---

## 12. Milestones & Phases

### Phase 1 — MVP (v1.0)
- [ ] Core generation engine (lorem, mixed, article types)
- [ ] TXT, JSON, HTML, Markdown output formats
- [ ] UI with basic controls (doc type, word count, seed, language)
- [ ] GitHub Pages deployment

### Phase 2 — Full Format Support (v1.1)
- [ ] PDF and DOCX output
- [ ] Legal, financial, resume document types
- [ ] Realism level and randomness controls
- [ ] Bulk generation + ZIP download

### Phase 3 — API (v1.2)
- [ ] REST API on Cloudflare Workers
- [ ] OpenAPI spec + Swagger UI embedded in docs page
- [ ] Rate limiting
- [ ] Wikipedia integration with CC BY-SA attribution

### Phase 4 — Polish (v1.3)
- [ ] Multilingual support (ur, ar, fr, de, es)
- [ ] Dark mode
- [ ] WCAG 2.1 AA audit
- [ ] Contribution guide + issue templates

---

## 13. Open Questions (To Resolve Before Dev Starts)

| # | Question | Owner |
|---|---|---|
| 1 | Will the API be hosted on Cloudflare Workers or another platform? | lassiipk |
| 2 | Should Wikipedia content be fetched live (real-time API) or pre-cached as a static corpus? | lassiipk |
| 3 | Is Urdu (ur) required in Phase 1 or can it wait for Phase 4? | lassiipk |
| 4 | Should bulk ZIP downloads include a manifest JSON listing all seeds used? | lassiipk |
| 5 | Is there a target benchmark dataset format this tool must be compatible with (e.g., TREC, BEIR)? | lassiipk |

---

*PRD Version 1.0 — Author: lassiipk — Last Updated: 2026-05-02*
