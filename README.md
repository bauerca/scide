# SciDE

In-browser development environment for web-format scientific articles.

## Goals

* Ease the creation of web-format scientific documents
* Facilitate fine-grained cross-referencing/sharing of scientific articles across the web
* Facilitate development of annotation/peer-reviewing tools

## Approach

Define a JSON data format for a scientific article comprising a set of generic
types.
Organization of the types will be defined by an
[article grammar](https://github.com/bauerca/scide#article-grammar).
Each type will need an editor and a viewer. Editors will capture
user input and translate that input into
the JSON data type (and the reverse; editors should translate JSON data to their UIs).
Viewers will turn JSON data into HTML (and vice versa?).

We should make use of existing tools whenever possible (e.g. markdown, MathJax).
Unfortunately, tools may skip the JSON data step (turning user input directly into
display), thereby hindering fine-grained cross-referencing/sharing.

## Article grammar

A scientific article will be defined according to the following grammar. The primary purpose
behind defining this grammar is to break an article into logical referenceable
components found in modern scientific publication (in my experience).
I use regex syntax to help define the grammar (`?`: 0 or 1, `*`: 0 or more, `+`: 1 or more,
`(...)`: grouping, `|`: or).

```
Article: Title Author+ Affiliation* Abstract? Section+ Bibliography?

Abstract: Inline+

Section: Title (Block | Section)+

Bibliography: Citation+

Block: Paragraph | Figure | Table | Grid | Equations

Paragraph: (Inline | Equations)+

Equations: (Equation EqnNum?)+

Grid: (Figure | Table | Grid | Row)+ Caption?

Row: Figure* Table* Grid*

Figure: Media FigNum Caption?

Table: Data TableNum Caption?

Caption: Inline+

Title: Word+

Inline: Word | InlineMath | Reference | Symbol

Media: Image | Movie | Canvas | SVG | ...
```

The undefined atomic types are thus:
`Image`, `Movie`, `Canvas`, `SVG`, `Word`, `InlineMath`, `Reference`,
`Symbol`, `Data`, `TableNum`, `FigNum`, `Equation`, `Citation`, `Author`,
`Affiliation`

## Types

Types omitted below are either self-explanatory or under active procrastination.

### Equation

This type is familiar to many scientists. It represents an important mathematical
statement, usually emphasized by being given the full page width for display. It is
produced in LaTeX by the `equation` environment; however, as opposed to the LaTeX
`equation`, this type says nothing about equation numbering.

### Equations

This type enumerates the Equation type to cover equation arrays and equation numbering.

### Block

This composite type has some display connotation built in. Standard practice would
give types in the Block group the full page width.

### Section

The definition of a Section in the article grammar is a recursive one. A section that
contains no subsections would match the definition, `Section: Title Block+`.

### Abstract

Should be (ideally) a single paragraph of text, inline-math, references, and symbols.
Maybe the definition should be: `Abstract: Paragraph+`?

### Grid

A method for spatially arranging and aligning elements side-by-side or on top of each other. 
Recursive use allows for "merged cell" arrangements.
