## General results

## Export approach (how PDFs were generated)

Goal:

- Export each corpus document to PDF using the method’s "native" export pipeline where possible (or the intended conversion toolchain), then validate the result.
- Tried to make UA-2 pdf if possible, where not possible or could not get it to pass use 1.7 and UA-1.

What I recorded per method

- PDF/UA Validation (AxesCheck/VeraPDF)
- Structure preservation (headings/lists/tables/figures/alt-text). Manually check whether there is at least some logical structure to the document still.
- and whether the tool forced/promoted PDF/UA settings.

Tools used to do checks (manually or automatically)

- AxesCheck: https://check.axes4.com (online checker, instead of PAC)
- VeraPDF CLI: https://verapdf.org/home/ (checker)
- Pave 2.0: https://pave-pdf.org/ (to easily look at the structure tags)

## Methods (E1: PDF/UA generation & validation)

### P1: Word (Native)

| Field                    | Result [File A](./outputs/P1_A.pdf)                                            | Result File B |
| ------------------------ | ------------------------------------------------------------------------------ | ------------- |
| Original file extension  | [`.docx`](./corpus/FILE_A.docx)                                                |               |
| AxesCheck pass/fail      | [fail](https://check.axes4.com/en/result/330d103c-65d2-4097-b5c3-9e56983d5222) |               |
| VeraPDF pass/fail        | fail                                                                           |               |
| Heading structure        | x                                                                              |               |
| List structure           | x                                                                              |               |
| Table structure          | x                                                                              |               |
| Figure/caption structure | x                                                                              |               |
| Alt text                 | x                                                                              |               |
| Paragraph Versions       | - Everything is a 'Paragraph', code blocks or quotes are not seperately tagged |               |

#### Interesting facts

- It does add all structure parts, iff entered correctly by the user (which is not trivial)
- Everything that is not a header, figure, link, or table is denoted as a 'Paragraph'. Even though some blockquotes or codeblocks were highlighted
- Nested Lists are not correctly tagged, instead it keeps being 1 layer where the 2a and 2b are just all text after each other.
- Footnotes are recorded as one large link instead of a link to a footnote, and a footnote tag.
- The printing version (= system's default) has no structure at all.
- Difficult to correctly adhere to tagging principles because the styling is so flexible. I can create a paragraph which looks like a header, but will not be tagged as such.
- No option to explicitely choose standard, no pdf version

#### PDF/UA prompting & specs

Did it prompt for UA specs?

- It silent prompts you in the footer (see [image](./assets/word_accessibility.png)).
- It does not prompt you in any way when exporting to a PDF.
- Default setting when installing Word is to not use the accesibility option to export, which requires an internet connection. Once selected tho it does remember across document as well.

What did it ask for / force you to set?

- Colour & Contrast
- Media and Illustrations (alt texts)
- Tables
- Headers, but i could not get it to trigger, even when mismatching headers or not using headers at all in the page.
- Document Access

Random references used:

- https://learn.microsoft.com/en-us/office/pdf/word/wordpdfaccessibility

---

### P2: LibreOffice (Native)

| Field                    | Result [File A](./outputs/P2_A.pdf)                                                               | Result File B |
| ------------------------ | ------------------------------------------------------------------------------------------------- | ------------- |
| Original file extension  | [`.odt`](./corpus/FILE_A.odt)                                                                     |               |
| AxesCheck pass/fail      | [fail (only 1 error tho)](https://check.axes4.com/en/result/10a18275-047f-48d3-bfa9-7849b67c0cd2) |               |
| VeraPDF pass/fail        | fail (1 error)                                                                                    |               |
| Heading structure        | x                                                                                                 |               |
| List structure           | x                                                                                                 |               |
| Table structure          | x                                                                                                 |               |
| Figure/caption structure | x                                                                                                 |               |
| Alt text                 | x                                                                                                 |               |
| Paragraph Versions       | x (except Quote-block)                                                                            |               |

#### Interesting facts

- Footnotes are nicely labebelled as such
- Paragraphs are shown as 'Standard' instead of 'Paragraph'
- Preformatted text is correctly indicated as such
- Again, its only correct because the user has to put some effort into using the correct 'styling'. Most likely users will just restyle with changing the type of pagraph (w/ codeblock for example).
- PDF version and standard selectable

#### PDF/UA prompting & specs

Did it prompt for UA specs?

- Yes, for errors it prompts when exporting to a PDF (see [image](./assets/libreoffice_accessibility.png)).

What did it ask for / force you to set?

- See the reference below. Mainly accessibility things, and loosely making sure that headers are entered correctly. It was a bit stricter on the headings. Making Header 1 to 3 it prompted to always use a Header 1 if there are any other headers, and that heaeder structure was broken (Word did not prompt to always use Header 1).

Random references used:

- Here the specifications of what accessibility means to LibreOffice are talked about: https://help.libreoffice.org/latest/en-US/text/shared/01/ref_pdf_export_universal_accessibility.html

---

### P3: LuaLaTeX (using Overleaf)

| Field                    | Result [File A](./outputs/P4_A.pdf)                                            | Result [File B](./outputs/P4_B.pdf) |
| ------------------------ | ------------------------------------------------------------------------------ | ----------------------------------- |
| Original file extension  | `.tex`                                                                         |                                     |
| AxesCheck pass/fail      | [pass](https://check.axes4.com/en/result/63c2323b-08b2-446e-982c-9e39a06d2520) |                                     |
| VeraPDF pass/fail        | pass (ua-2)                                                                    |                                     |
| Heading structure        | x                                                                              |                                     |
| List structure           | x                                                                              |                                     |
| Table structure          | x                                                                              |                                     |
| Figure/caption structure | x                                                                              |                                     |
| Alt text                 | x                                                                              |                                     |
| Paragraph Versions       | x                                                                              |                                     |

#### Interesting facts

- Once I got the ua-2 document metadata correctly set it was quite easy to get a compliant pdf. I could not use certain packages due to them crashing, but using LaTeX's built-in features such as verbatim worked like a charm.
- Because of the NEED to use `\section`, `\subsection`, `\includegraphics`, it is easy to get the complete structure right. Because the 'language' itself forces you to use it in a more correct way. It is more cubersome to style text differently then just use the built-in 'styled' options already. IMPORTANT!
- PDF version and standard selectable
- Looking in PAVE all tags look very custom (could look into this more, perhaps a PAVE visualisation issue/thing)
- Formula's are correctly tagged
- Incorrect reading order for floating figures/tables (file B's table is said to be at the end of page 2 for reading).

#### PDF/UA prompting & specs

Did it prompt for UA specs?

- When using the taggedpdf package and correctly setting the document metadata as per the reference below you indeed see errors and warnings.

What did it ask for / force you to set?

- A variety of things, especially related to alt texts.

Random references used:

- https://docs.overleaf.com/writing-and-editing/creating-accessible-pdfs

---

### P4: Typst (Native)

| Field                    | Result [File A](./outputs/P4_A.pdf)                                            | Result File B |
| ------------------------ | ------------------------------------------------------------------------------ | ------------- |
| Original file extension  | `.typ`                                                                         |               |
| AxesCheck pass/fail      | [pass](https://check.axes4.com/en/result/4e37ae30-3a60-48e0-b843-682f011b5c4a) |               |
| PAC pass/fail            | pass                                                                           |               |
| Heading structure        | x                                                                              |               |
| List structure           | x                                                                              |               |
| Table structure          | x                                                                              |               |
| Figure/caption structure | x                                                                              |               |
| Alt text                 | x                                                                              |               |
| Paragraph Versions       | x                                                                              |               |

#### Interesting facts

- UA-2 not supported yet, will come
- UA-1 has to followed strictly, otherwise does not compile. Extremely easy to adhere to this. Especially when using the semantic guide they have created and reference in the acecssibility guidelines.

#### PDF/UA prompting & specs

Did it prompt for UA specs?

- Yes, aggressively, does not compile otherwise.

What did it ask for / force you to set?

Random references used:

- https://typst.app/docs/guides/accessibility/

---

### P5: Chrome (HTML “Save as PDF”)

| Field                    | Result [File A](./outputs/P5_A.pdf) | Result File B |
| ------------------------ | ----------------------------------- | ------------- |
| Original file extension  | `.html`                             |               |
| AxesCheck pass/fail      | fail                                |               |
| PAC pass/fail            | fail                                |               |
| Heading structure        | -                                   |               |
| List structure           | x                                   |               |
| Table structure          | -                                   |               |
| Figure/caption structure | x                                   |               |
| Alt text                 | -                                   |               |
| Paragraph Versions       | -                                   |               |

#### Interesting facts

- Interesting to see that some tags are added, but a table tag and paragraphs even are missing completely.
- As this is mainly used for printing, it kind of makes sense that all metadata is lost.

#### PDF/UA prompting & specs

Did it prompt for UA specs?

- Nope

What did it ask for / force you to set? n/a

Random references used: n/a

---

### P6: Pandoc (`.md` → `.tex` -> `.pdf`)

Running: `pandoc ./FILE_A.md --pdf-engine=lualatex -o ./out.pdf`

| Field                    | Result [File A](./outputs/P6_A.pdf)                                            | Result File B |
| ------------------------ | ------------------------------------------------------------------------------ | ------------- |
| Original file extension  | `.md`                                                                          |               |
| AxesCheck pass/fail      | [fail](https://check.axes4.com/en/result/4cf04be4-df39-46b5-a6b1-fbe68635f870) |               |
| PAC pass/fail            | fail                                                                           |               |
| Heading structure        | x                                                                              |               |
| List structure           |                                                                                |               |
| Table structure          | -                                                                              |               |
| Figure/caption structure | -                                                                              |               |
| Alt text                 | -                                                                              |               |
| Paragraph Versions       | -                                                                              |               |

#### Interesting facts

- Mainly structure errors.
- From what I can see in Pave there does seem to be quite a bit of structure preserved. Just not accordingly to the specific ua-2 spec.
- It has a similar non-adherence to the correct headings etc as to LuaLatex (obv.) (see [image](./assets/pandoc_md_pdf_pave.png)). I'd love to actually be able to see the tree structure as well. But i reckon that this can easily be used to extract the underlying structure. Lets see with the next experiment.

#### PDF/UA prompting & specs

Did it prompt for UA specs?

- Nope

What did it ask for / force you to set? n/a

Random references used:

- https://pandoc.org/MANUAL.html

---

### P7: Pandoc (`.md` → `.typ` -> `.pdf`)

Running: `pandoc ./FILE_A.md --pdf-engine=typst --pdf-engine-opt=--pdf-standard=ua-1 -o ../outputs/P7_A.pdf`

| Field                    | Result [File A](./outputs/P7_A.pdf) | Result File B |
| ------------------------ | ----------------------------------- | ------------- |
| Original file extension  | `.md`                               |               |
| AxesCheck pass/fail      | pass                                |               |
| PAC pass/fail            | pass                                |               |
| Heading structure        | x                                   |               |
| List structure           | x                                   |               |
| Table structure          | x                                   |               |
| Figure/caption structure | x                                   |               |
| Alt text                 | x                                   |               |
| Paragraph Versions       | x                                   |               |

---

### P8: Pandoc (`.html` -> `.typ` -> `.pdf`)

Running: `pandoc ./FILE_A_no-footnotes.html --pdf-engine=typst --pdf-engine-opt=--pdf-standard=ua-1 -o ../outputs/P8_A.pdf`

| Field                    | Result [File A](./outputs/P8_A.pdf) | Result File B |
| ------------------------ | ----------------------------------- | ------------- |
| Original file extension  | `.md`                               |               |
| AxesCheck pass/fail      | pass                                |               |
| PAC pass/fail            | pass                                |               |
| Heading structure        | x                                   |               |
| List structure           | x                                   |               |
| Table structure          | x                                   |               |
| Figure/caption structure | x                                   |               |
| Alt text                 | x                                   |               |
| Paragraph Versions       | x                                   |               |

#### Interesting facts

- Could not get the internal footnotes to work. When removing them all looks fine and its ua-1 compliant.

#### PDF/UA prompting & specs

Did it prompt for UA specs?

- Nope

What did it ask for / force you to set? n/a

Random references used:

- https://pandoc.org/MANUAL.html#typst

---

## Overall interesting things

- When using pandoc it basically depends on the underlying pdf-engine that you are using. It will try to convert any format into the engine's format (e.g, .typ, .tex) and then transforms it using that engine.
  - This makes it heavily reliant on whether a file correctly gets transformed into the format of the engine. Not a lot of files are correctly transformed into .typ files in such a way that we can transform them into PDF

- The structure of paragraphs, tables, figures, and links is different from time to time:
  - Sometimes a table is nested inside a paragraph, sometimes it is not.
  - Same with how footnotes are handled, sometimes 1 big link, sometimes 2 seperate, sometimes correctly 1 link and a footnote).
  - Sometimes other names are used (w/ LuaLaTeX).
