# Preamble

Knowledge in BEL is expressed as BEL Statements. Generally, BEL Statements have
the form of a __subject__ - __predicate__ - __object__ triple, where the
subject is a BEL Term, the predicate is one of the BEL relationship types
(e.g., `__increases__`), and the object can be either a BEL Term or a BEL
Statement. A BEL Statement may also be comprised of a subject term only.

BEL Terms are composed of BEL Functions applied to concepts referenced using
Namespace identifiers. Each BEL Term represents either an abundance of a
biological entity, e.g., human AKT1 protein, or a process such as apoptosis.

BEL Annotations are applied to BEL Statements to optionally express additional
information about the statement itself such as the citation for the
publication reporting the observation, or the context in which the observation
was made (e.g., species, tissue, cell line).

## Identifiers

BEL is specifically designed to adopt external vocabularies and ontologies, and
represent life-science knowledge in the language and schema of the organization
collecting or using the knowledge. Thus, BEL Terms are defined by reference to
concepts in external vocabularies, which provide a set of well-known domain
values, such as the official human gene symbols provided by HGNC [http://www.genenames.org/](http://www.genenames.org/).
While we consider it good practice to define biological entities with respect
to well-defined domains such as public ontologies, no specific vocabulary is
essential to the use of BEL, and users are free to define and reference their
own vocabularies as needed.

BEL uses Namespaces to unambiguously reference concepts. The user associates a
Namespace prefix with an external vocabulary and uses the prefix to refer to
elements of the vocabulary. For example, if we associate the Namespace prefix
HGNC with the vocabulary of symbols managed by the HGNC committee, we can then
compose BEL Terms by referencing the HGNC Namespace prefix and any concept from
the HGNC namespace together with a relevant BEL Function, e.g., `proteinAbundance(HGNC:AKT1)`
or `rnaAbundance(HGNC:TNF)`.

### Namespaces Used in Examples

Namespaces are a reference to the specific vocabulary that a value used in a
BEL Term comes from. The examples in this documentation use the following set
of BEL Namespaces to reference external ontologies and vocabularies:

| **Namespace Abbreviation** | **Namespace Description**        |
| -------------------------- | -------------------------------- |
| EGID                       | Entrez Gene IDs                  |
| HGNC                       | HGNC human gene symbols          |
| MGI                        | MGI mouse gene symbols           |
| RGD                        | RGD rat gene symbols             |
| UNIPROT                    | SwissProt accession numbers      |
| MESH                       | Medical Subject Headings         |
| CHEBI                      | Chemicals of Biological Interest |
| GO                         | Gene Ontology                    |
| SCOMP                      | Selventa Named Complexes         |
| SFAM                       | Selventa Protein Families        |

## Terms

Two general categories of biological entities are represented as BEL Terms: **abundances** and **processes**.

### Abundances

Life science experiments often measure the abundance of a type of thing in a
given sample or set of samples. BEL Abundance Terms represent classes of
abundance, the abundances of specific types of things. Examples include the
__protein abundance of TP53__, the __RNA abundance of CCND1__, the __abundance
of the protein AKT1 phosphorylated at serine 21__, or the __abundance of the
complex of the proteins CCND1 and CDK4__.

### Processes

BEL Process Terms represent classes of complex phenomena taking place at the
level of the cell or the organism, such as the biological process of __cell
cycle__ or a disease process such as __Cardiomyopathy__. In other cases, BEL
Terms may represent classes of specific molecular activities, such as the
kinase activity of the AKT1 protein, or a specific chemical reaction like
conversion of superoxides to hydrogen peroxide and oxygen.

Measurable biological parameters such as __Blood Pressure__ or __Body
Temperature__ are represented as process BEL Terms. These BEL Terms denote
biological activities that, when measured, are reduced to an output parameter.

### Nested Statements

BEL Terms are denoted by expressions composed of a BEL Function and a list of
arguments. BEL v2.0 specifies a set of approximately 20 functions allowed in
term expressions.

The combination of a BEL function and its arguments fully specifies a BEL Term. The BEL Term expression `f(a)` denotes a BEL Term defined by function `f()` applied to an argument `a`. Wherever the same function is applied to the same arguments, the resulting BEL Term references the same biological entity.

The semantics of a BEL Term are determined by the function used in the term expression. For example, the function `proteinAbundance()` is defined such that any term expression using `proteinAbundance()` represents a class of abundance of protein. Many BEL functions take only single values as arguments, providing a structured method of using ontologies and vocabularies in BEL. For example, values in the HUGO Gene Nomenclature Committee (HGNC) vocabulary of official human gene symbols can be used to designate gene, RNA, and protein abundances. The function `proteinAbundance()` could then be applied to an HGNC gene symbol, __AKT1__ for example, to indicate the class of protein abundances produced by the AKT1 gene, producing the BEL Term `proteinAbundance(HGNC:AKT1)`.

### Statements

A BEL Statement represents an experimental observation, generally reported in
a scientific publication or unpublished experimental data. Generally, BEL
Statements express a causal or correlative relationship between two biological
entities. Because BEL Terms are functionally composed, a BEL Statement can
consist of a single BEL Term; this simple statement indicates that the
biological entity represented by the term has been observed.

#### Example BEL Statements

##### Subject Term Only

```
complex(p(HGNC:CCND1), p(HGNC:CDK4))
```

The abundance of a complex formed from protein abundances designated by __CCND1__ and __CDK4__ in the HGNC namespace. This is a subject term only statement, and indicates that the entity specified by the term has been observed.

##### Causal

```
p(HGNC:CCND1) => act(p(HGNC:CDK4))
```

The abundance of the protein designated by __CCND1__ in the HGNC namespace directly increases the activity of the abundance of the protein designated by __CDK4__ in the HGNC namespace.

##### Causal

```
p(HGNC:BCL2)-| bp(MESHPP:Apoptosis)
```

The abundance of the protein designated by __BCL2__ in the HGNC namespace decreases the biological process designated by __apoptosis__ in the MESHPP (phenomena and processes) namespace.

##### Nested Statement - Object Term is Statement

```
p(HGNC:GATA1) => ( act(p(HGNC:ZBTB16)) => r(HGNC:MPL) )
```

The abundance of the protein designated by __GATA1__ in the HGNC namespace directly increases the process in which the activity of the protein abundance designated by __ZBTB16__ in the HGNC namespace directly increases the abundance of RNA designated by __MPL__ in the HGNC namespace.

### Annotations

Each BEL Statement can optionally be annotated to express knowledge about the statement itself. Some important uses of annotations are to specify information about the:

*   biological system in which the observation represented by the statement was made
*   experimental methods used to demonstrate the observation
*   knowledge source on which the statement is based, such as the citation and specific text supporting the statement

Examples of annotations that could be associated with a BEL Statement are the:

*   PubMed ID specifying the publication in which the observation was reported,
*   Species, tissue, and cellular location in which the observations were made, and
*   Dosage, exposure and recovery time associated with the observation.

