# Literature Review Building Guide

**Objective:** Set up a workflow for literature reviews with Zotero integration in VS Code: manage papers in Zotero, automatically export references, and compare papers in reusable LaTeX tables.

## Contents

1. [Prepare the tools](#1-prepare-the-tools)
2. [Install Better BibTeX](#2-install-better-bibtex)
3. [Create and synchronize references.bib](#3-create-and-synchronize-referencesbib)
4. [Set up the LaTeX preamble](#4-set-up-the-latex-preamble)
5. [Add papers and call the bibliography](#5-add-papers-and-call-the-bibliography)
6. [Build and maintain the review](#6-build-and-maintain-the-review)

## 1. Prepare the tools

- Install Zotero and VS Code.
- Install a LaTeX distribution, such as MacTeX on macOS.
- In VS Code, install **LaTeX Workshop** by James Yu and open the cloned `Literature_Review_Framework` folder.

Use this project structure:

```text
Literature_Review_Framework/
├── main_LR.tex
├── preamble.tex
├── references.bib
└── other_material/
    └── LR_building_guide.md
```

## 2. Install Better BibTeX

1. Download the `.xpi` file linked from the [Better BibTeX installation page](https://retorque.re/zotero-better-bibtex/installation/).
2. In Zotero, open **Tools → Plugins**.
3. Click the gear icon → **Install Plugin From File…** and select the `.xpi` file.
4. Complete installation and restart Zotero if prompted.
5. Create or select a collection for the review, such as `LR_correlation`, and add your papers.

Better BibTeX is installed in **Zotero**, not VS Code.

## 3. Create and synchronize references.bib

1. Right-click the collection in Zotero → **Export Collection…**.
2. Select **Better BibTeX** as the format, for use with `natbib` and BibTeX.
3. Enable **Keep updated**.
4. Save as `references.bib` in the project folder, next to `main_LR.tex`.

`references.bib` is a **file**, not a folder. This repository includes two sample entries so the template builds immediately. Import it into Zotero before replacing it if you want to retain those examples, or replace the sample table rows with your own citation keys.

Keep Zotero running to process updates. Edit reference metadata in Zotero, since automatic exports regenerate the `.bib` file. Manage the export under **Zotero Settings → Better BibTeX → Automatic exports**. See the [automatic export documentation](https://retorque.re/zotero-better-bibtex/exporting/auto/).

For public repositories, exclude attachment paths and private notes from your Zotero export.

The `http://127.0.0.1:23119/better-bibtex/...` link is a separate local export mechanism; it is not needed for this workflow. Remove any `filecontents*` block that previously generated the bibliography in your `.tex` file.

## 4. Set up the LaTeX preamble

The repository already includes `preamble.tex`. For a new project, save the following in that file. Load it once from the main document using `\input{preamble}`; remove duplicate package and command definitions from `main_LR.tex`.

```latex
\usepackage[T1]{fontenc}
\usepackage[utf8]{inputenc}
\usepackage[authoryear,round]{natbib}
\usepackage[english]{babel}
\usepackage[a4paper,margin=2cm]{geometry}
\usepackage{lmodern}
\usepackage{microtype}
\usepackage{array}
\usepackage{booktabs}
\usepackage{xltabular}
\usepackage[hidelinks]{hyperref}

% One paper: citation key, research question, method, result.
\newcommand{\papier}[4]{%
  \citet{#1} & #2 & #3 & #4 \\
  \addlinespace[0.7em]
}

% Reusable table with automatic page breaks and repeated headings.
\newcommand{\tableaurevue}[1]{%
  \begingroup
  \small
  \renewcommand{\arraystretch}{1.2}
  \setlength{\tabcolsep}{5pt}

  \begin{xltabular}{\textwidth}{
    @{}
    >{\raggedright\arraybackslash}p{0.18\textwidth}
    *{3}{>{\raggedright\arraybackslash}X}
    @{}
  }
    \toprule
    \textbf{Citation} &
    \textbf{Research question} &
    \textbf{Method} &
    \textbf{Result} \\
    \midrule
    \endhead

    #1

    \bottomrule
  \end{xltabular}
  \endgroup
}
```

`\newcommand` creates a command; `\renewcommand` changes an existing one. Here, `\renewcommand{\arraystretch}{1.2}` sets the table's row spacing.

## 5. Add papers and call the bibliography

The repository includes a working `main_LR.tex`. Use this structure when adapting it. The example key already exists in this project's `references.bib`; replace the placeholder cell text with your reading notes.

```latex
\documentclass[11pt,a4paper]{article}
\input{preamble}

\begin{document}

\section*{Literature review}

\tableaurevue{
  \papier
    {canalesAssessingTemporalComplementarity2020}
    {State the research question.}
    {Summarize the data and method.}
    {Summarize the main result and relevant limitations.}

  % Add further papers here:
  % \papier{anotherKey}{Research question}{Method}{Result}
}

\bibliographystyle{plainnat}
\bibliography{references}

\end{document}
```

- Copy each citation key from Zotero's **Citation Key** field or the `.bib` entry: `@article{citationKey, ...}`.
- Add one `\papier{key}{question}{method}{result}` per paper; call `\tableaurevue{...}` again for another theme.
- Use `\bibliography{references}` without the `.bib` extension.
- Keep `\tableaurevue` outside a `table` environment so it can span pages.
- For ordinary text, use `\citet{key}` for Author (year) or `\citep{key}` for (Author, year).

## 6. Build and maintain the review

1. Open `main_LR.tex` in VS Code and run **LaTeX Workshop: Build LaTeX project** from the Command Palette.
2. For a manual build, run `latexmk -pdf main_LR.tex` in the project folder; it handles LaTeX and BibTeX passes.
3. Add or edit papers in Zotero, wait for the automatic export, then rebuild.
4. If a citation appears as `?`, check that its exact key is in `references.bib` and that the BibTeX build completed.

LaTeX Workshop suggests bibliography keys inside citation commands such as `\citep{...}`. See its [citation completion documentation](https://github.com/James-Yu/LaTeX-Workshop/wiki/Intellisense#citations).
