# Physics Notes LaTeX Template

## Project Structure

- `main.tex`: the master file; it includes the title page, table of contents,
  chapters, appendix, and bibliography.
- `preamble.tex`: packages, styles, theorem environments, and custom commands.
- `chapters/`: one independently compilable `subfiles` document per chapter.
- `bib.tex`: prints the bibliography at the end of the master document.
- `references.bib`: the BibLaTeX database.
- `images/`: image assets.
- `USAGE_GUIDE.md`: the complete usage tutorial.

## Compilation

The template uses BibLaTeX and is configured for XeLaTeX + Biber:

```bash
latexmk -xelatex main.tex
```

For a manual build, run:

```bash
xelatex main.tex
biber main
xelatex main.tex
xelatex main.tex
```

Each chapter can also be compiled independently. Run XeLaTeX directly for a
chapter without citations; use `latexmk` for a chapter with citations:

```bash
cd chapters
latexmk -xelatex 04-quantum-mechanics.tex
```

## Adding a Chapter

Copy any file in `chapters/`, update its title and label, and add the following
line to `main.tex`:

```tex
\subfile{chapters/your-file-name}
```

## Package Notes

The template includes support for mathematics, tensors, Dirac notation, SI
units, chemical and nuclear equations, TikZ, PGFPlots, circuit diagrams,
tables, code listings, note boxes, intelligent cross-references, and
bibliography management. `tikz-feynman` is left as an optional package in
`preamble.tex` because it generally works best with LuaLaTeX.

The legacy `physics` package is not loaded because commands such as `\qty` can
conflict with current versions of `siunitx`. Common commands for derivatives,
partial derivatives, vectors, commutators, and related notation are provided
directly in `preamble.tex`.
