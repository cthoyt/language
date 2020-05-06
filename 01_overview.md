# Preamble

BEL has three levels: terms, statements, and script. Terms represent
physical/abstract entities like genes, proteins, complexes, and phenotypes;
statements represent the relationships between terms; and scripts organize
groups of statements with their annotations such as the citation for the
publication reporting the observation, or the context in which the observation
was made (e.g., species, tissue, cell line). Script also contains
document-level metadata about the curator, references to external vocabularies,
and provenance.

The first part of this tutorial will focus on writing biological entities
as BEL terms, then how to combine them into relations. After, it will go over
using annotations and finally how to group them all together in BEL Script
with appropriate metadata.

Parts of this tutorial were copied/adapted from the BEL v2.0 tutorial at
http://openbel.org/language/version_2.0/bel_specification_version_2.0.html.
Many thanks to Natalie Catlett and William Hayes for their contributions to
and maintenance of that document.

## Identifiers

BEL uses [compact uniform resource locators](https://en.wikipedia.org/wiki/CURIE),
or CURIEs, to reference entries in ontologies and other external vocabularies.
A CURIE has two parts, a prefix and an identifier that are delimited by a
colon `:`. They can be used to reference entities in ontologies like the
Gene Ontology, vocabularies like the HGNC, and hierarchies like MeSH.
You've probably seen entities written this way before like in:

```
// CURIE for apopototic process in the Gene Ontology
go:0006915

// CURIE for MAPT in HGNC
hgnc:6893

// CURIE for Parkinson's disease in the NCIT
ncit:C26845

// CURIE for homo sapiens in the NCBI Taxonomy
taxonomy:9606
```

CURIEs are great because they can be resolved to their own resources using
services like https://identifiers.org. They're also easier to deal with than
PURLs becuase you don't have to understand all of the assumptions made by the
PURL community in modeling. However, CURIEs are hard to read, so BEL adopts
an OBO-like syntax for allowing CURIEs to be written with an optional label
like in:

```
go:0006915 ! "apopototic process"

hgnc:6893 ! MAPT

ncit:C26845 ! "Parkinson's disease"

taxonomy:9606 ! "Homo sapiens"
```

Therefore, anywhere in this tutorial that identifier is referenced,
it can take the following form:

```
prefix:identifier [! name]
```

BEL is also designed to be extensible to custom vocabularies and does not
necessarily rely on the existence of CURIEs. More information can be found
in the BEL Script section of this tutorial.

More information about how BEL uses CURIEs can be found in
[BEP-0008](http://bep.bel.bio/published/BEP-0008.html).
