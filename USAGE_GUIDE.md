# Physics Notes LaTeX Template: User Guide

This guide explains how to use the template efficiently for recording,
organizing, and reviewing physics notes. The recommended workflow is simple:
**compile the current chapter while writing, then compile `main.tex`
periodically to verify the full document, table of contents, references, and
layout.**

## 1. Project Structure

```text
physics-notes-template/
├── main.tex
├── preamble.tex
├── bib.tex
├── references.bib
├── chapters/
│   ├── 01-mathematical-tools.tex
│   ├── 02-classical-mechanics.tex
│   ├── 03-electromagnetism.tex
│   ├── 04-quantum-mechanics.tex
│   └── appendix-constants.tex
├── images/
├── README.md
└── USAGE_GUIDE.md
```

- `main.tex`: the master document. It controls the title page, table of
  contents, chapter order, appendices, and bibliography.
- `preamble.tex`: contains packages, page styles, theorem environments, and
  custom physics commands.
- `chapters/`: contains the body of the notes. Every chapter is an independently
  compilable `subfiles` document.
- `references.bib`: stores books, papers, and other bibliography entries.
- `bib.tex`: prints the bibliography at the end of the complete notes.
- `images/`: stores diagrams, plots, screenshots, and photographs.

## 2. Compile the Complete Notes

From the project root, run:

```bash
latexmk -xelatex main.tex
```

`latexmk` automatically runs XeLaTeX as many times as necessary and invokes
Biber when needed. This resolves the table of contents, cross-references,
citations, and bibliography.

For a manual build, run:

```bash
xelatex main.tex
biber main
xelatex main.tex
xelatex main.tex
```

To remove auxiliary build files, run:

```bash
latexmk -c
```

## 3. Compile Only the Current Chapter

You do not need to compile the complete book after every change. While working
on a chapter, enter the `chapters/` directory and compile only that file:

```bash
cd chapters
latexmk -xelatex 02-classical-mechanics.tex
```

This is considerably faster for a large notebook. After completing a section
or a writing session, return to the project root and compile the full document:

```bash
cd ..
latexmk -xelatex main.tex
```

Use `latexmk` for chapters containing citations so that Biber runs
automatically.

## 4. Add a New Chapter

Copy an existing chapter:

```bash
cp chapters/02-classical-mechanics.tex chapters/05-thermodynamics.tex
```

Use the following structure in the new file:

```tex
% !TEX root = ../main.tex
\documentclass[../main.tex]{subfiles}

\begin{document}

\chapter{Thermodynamics}
\label{chap:thermodynamics}

% Write your notes here.

\end{document}
```

Then add the chapter to `main.tex`:

```tex
\subfile{chapters/05-thermodynamics}
```

The order of the `\subfile` commands in `main.tex` determines the chapter
order in the final PDF. The table of contents updates automatically.

## 5. Recommended Chapter Structure

A consistent chapter structure makes the notes easier to review:

```tex
\chapter{Classical Mechanics}
\label{chap:mechanics}

\section{Core Concepts}

\begin{definition}
A concise definition.
\end{definition}

\section{Main Equations}

\begin{equation}
  \vect{F} = \dv{\vect{p}}{t}.
  \label{eq:newton-second-law}
\end{equation}

\section{Worked Examples}

\begin{example}
Consider a particle moving under a constant force.
\end{example}

\section{Common Pitfalls}

\begin{warningbox}
Always state the reference frame before applying Newton's laws.
\end{warningbox}

\begin{summarybox}
Record the central results, assumptions, and connections here.
\end{summarybox}
```

A useful sequence is: concepts, equations, derivations, examples, common
pitfalls, and summary.

## 6. Common Physics Commands

The following commands are already defined in `preamble.tex`:

```tex
\vect{v}                     % bold vector
\uvect{n}                    % unit vector
\dv{x}{t}                    % ordinary derivative
\pdv{\psi}{t}                % partial derivative
\grad \phi                   % gradient
\laplacian \phi              % Laplacian
\comm{\hat{x}}{\hat{p}}      % commutator
\acomm{\hat{A}}{\hat{B}}     % anticommutator
\expect{\hat{H}}             % expectation-style brackets
\absq{\psi}                  % |psi|^2
\ket{\psi}                   % ket
\bra{\psi}                   % bra
```

For example, the time-dependent Schrödinger equation can be written as:

```tex
\begin{equation}
  \ii\hbar\pdv{}{t}\ket{\psi(t)}
  = \hat{H}\ket{\psi(t)}.
\end{equation}
```

If you repeatedly use a notation that is not included, define a new command in
`preamble.tex` instead of redefining it in multiple chapters.

## 7. Units and Numerical Values

Use `siunitx` for numerical values and units. Do not insert spaces or format
units manually.

```tex
\SI{9.81}{\metre\per\second\squared}
\SI{1.602e-19}{\coulomb}
\SIrange{400}{700}{\nano\metre}
\si{\joule\per\kelvin}
```

This keeps unit fonts, number formatting, and spacing consistent throughout the
notes.

## 8. Equations and Cross-References

Add a `\label` to every equation that may be referenced later:

```tex
\begin{equation}
  E = mc^2.
  \label{eq:mass-energy}
\end{equation}
```

Reference it with `\cref`:

```tex
Using \cref{eq:mass-energy}, we find that ...
```

The `cleveref` package automatically inserts words such as `equation`,
`figure`, and `table`.

Use consistent label prefixes:

```text
chap:mechanics
sec:lagrangian
eq:euler-lagrange
fig:phase-space
tab:physical-constants
```

Every label should be globally unique and describe the content it identifies.

## 9. Manage Images

Place all image assets in `images/` and use descriptive English file names:

```text
images/
├── free-body-diagram.pdf
├── electric-field.png
└── experiment-setup.jpg
```

Include an image with:

```tex
\begin{figure}[htbp]
  \centering
  \includegraphics[width=0.7\textwidth]{free-body-diagram.pdf}
  \caption{Free-body diagram of the system.}
  \label{fig:free-body-diagram}
\end{figure}
```

Reference the figure with:

```tex
As shown in \cref{fig:free-body-diagram}, ...
```

Use vector formats such as PDF for equations, geometric diagrams, and
schematics. Use PNG or JPEG for photographs and screenshots. Avoid vague file
names such as `image1.png`.

## 10. Draw Circuits and Plots

CircuitikZ is already loaded. For example:

```tex
\begin{circuitikz}
  \draw (0,0) to[battery1,l=$V$] (0,2)
    to[R,l=$R$] (3,2)
    to[C,l=$C$] (3,0) -- (0,0);
\end{circuitikz}
```

Use PGFPlots for function plots:

```tex
\begin{tikzpicture}
  \begin{axis}[
    xlabel={$x$},
    ylabel={$V(x)$},
    grid=major
  ]
    \addplot[domain=-2:2,samples=100] {x^2};
  \end{axis}
\end{tikzpicture}
```

For complex diagrams that are reused, place the drawing code in a separate
file and include it with `\input`.

## 11. Manage References

Add books and papers to `references.bib`:

```bibtex
@book{goldstein2002,
  author    = {Goldstein, Herbert and Poole, Charles and Safko, John},
  title     = {Classical Mechanics},
  edition   = {3},
  publisher = {Addison-Wesley},
  year      = {2002}
}
```

Cite the source in a chapter:

```tex
The Hamiltonian formulation is discussed in \cite{goldstein2002}.
```

`bib.tex` automatically prints the bibliography at the end of the complete
notes. After adding or changing citations, use `latexmk` or the complete
XeLaTeX + Biber build sequence.

## 12. Suggested Chapter Organization

A long-term physics knowledge base might use the following structure:

```text
01-mathematical-tools.tex
02-classical-mechanics.tex
03-electromagnetism.tex
04-quantum-mechanics.tex
05-thermodynamics.tex
06-statistical-mechanics.tex
07-relativity.tex
08-optics.tex
appendix-constants.tex
```

You can also maintain separate template projects for different courses,
textbooks, or research areas.

## 13. Efficient Note-Taking Habits

1. Give each topic its own `\section` or `\subsection`.
2. Record assumptions, symbol definitions, and dimensions beside important
   equations.
3. Separate derivations from final results, and place the final conclusions in
   a `summarybox`.
4. Put common mistakes, sign conventions, and subtle assumptions in a
   `warningbox`.
5. Use `\label` and `\cref`; never type equation or figure numbers manually.
6. Define reusable commands in `preamble.tex`, not inside individual chapters.
7. Keep `main.tex` focused on document organization rather than chapter text.
8. Compile the current subfile while writing; compile `main.tex` before review,
   export, or submission.
9. Run `latexmk -c` periodically to remove auxiliary files.
10. After major edits, inspect the table of contents, bibliography, headers,
    footers, and cross-references.

## 14. Recommended Working Routine

For each note-taking session:

1. Open the current chapter file.
2. Create the appropriate `\section` or `\subsection`.
3. Record the concepts, assumptions, and symbol definitions first.
4. Add the central equations and give them descriptive labels.
5. Add derivations, worked examples, diagrams, and source citations.
6. Record mistakes and easily confused points in a `warningbox`.
7. Summarize the most important results in a `summarybox`.
8. Compile the current chapter and inspect the result.
9. Compile `main.tex` periodically to verify the table of contents and global
   references.

With this workflow, the template works well for lecture notes, post-class
review, formula collections, problem-solving records, and a long-term physics
knowledge base.
