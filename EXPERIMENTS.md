# Explains how the experiments are run

In the `experiments` folder you can find the data, scripts, and results for a comparative study on document structure preservation and recovery. The main focus lies on the transition from source content to PDF/UA and then the extraction of structure using various tools.

## Corpus

All experiments use a set of three documents to ensure consistency:

- **Doc A (Semantic Article):** Based on the Markdown syntax tutorial (headings, lists, code, tables, alt-text).
- **Doc B (Accessibility Stress Test):** Focuses on edge cases (complex tables, skipped headings, specific metadata).
- **Doc C (Layout Stress Test):** Two-column academic-style layout with footnotes and figures.

## Scoring

We use a **Feature Rubric** to evaluate success across all experiments:

- **2:** Correctly preserved/recovered.
- **1:** Partially preserved or degraded (e.g., lost nesting, formatting issues).
- **0:** Lost or incorrect.

## Experiments

The three experiments are shown below:

### E1: PDF/UA Generation & Validation

Goal: Can common authoring pipelines produce valid, structurally faithful PDF/UA?

- Methods
  - `P1`: Word (Native)
  - `P2`: LibreOffice (Native)
  - `P3`: LuaLaTeX (CLI, Tagged Project)
  - `P4`: LuaLaTeX (Overleaf)
  - `P5`: Typst (Native)
  - `P6`: Typst (CLI)
  - `P7`: Chrome (html save as PDF)
  - `P8`: Pandoc (.typ -> .pdf)
  - `P9`: Pandoc (.docx -> .pdf)
  - `P10`: Pandoc (.md -> .pdf)
  - `P11`: Pandoc (.md -> .docx -> .pdf)
  - `P12`: Pandoc (.md -> .tex -> .pdf)
- How? Export the corpus via each method and validate using PAC, VeraPDF, and Adobe Preflight.

### E2: Fault Injection (Enforcement Test)

Goal: Do tools prevent or warn about accessibility errors?

- How? Introduce deliberate errors in source files (e.g., missing text, missing alt-text, skipped heading levels).
- Metrics: Record if the tool blocks export, issues a warning, or silently generates an invalid PDF.

### E3: Round-Trip Recovery

Goal: Can structure be recovered from a valid PDF/UA into a reusable format?

- How? Take the best PDF/UA outputs from E1 and extract them back to Markdown/HTML/JSON.
- Tools: `pdftohtml`, `PyMuPDF`, `Docling`, `Adobe Extract`.
- Metrics: Compare extracted structure against the original source using the 0/1/2 rubric.

### E4: Extraction from Untagged PDFs

Goal: Evaluation of fallback performance on born-digital PDFs without a tag tree.

- Taxonomy:
  - **Class I (Geometry):** `pdftotext`, `pdftohtml`.
  - **Class II (Advanced Parser):** `PyMuPDF`, `pdfplumber`.
  - **Class III (VLM/AI):** `Docling`, `Marker`, `MinerU`.
- Metrics: Text similarity and structural reconstruction accuracy.

## Structure

```
/corpus - Original files
/output - Outputted pdf files per method
```

## Todo's

- Build the `corpus/` and `gold/` standards.
- Generate PDFs (`E1`) and run `PAC` validation.
- Perform round-trip extraction (`E3`) on tagged PDFs.
- Run extraction (`E4`) on untagged and scanned variants.

- Edit distance / Cosine similarity voor boven

- Converters beschijven + experimentjes

- Wiskunde formules
