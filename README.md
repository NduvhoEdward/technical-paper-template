# witseiepaper — Wits EIE LaTeX template

The LaTeX house style of the School of Electrical & Information Engineering,
University of the Witwatersrand. Originally written by **Ken J. Nixon** (2005)
as the paper format for ELEN417/455 final-year laboratory projects.

Kept here so it is version-controlled and available from any machine, rather
than living as loose files in a folder.

## Files

| File | Purpose |
|---|---|
| `witseiepaper.cls` | The document class. This is the template. |
| `witseie.bst` | EIE bibliography style. |
| `KJN.sty` | Nixon's macro package (`\tabref`, `\figref`, and friends). |
| `witseie-paper-2005.tex` | The original sample paper — doubles as the style guide. |
| `witseie-paper-2005.pdf` | Reference render of the above. |
| `sample.bib` | Example bibliography. |
| `example.eps` / `example.pdf` | Example figure, in both formats. |

## Usage

The class takes the usual options. Two established configurations:

```latex
% Final-year project paper: A4, two columns, 6-page limit
\documentclass[10pt,twocolumn]{witseiepaper}

% Essay / report: A4, single column
\documentclass[12pt]{witseiepaper}
```

A minimal document:

```latex
\documentclass[12pt]{witseiepaper}
\usepackage{KJN}
\usepackage[authoryear]{natbib}
\bibliographystyle{plainnat}   % or witseie

\begin{document}
\title{Your Title}
\author{Your Name -- Student Number
\thanks{School of Electrical \& Information Engineering, University of the
Witwatersrand, Private Bag 3, 2050, Johannesburg, South Africa}}

\abstract{50--200 words.}
\keywords{four to six, alphabetical, comma separated}

\maketitle

\section{INTRODUCTION}
...

\bibliography{yourbib}
\end{document}
```

## Compiling on Overleaf

Overleaf does not know `witseiepaper`, so the class must travel with the
document. Either:

1. Download this repo as a zip and upload it to Overleaf as a new project, or
2. Copy `witseiepaper.cls`, `witseie.bst` and `KJN.sty` into an existing
   Overleaf project alongside your `.tex`.

Set the main document and compiler (pdfLaTeX) in Overleaf's menu. If you cite
with `natbib`, remember Overleaf needs a `.bib` file present to run BibTeX.

## Style rules the template enforces

Worth knowing before you fight the class:

- A4, text length 250 mm.
- Times/Roman serif face. Do not change font sizes or line spacing to fit
  more text in.
- Italics for emphasis; never underline.
- Abstract 50–200 words; four to six keywords, alphabetical.
- The project paper is capped at **6 pages**.

## Fixes applied to the 2005 original

The template as distributed compiles, but carries three latent defects. All
three are fixed here; the rendered output is unchanged (verified: same 4 pages,
same A4 geometry, same captions).

1. **Three-argument `\@makecaption` (the serious one).** The class passed the
   float type to `\@makecaption` as a third argument. Standard LaTeX — and so
   hyperref, which `KJN.sty` loads — assumes the two-argument signature. Once
   hyperref rewired `\@caption`, it called `\@makecaption` with two arguments,
   `#3` absorbed whatever token followed, and the `\ifx` testing it was left
   dangling. Every document built from this template since 2005 has emitted
   `(\end occurred when \ifx on line NNN was incomplete)` because of it. A
   dangling conditional is a live hazard: it will swallow the next `\fi` it
   meets. Now uses LaTeX's own `\@captype`, with standard signatures throughout.

2. **Packages loaded inside a conditional.** `KJN.sty` loaded `graphicx` and
   `hyperref` inside `\ifpdf ... \else ... \fi`. The conditional now only
   selects options; the packages load outside it.

3. **No paper size.** The class never set `\paperwidth`/`\paperheight`, leaving
   them at 0pt, so hyperref computed link destinations against a zero-height
   page. A4 is now stated explicitly.

Verified with MiKTeX 25.12 / pdfTeX 4.23: `pdflatex → bibtex → pdflatex ×2`
completes with **zero** errors and zero warnings.

## Provenance

The `witseiepaper` class, `witseie.bst` and `KJN.sty` were written by
**Ken J. Nixon** (2005) and are © School of Electrical & Information
Engineering, University of the Witwatersrand. They are distributed to students
from the school website.

This repository **redistributes** that material for convenience — it does not
claim authorship of it. If the school would rather it not be mirrored publicly,
it will be taken down on request.
