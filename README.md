Salut, j'ai bidouillé ça vite fait avec la version desktop de GPT-6 Astra 'Très Élevé'. Ça fait le taff pour une revue de littérature à condition d'être à l'aise avec VS Code, LaTeX et Zotero (je considère que la prise de note est faite directement via Zotero - pour les recalcitrants et adorateur de Notion/Obsidian, il existe une version 'Notero' de Zotero+Notion, ça vaut ce que ça vaut mais ça a le mérite d'exister). En vous souhaitant une bonne utilisation (si tant est qu'elle le soit !) du soleil et d'entendre les cigales.

# Literature Review Framework

A reusable LaTeX framework for comparing research papers, with Zotero bibliography synchronization and VS Code support.

Each paper is one table row: **Citation · Research question · Method · Result**. Tables span pages automatically and repeat their headings.

## Quick start

1. Install a TeX distribution with **pdfLaTeX, BibTeX and latexmk**: [MacTeX](https://www.tug.org/mactex/) on macOS, [TeX Live](https://www.tug.org/texlive/) on Linux, or [MiKTeX](https://miktex.org/) on Windows. Install missing LaTeX packages if prompted; latexmk on Windows also requires Perl. I recommend to use VS code for a better compatibility with GitHub, but it is more a personal tasted rather than a polished advice.
2. Clone or download this repository:

   ```sh
   git clone https://github.com/LeLubb/Literature_Review_Framework.git
   cd Literature_Review_Framework
   ```

3. Open the folder in VS Code and install the recommended **LaTeX Workshop** extension.
4. Open `main_LR.tex` and run **LaTeX Workshop: Build LaTeX project**, or build from the project folder:

   ```sh
   latexmk -pdf main_LR.tex
   ```

5. Open `main_LR.pdf`. The included sample bibliography works immediately; Zotero is optional for this first build.

## Add your papers

Edit `main_LR.tex`:

```latex
\tableaurevue{
  \papier
    {yourBibTeXKey}
    {What question does the paper address?}
    {Which data and methods does it use?}
    {What does it find, and what are its limitations?}
}
```

Replace `yourBibTeXKey` with a key from `references.bib`. Add one `\papier` per paper and one `\tableaurevue` per theme. The included wind/solar examples are short illustrations, not complete critical reviews.

## Connect Zotero

1. Install [Better BibTeX in Zotero](https://retorque.re/zotero-better-bibtex/installation/).
2. Right-click your collection → **Export Collection…**.
3. Choose **Better BibTeX**, enable **Keep updated**, and save as `references.bib` in this folder.
4. Replace the example citation keys in `main_LR.tex` with keys from your export.

To keep the example papers, import the supplied `references.bib` into Zotero first. Zotero must be running to process export updates. Edit bibliography metadata in Zotero; exports regenerate the file. Before publishing your own export, omit local attachment paths and private notes.

See the [step-by-step building guide](other_material/LR_building_guide.md) and [Better BibTeX export documentation](https://retorque.re/zotero-better-bibtex/exporting/auto/).

## Files

| File | Purpose |
| --- | --- |
| `main_LR.tex` | Review text and example comparison table |
| `preamble.tex` | Packages and reusable table commands |
| `references.bib` | Two sample references; replace with your Zotero export |
| `other_material/LR_building_guide.md` | Zotero setup, preamble and writing instructions |
| `.vscode/` | Recommended extension and latexmk build recipe |
| `.gitignore` | Excludes generated build files |
| `LICENSE` | MIT license for the framework |

## Customize

- Change the title, sections and table rows in `main_LR.tex`.
- Edit `preamble.tex` to adjust font size, column widths or row spacing.
- Keep `\bibliographystyle{plainnat}` and `\bibliography{references}` for the supplied natbib/BibTeX setup.
- Use `\citet{key}` for Author (year) and `\citep{key}` for (Author, year).
- Keep `\tableaurevue` outside a floating `table` environment to allow page breaks.

## Troubleshooting

- **Citation shows `?`:** check the exact key in `references.bib`, then rebuild with latexmk.
- **Package or latexmk missing:** install it through your TeX distribution.
- **Bibliography stops updating:** check Zotero → Better BibTeX → Automatic exports.
- **Need a clean build:** run `latexmk -C main_LR.tex`, then rebuild. This also removes the generated PDF.

## License

The framework is available under the [MIT License](LICENSE). Referenced papers remain subject to their own copyright.
