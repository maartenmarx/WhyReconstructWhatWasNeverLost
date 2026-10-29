---
title: File A benchmark
author: Floris
date: "2026-03-26"
lang: en
---

# Introduction

This document is intended as a small semantic benchmark for PDF/UA generation
and structure recovery. It contains headings, paragraphs, lists, links, code
blocks, a blockquote, a table, a figure, and footnotes.

The main goal is to test whether common authoring pipelines preserve document
structure from source to PDF and back again. For a general reference on
Markdown, see the
[Markdown Guide](https://www.markdownguide.org/basic-syntax/).

## Headings and Paragraphs

A structured document should preserve heading hierarchy, paragraph flow, and
inline semantics. This paragraph contains **bold text**, _italic text_, and
`inline code` to test whether emphasis and code are preserved during
conversion.

### Inline Semantics

Semantic preservation matters because a heading is not just larger text, and a
list is not just visually indented text. A machine-readable document should
retain these distinctions so that downstream extraction tools can recover them
reliably.[^a1]

## Lists

Below is an unordered list with nesting:

- First bullet item
- Second bullet item
  - Nested bullet item
  - Another nested bullet with a
    [link to the W3C](https://www.w3.org/)
- Third bullet item

Below is an ordered list with nesting:

1. Prepare the source file.
2. Export the document to PDF.
   1. Enable accessibility tags if the tool supports them.
   2. Record the export settings.
3. Validate the resulting PDF.
4. Compare the extracted output against the source.

## Blockquote

> A document can look visually correct while still being structurally broken.
> Accessibility and machine readability depend on the logical structure being
> preserved, not only on the page layout.

## Code Block

The following shell command illustrates a simple conversion workflow:

```bash
pandoc doc-a-semantic-article.md -o doc-a.tex
lualatex doc-a.tex
```

A second code example in Python:

```python
from pathlib import Path

source = Path("doc-a-semantic-article.md")
print(f"Processing {source.name}")
```

## Table Example

| Element  | Example          | Purpose                              |
| -------- | ---------------- | ------------------------------------ |
| Heading  | '# Introduction' | Encodes section hierarchy            |
| Emphasis | '\*'             | Marks strong importance              |
| Link     | \'\[\]\(\)\'     | Preserves navigation and references  |
| Footnote | '^1'             | Adds supporting notes                |
| Code     | 'inline'         | Distinguishes literal code from text |

## Figure Example

![A picture of a bench with coffee and a croissant.](../assets/FILE_A_IMG.jpg)

_Figure A1. Example figure used to test image handling, alt text, and caption
preservation._

## Footnotes and Closing

This sentence includes a footnote for testing purposes.[^a2] This second
sentence includes another footnote to test multiple notes in the same
document.[^a3]

In summary, this document is meant to be simple, readable, and rich enough to
test whether structure survives across authoring, PDF generation, validation,
and extraction workflows.

[^a1]:
    In this benchmark, "structure" includes headings, lists, tables,
    figures, links, reading order, and metadata.

[^a2]:
    A footnote should remain associated with the correct reference point in
    the main text.

[^a3]:
    Some pipelines preserve footnote text but lose the reference structure
    or numbering semantics.
