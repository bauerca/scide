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
components found in modern scientific publication (in our experience). The grammar tries to stay
layout agnostic; that is, the types describe only content and its organization, not the display
of the content.
Display is left to the definition and implementation of concrete
[subtypes](https://github.com/bauerca/scide#types) of the generic
types enumerated in the grammar.
Regex syntax is used (`?`: 0 or 1, `*`: 0 or more, `+`: 1 or more,
`(...)`: grouping, `|`: or).

```
Article: Title Author+ Affiliation* Abstract? Section+ Bibliography?

Abstract: Paragraph+

Section: Title (Paragraph | DisplayGroup | Section)+

Bibliography: Citation+

Theorem: Statement Proof

Statement: Paragraph+

Proof: Paragraph+

DisplayGroup: (Figure | Table | DisplayGroup)+ Caption?

Caption: Paragraph

Paragraph: Basic+

Title: (Word | Math | Symbol)+

Basic: Word | Math | Reference | Symbol
```

The undefined atomic types are thus:
`Word`, `Math`, `Reference`, `Symbol`, `Figure`, `Table`,
`Citation`, `Author`, `Affiliation`

## Types

In the following, each of the undefined generic types in the grammar has a section
comprising some possible concrete subtypes. 
Types omitted below are either self-explanatory or under active procrastination.

### Math

#### Equation

This subtype is familiar to many scientists. It represents an important mathematical
statement, usually emphasized by being given the full page width for display. It is
produced in LaTeX by the `equation` environment; however, unlike the LaTeX
`equation`, this type says nothing about equation numbering.

##### NumberedEquation

##### UnnumberedEquation

#### EquationArray

An array of equations!


### Section

The definition of a Section in the article grammar is a recursive one. A section that
contains no subsections would match the definition, `Section: Title Block+`.

### Abstract

Should be (ideally) a single paragraph of text, inline-math, references, and symbols.
Maybe the definition should be: `Abstract: Paragraph+`?

### Figure

### Table

### DisplayGroup

#### Row

A horizontally arranged group of Figures and/or Tables.

#### Grid

A method for spatially arranging and aligning elements side-by-side or on top of each other. 
Recursive use allows for "merged cell" arrangements.

#### Slideshow

A scrollable horizontal arrangement of Figures and/or Tables.
