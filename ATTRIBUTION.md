# Attribution

## Wikipedia Content

JunkDocGen's **Wikipedia-style** document type is structured after the format of encyclopedia articles. Body text is fully synthetic and algorithmically generated. Topic names and article structure may reference real Wikipedia articles.

When a generated document is derived from or references a Wikipedia article, the following attribution applies:

> *Source: Wikipedia contributors. "[Article Title]." Wikipedia, The Free Encyclopedia. [https://en.wikipedia.org/wiki/[Article_Title]](https://en.wikipedia.org). Licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).*

This attribution is embedded automatically in every Wikipedia-style document produced by JunkDocGen — in the document footer (HTML, PDF, DOCX) and in the `meta.license` field (JSON, CSV).

### What CC BY-SA 4.0 Requires

If you use, publish, or distribute documents generated in Wikipedia-style format:

1. **Attribution** — Credit Wikipedia contributors and link to the source article.
2. **License notice** — State that the work is licensed under CC BY-SA 4.0.
3. **ShareAlike** — If you adapt or build on the content, distribute your version under the same CC BY-SA 4.0 license.

Full license text: [https://creativecommons.org/licenses/by-sa/4.0/legalcode](https://creativecommons.org/licenses/by-sa/4.0/legalcode)

### What This Does NOT Apply To

CC BY-SA 4.0 obligations apply **only** to Wikipedia-style output. All other document types (legal, financial, resume, article, lorem, mixed) are 100% synthetic with no Wikipedia-derived content. Those documents are released under the MIT License along with the rest of this tool.

---

## Third-Party Libraries

JunkDocGen loads the following libraries at runtime via CDN:

| Library | Version | License | Purpose |
|---|---|---|---|
| [jsPDF](https://github.com/parallax/jsPDF) | 2.5.1 | MIT | Client-side PDF generation |
| [seedrandom](https://github.com/davidbau/seedrandom) | 3.0.5 | MIT | Seeded PRNG for reproducibility |
| [IBM Plex Mono](https://fonts.google.com/specimen/IBM+Plex+Mono) | — | OFL 1.1 | UI monospace font |
| [Syne](https://fonts.google.com/specimen/Syne) | — | OFL 1.1 | UI display font |

None of these libraries are bundled in this repository. They are loaded from `cdnjs.cloudflare.com` and `fonts.googleapis.com` at runtime. Their licenses are reproduced at the links above.

---

## Tool License

The JunkDocGen tool itself — all source code in this repository — is released under the **MIT License**.

```
MIT License

Copyright (c) 2026 lassiipk

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

*Last updated: 2026-05-02 — lassiipk/junkdocgen*
