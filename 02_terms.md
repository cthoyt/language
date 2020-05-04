# BEL Terms

Throughout this tutorial, most functions have both a long and short form.
For brevity, the long forms will be introduced, but thereafter the short
forms will be used.

Two general categories of biological entities are represented as BEL Terms:
**abundances** and **processes**.

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

## Physical Entities

The following BEL Functions represent classes of abundances of specific types
of biological entities like RNAs, proteins, post-translationally modified
proteins, and small molecules. Biological experiments frequently involve the
manipulation and measurement of entities in samples. These BEL functions
specify the type of entity referred to by a namespace value. For example,
`geneAbundance(hgnc:391 ! AKT1)`, `rnaAbundance(hgnc:391 ! AKT1)`, and `proteinAbundance(hgnc:391 ! AKT1)`,
represent the abundances of the AKT1 gene, RNA, and protein, respectively.

### Genes

The protein-coding gene [transmembrane O-mannosyltransferase targeting
cadherins 1](https://identifiers.org/hgnc:24099) can be encoded in BEL
like:

```bel
// long form
geneAbundance(hgnc:24099 ! TMTC1)

// short form
g(hgnc:24099 ! TMTC1)
```

In general, any gene can be encoded using the form:

```
g(prefix:identifier [! name])
```

You can encode the genomic relationship between a gene and the RNA(s) to which it
is is transcribed like:

```bel
g(hgnc:24099 ! TMTC1) transcribedTo r(ensembl:ENST00000539277.6 ! TMTC1-203)
g(hgnc:24099 ! TMTC1) transcribedTo r(ensembl:ENST00000256062.9 ! TMTC1-201)
g(hgnc:24099 ! TMTC1) transcribedTo r(ensembl:ENST00000551659.5 ! TMTC1-206)
g(hgnc:24099 ! TMTC1) transcribedTo r(ensembl:ENST00000552618.5 ! TMTC1-207)
g(hgnc:24099 ! TMTC1) transcribedTo r(ensembl:ENST00000550354.1 ! TMTC1-205)
g(hgnc:24099 ! TMTC1) transcribedTo r(ensembl:ENST00000319685.12 ! TMTC1-202)
g(hgnc:24099 ! TMTC1) transcribedTo r(ensembl:ENST00000552925.5 ! TMTC1-208)
g(hgnc:24099 ! TMTC1) transcribedTo r(ensembl:ENST00000546582.1 ! TMTC1-204)
g(hgnc:24099 ! TMTC1) transcribedTo r(ensembl:ENST00000553189.5 ! TMTC1-209)
```

You can also encode the binding event between a gene and its transcription
factor(s) like:

```bel
complex(g(hgnc:24099 ! TMTC1), p(hgnc:3819 ! FOXO1))
```

More information about complexes can be found [below](#complexes-of-physical-entities).

#### Recommended Nomenclatures

The following nomenclatures are recommended for genes:

| prefix   | species   |
| -------- | --------- |
| ncbigene | all       |
| hgnc     | human     |
| flybase  | drosophila melanogaster |
| mgi      | mouse     |
| rgd      | rat       |
| sgd      | Saccharomyces cerevisiae (baker's yeast) |
| pombase  | Schizosaccharomyces pombe (fission yeast) |
| wormbase  | C elegans (nematode) |
| xenbase  | xenopus (frogs) |
| zfin     | zebrafish |
| dbsnp    | human |

The orthology between two genes from different species can be written
like:

```bel
g(hgnc:14064 ! HDAC6) orthologousTo g(mgi:1333752 ! Hdac6)
```

#### Genetic Variants

Variants like substitutions, deletions, insertions, and anything that
can be represented with the [HGVS nomenclature](http://varnomen.hgvs.org/) can
be added to a gene following the identifier using the `variant()` / `var()`
function.

For example, the protein-coding gene [CFTR (hgnc:1884)](https://identifiers.org/hgnc:1884)
when missing phenylalanine 508 (__ΔF508__) misfolds and leads to cystic
fibrosis. This genetic variant can be written in BEL as:

```bel
g(hgnc:1884 ! CFTR, var("c.1521_1523delCTT"))
```

This variant has been listed by the dbSNP database as [rs113993960](https://identifiers.org/dbsnp:rs113993960).
It can be directly encoded in BEL as:

```bel
g(dbsnp:rs113993960)
```

The equivalence between these two BEL terms is different than simple
identifier equivalence, so it can be encoded with the
`equivalentTo` / `eq` relationship like:

```bel
g(dbsnp:rs113993960) eq g(hgnc:1884 ! CFTR, var("c.1521_1523delCTT"))
```

Because a specific position is referenced, a namespace value for a
non-ambiguous sequence like the RefSeq ID in the lower example is preferred
over the HGNC gene symbol. The __c.__ within the `var("")` expression indicates
that the numbering is based on a coding DNA reference sequence. The coding DNA
reference sequence covers the part of the transcript that is translated into
protein; numbering starts at the A of the initiating ATG codon, and ends at the
last nucleotide of the translation stop codon.

```bel
g(refseq:"NM_000492.3", var("c.1521_1523delCTT"))
```

##### Example - substitution

TODO

##### Example - insertion

TODO

#### Genetic Modifications

Modifications to the physical genomic sequence can be represented
in BEL using the `geneModification()` / `gmod()` function following
the identifier in a `g()` function.

For example, the methylation of the NDUFB6 gene causes a decline in its
expression in muscles (pubmed:17948130). This can be represented with:

```bel
// long form
g(hgnc:7701 ! NDUFB6, geneModification(Methylation))

// short form
g(hgnc:7701 ! NDUFB6, gmod(Me))
```

In general, the `geneModification()` / `gmod()` function takes the
following form:

```
gmod(prefix:identifier [! name])
```

However, there are several built-in gene modifications in BEL that
can be referenced without a CURIE from the following table:

| Short | Long |
| ----- | ---- |
| Me     | Methylation |
| ADPRib | ADP-ribosylation |

These can be written as:

```
// long form
gmod(Methylation)
gmod(ADP-ribosylation)

// short form
gmod(Me)
gmod(ADPRib)
```

While protein modifications have high quality sources like
PSI-MOD, genenetic/genomic modifications do not. One source
that may grow over time is the [DNA modification (go:0006304)](https://identifiers.org/GO:0006304)
branch in the GO biological process namespace. Even though the semantics
are not exactly correct, this still may prove a useful source for gene
modification nomenclature.

The previous example can be written with [DNA methylation (go:0006306)](https://identifiers.org/GO:0006306)
instead of the BEL default name `Me` like:

```bel
g(hgnc:7701 ! NDUFB6, gmod(go:0006306 ! "DNA methylation"))
```

### RNAs

The long non-coding RNA [Homo sapiens MAPT antisense RNA 1 (MAPT-AS1)](https://rnacentral.org/rna/URS000075DB76/9606)
can be encoded in BEL with:

```bel
// long form
rnaAbundance(rnacentral:URS000075DB76 ! MAPT-AS1)

// short form
r(rnacentral:URS000075DB76 ! MAPT-AS1)
```

In general, any RNA type can be encoded using the form, with the exception of
[miRNAs](#micro-rnas), which have their own function:

```
r(prefix:identifier [! name])
```

Like in the previous example, it can be shown from which gene an RNA is
transcribed like:

```bel
g(hgnc:43738 ! ) transcribedTo r(rnacentral:URS000075DB76 ! MAPT-AS1)
```

It can also be shown that an RNA is translated to a protein like:

```bel
g(hgnc:6893 ! MAPT) transcribedTo r(ensembl:ENST00000344290.9 ! MAPT-204)
r(ensembl:ENST00000344290.9 ! MAPT-204) translatedTo p(uniprot:P10636 ! TAU_HUMAN)
```

There are situations when the exact transcript for a given gene is not known,
in which case it is common to refer to the RNA transcript using the gene's
identifier like:

```bel
g(hgnc:6893 ! MAPT) transcribedTo r(hgnc:6893 ! MAPT)
```

A functional RNA might appear in a relationship in which it causes the regulation
of a gene's expression [ref](https://identifiers.com/pubmed:27336847):

```bel
r(rnacentral:URS000075DB76 ! MAPT-AS1) decreases r(hgnc:6893 ! MAPT)
```

#### RNA Variants

Like genes, RNAs can be described with variants using the `var()`
function.

Looking back at the CFTR gene that causes __ΔF508__, the RNA variant would look like:

```bel
r(hgnc:1884 ! CFTR, var("r.1653_1655delcuu"))
```

The RefSeq identifier could be used to more explicitly state the sequence to which
the HGVS string is referring.

```bel
r(refseq:"NM_000492.3", var("r.1653_1655delcuu"))
```

The coding gene and protein from this variant could be connected with the
following `transcribedTo` and `translatedTo` relations:

```bel
g(hgnc:1884 ! CFTR, var("c.1521_1523delCTT")) transcribedTo r(hgnc:1884 ! CFTR, var("r.1653_1655delcuu"))
r(hgnc:1884 ! CFTR, var("r.1653_1655delcuu")) translatedTo p(hgnc:1884 ! CFTR, var("p.Phe508del"))
```
 
Or using RefSeq:

```bel

g(refseq:"NM_000492.3", var("c.1521_1523delCTT")) transcribedTo r(refseq:"NM_000492.3", var("r.1653_1655delcuu"))
r(refseq:"NM_000492.3", var("r.1653_1655delcuu")) translatedTo p(refseq:"NP_000483.3", var("p.Phe508del"))
```

Because a specific position is referenced, a namespace value for a
non-ambiguous sequence like the [RefSeq](http://www.ncbi.nlm.nih.gov/refseq/about/)
ID in the lower example is preferred over the HGNC gene symbol. The __r.__
within the `var("")` expression indicates that the numbering is based on an RNA
reference sequence. The RNA reference sequence covers the entire transcript
except for the poly A-tail; numbering starts at the transcription initiation
site and ends at the transcription termination site.

#### Recommended Nomenclatures

The following nomenclatures are recommended for RNAs:

| prefix     | species   |
| ---------- | --------- |
| ensembl | all |
| rnacentral | all |
| snornabase | all |

### Micro-RNAs

Micro-RNAs (or, miRNAs) are functional RNAs that regulate gene expression.
Unlike other non-protein coding RNAs (e.g. lncRNAs, sRNAs, etc.), miRNAs
have their own function in BEL. It works exactly the same way as the `r()`
function.

```bel
// long form
microRNAAbundance(mirbase:MI0000139 ! mmu-mir-1a-1)

// short form
m(mirbase:MI0000139 ! mmu-mir-1a-1)
```

Like in the previous two examples, it can be shown from which gene an miRNA is
transcribed like:

```
g(mgi:2676869 ! Mir1a-1) transcribedTo m(mirbase:MI0000139 ! mmu-mir-1a-1)
```

miRNAs have the unique quality that they have a pre-mature and a mature variant.
BEL doesn't have an ontological relationship for this yet, but it's very important
to know the related *3'* and *5'* processed mature miRNAs, which are conveniently
listed in miRBase.

```bel
m(mirbase:MI0000139 ! mmu-mir-1a-1) -- m(mirbase.mature:MIMAT0016979 ! mmu-miR-1a-1-5p)
m(mirbase:MI0000139 ! mmu-mir-1a-1) -- m(mirbase.mature:MIMAT0000123 ! mmu-miR-1a-3p)
```

#### Recommended Nomenclatures

The following nomenclatures are recommended for miRNAs:

| prefix     | species   |
| ---------- | --------- |
| ensembl | all |
| rnacentral | all |
| mirbase | all |
| mirbase.mature | all |

### Proteins

`proteinAbundance(ns:v)` or `p(ns:v)` denotes the abundance of the protein
designated by the value +v+ in the namespace +ns+, where +v+ references a
gene or a named protein family.

```
p(hgnc:391 ! AKT1)
```

#### Protein Variants

The `variant("<expression>")` or `var("<expression>")` function can be used as
an argument within a `geneAbundance()`, `rnaAbundance()`, `microRNAAbundance()`,
or `proteinAbundance()` to indicate a sequence variant of the specified
abundance. The `var("")` function takes [HGVS](http://www.hgvs.org/mutnomen/)
variant description expression, e.g., for a substitution, insertion, or
deletion variant. Multiple `var("")` arguments may be applied to an abundance
term.

The following BEL functions are special functions that can be used only as
an argument within an abundance function. These functions modify the abundance
to specify sequence variations (gene, RNA, microRNA, protein),
post-translational modifications (protein), fragment resulting from proteolytic
processing (protein), or cellular location (most abundance types).

As a follow up to the previous examples showing the gene-level and RNA-level
modificaitons that lead to the __ΔF508__ variant of CFTR, the following
is how to express it using the `var()` function inside a protein with
HGVS nomenclature:

```bel
p(hgnc:1884 ! CFTR, var("p.Phe508del"))
```

And mor specifically with RefSeq:

```bel
p(refseq:"NP_000483.3", var("p.Phe508del"))
```

CFTR ΔF508 variant (HGVS __NP_000483.3:p.Phe508del__). Because a specific
position is referenced, a namespace value for a non-ambiguous sequence like
the RefSeq ID in the lower example is preferred over the HGNC gene symbol.
The __p.__ within the `var("")` expression indicates that the numbering is
based on a protein reference sequence.

##### Example - Protein reference allele

```
p(hgnc:1884 ! CFTR, var("="))
```

This is different than `p(hgnc:1884 ! CFTR)`, the root protein abundance, which
includes all variants.

##### Example - Protein unspecified variant

```
p(hgnc:1884 ! CFTR, var("?"))
```

##### Example - Protein substitution

```
p(hgnc:1884 ! CFTR, var("p.Gly576Ala"))
p(refseq:"NP_000483.3", var("p.Gly576Ala"))
```

CFTR substitution variant Glycine 576 Alanine (HGVS __NP_000483.3:p.Gly576Ala__).
Because a specific position is referenced, a namespace value for a
non-ambiguous sequence like the [RefSeq](http://www.ncbi.nlm.nih.gov/refseq/about/)
ID in the lower example is preferred over the HGNC gene symbol. The __p.__
within the `var("")` expression indicates that the numbering is based on a
protein sequence.


##### Example - Protein frameshift

```
p(hgnc:1884 ! CFTR, var("p.Thr1220Lysfs"))
p(refseq:"NP_000483.3", var("p.Thr1220Lysfs"))
```

CFTR frameshift variant __(__HGVS__ NP_000483.3:p.Thr1220Lysfs*7). __Because a
specific position is referenced, a namespace value for a non-ambiguous sequence
RefSeq ID in the lower example is preferred over the HGNC gene symbol. The
__p.__ within the `var("")` expression indicates that the numbering is based on
a protein reference sequence.


##### Example - Protein Substitution

```
// long form
p(HGNC:PIK3CA, variant("p.Glu545Lys"))

// short form
p(HGNC:PIK3CA, var("p.Glu545Lys"))
```

This term represents the abundance of the human PIK3CA protein in which the
glutamic acid residue at position 545 has been substituted with a lysine.

###### Example - Protein Truncation

The abundances of proteins that are truncated by the introduction of a stop
codon can be specified by using the `variant("")` or `var("")` function within
a protein abundance term.

```
// long form
p(HGNC:ABCA1, variant("p.Arg1851*"))

// short form
p(HGNC:ABCA1, var("p.Arg1851*"))
```

#### Protein Fragments

The `fragment()` or `frag()` function can be used within a `proteinAbundance()`
term to specify a protein fragment, e.g., a product of proteolytic cleavage.
Protein fragment expressions take the general form:

p(ns:v, frag(<range>, <descriptor>))

where `<range>` (required) is an amino acid range, and `<descriptor>`
(optional) is any additional distinguishing information like fragment size
or name.

For these examples, __HGNC:YFG__ is ‘your favorite gene’. For the first four
examples, only the `<range>` argument is used. The last examples include use of
the optional `<descriptor>`.

```
// fragment with known start/stop
p(HGNC:YFG, frag("5_20"))

// amino-terminal fragment of unknown length
p(HGNC:YFG, frag("1_?"))

// carboxyl-terminal fragment of unknown length
p(HGNC:YFG, frag("?_*"))

// fragment with unknown start/stop
p(HGNC:YFG, frag("?"))

// fragment with unknown start/stop and a descriptor
p(HGNC:YFG, frag("?", "55kD"))
```

#### Protein Modifications

The `proteinModification()` or `pmod()` function can be used only as an
argument within a `p()` function to indicate modification of the
specified protein. Multiple modifications can be applied to the same protein
abundance. Modified protein abundance term expressions have the general form:

```
p(ns:protein_value, pmod(ns:type_value, <code>, <pos>))
```

`type_value` (required) is a namespace value for the type of modification,
**`<code>`** (optional) is a Supported One- and Three-letter Amino Acid
Codes, single-letter or three-letter code for one of the twenty standard
amino acids, and `<pos>` (optional) is the position at which the modification
occurs based on the reference sequence for the protein. If **`<pos>`** is
omitted, then the position of the modification is unspecified. If both
**`<code>`** and **`<pos>`** are omitted, then the residue and position of the
modification are unspecified. 


The `proteinModification()` or `pmod()` function is used within a protein
abundance to specify post-translational modifications. Types of
post-translational modification are specified by a namespace value; the
default BEL namespace provides many commonly used modification types.
Abundances of modified proteins take the form `p(ns:v, pmod(ns:type_value, <code>, <pos>))`,
where `<type>` (required) is the kind of modification, `<code>` (optional)
is the one- or three- letter Supported One- and Three-letter Amino Acid
Codes, amino acid code for the modified residue, and `<pos>` (optional) is
the sequence position of the modification.

The default BEL namespace includes commonly used protein modification types
from the following table:

| Label     | Synonym                                                                             |
| --------- | ----------------------------------------------------------------------------------- |
| Ac        | acetylation                                                                         |
| ADPRib    | ADP-ribosylation, ADP-rybosylation, adenosine diphosphoribosyl                      |
| Farn      | farnesylation                                                                       |
| Gerger    | geranylgeranylation                                                                 |
| Glyco     | glycosylation                                                                       |
| Hy        | hydroxylation                                                                       |
| ISG       | ISGylation, ISG15-protein conjugation                                               |
| Me        | methylation                                                                         |
| Me1       | monomethylation, mono-methylation                                                   |
| Me2       | dimethylation, di-methylation                                                       |
| Me3       | trimethylation, tri-methylation                                                     |
| Myr       | myristoylation                                                                      |
| Nedd      | neddylation                                                                         |
| NGlyco    | N-linked glycosylation                                                              |
| NO        | Nitrosylation                                                                       |
| OGlyco    | O-linked glycosylation                                                              |
| Palm      | palmitoylation                                                                      |
| Ph        | phosphorylation                                                                     |
| Sulf      | sulfation, sulphation, sulfur addition, sulphur addition, sulfonation, sulphonation |
| Sumo      | SUMOylation                                                                         |
| Ub        | ubiquitination, ubiquitinylation, ubiquitylation                                    |
| UbK48     | Lysine 48-linked polyubiquitination                                                 |
| UbK63     | Lysine 63-linked polyubiquitination                                                 |
| UbMono    | monoubiquitination                                                                  |
| UbPoly    | polyubiquitination                                                                  |

The following one or three-letter amino acid codes are supported:

| **Amino Acid** | **1-Letter Code** | **3-Letter Code** |
| -------------- | ----------------- | ----------------- |
| Alanine       | A | Ala |
| Arginine      | R | Arg |
| Asparagine    | N | Asn |
| Aspartic Acid | D | Asp |
| Cysteine      | C | Cys |
| Glutamic Acid | E | Glu |
| Glutamine     | Q | Gln |
| Glycine       | G | Gly |
| Histidine     | H | His |
| Isoleucine    | I | Ile |
| Leucine       | L | Leu |
| Lysine        | K | Lys |
| Methionine    | M | Met |
| Phenylalanine | F | Phe |
| Proline       | P | Pro |
| Serine        | S | Ser |
| Threonine     | T | Thr |
| Tryptophan    | W | Trp |
| Tyrosine      | Y | Tyr |
| Valine        | V | Val |

##### Example - AKT1 phosphorylated at Serine 473

Default BEL namespace and 3-letter amino acid code:

```
p(hgnc:391 ! AKT1, pmod(Ph, Ser, 473))
```

[PSI-MOD](http://psidev.cvs.sourceforge.net/viewvc/psidev/psi/mod/data/PSI-MOD.obo)
namespace and 3-letter amino acid code:

```
p(hgnc:391 ! AKT1, pmod(MOD:PhosRes, Ser, 473))
```

##### Example -  MAPK1 phosphorylated at both Threonine 185 and Tyrosine 187

default BEL namespace and 3-letter amino acid code:

```
p(hgnc:6871 ! MAPK1, pmod(Ph, Thr, 185), pmod(Ph, Tyr, 187))
```

##### Example - Palmitoylated HRAS

HRAS palmitoylated at an unspecified residue. Default BEL namespace:

```
p(hgnc:5173 ! HRAS, pmod(Palm))
```

##### Hydroxylation

This term represents the abundance of human HIF1A protein
hydroxylated at asparagine 803.

```
// long form
p(hgnc:4910 ! HIF1A, proteinModification(Hy, Asn, 803))

// short form
p(hgnc:4910 ! HIF1A, pmod(Hy, Asn, 803))
```

##### Phosphorylation

This term represents the phosphorylation of the human AKT protein family at an
unspecified amino acid residue.

```
p(SFAM:"AKT Family", pmod(Ph))
```

##### Acetylation

This term represents the abundance of mouse RELA protein acetylated at lysine
315.

```
p(MGI:Rela, pmod(Ac, Lys, 315))
```

##### Glycosylation

This term represents the abundance of human SP1 protein glycosylated at an
unspecified amino acid residue.

```
p(HGNC:SP1, pmod(Glyco))
```

##### Methylation

This term represents the abundance of rat STAT1 protein methylated at an
unspecified arginine residue:

```
p(RGD:STAT1, pmod(Me, Arg))
```

##### Ubiquitination

This term represents the abundance of human MYC protein ubiquitinated at an
unspecified lysine residue:

```
p(HGNC:MYC, pmod(Ub, Lys))
```

#### Recommended Nomenclatures


### Protein Families

Families of proteins can be expressed using the `p()` function as well.

For example, the Protein Kinase B (a.k.a., AKT family) can be
expressed using any of these several namespaces:

```bel
p(fplx:AKT)

// see: https://signor.uniroma2.it/relation_result.php?id=SIGNOR-PF24&organism=human
p(signor:SIGNOR-PF24 ! AKT)

// see: https://identifiers.org/ncit:C41625
// NCIT not recommended because they do not provide mappings
p(ncit:C41625 ! "Protein Kinase B")

// The selventa namespace is not maintained. Please do not use.
p(sfam:F0014 ! "AKT Family")
```

While many resources (such as Bio2BEL repositories) can be used to enrich
BEL graphs with protein family's members, they can be encoded directly
with the `isA` relationship as in:

```bel
p(hgnc:391 ! AKT1) isA p(fplx:AKT)
p(hgnc:392 ! AKT2) isA p(fplx:AKT)
p(hgnc:393 ! AKT3) isA p(fplx:AKT)
```

Warning: some nomenclatures, such as InterPro, are not organism specific. 
This results in protein families that contain proteins that are orthologs.


#### Recommended Nomenclatures

| prefix     | name   |
| ---------- | --------- |
| fplx | FamPlex |
| interpro | InterPro |
| signor | SIGNOR |
| pro | Protein Ontology |

#### Not Recommended Nomenclatures

| prefix     | reason   |
| ---------- | --------- |
| ncit | no mappings available to other resources |
| mesh | no mappings available to other resources |
| sfam | Selventa families are not maintained. Please upgrade to FamPlex (fplx) |

A community maintained list of equivalences between protein families is maintained
at https://github.com/sorgerlab/famplex/blob/master/equivalences.csv. This is a useful
tool for moving away from deprecated namespaces.

### Protein Domains

Protein domains and functional are considered similarly to protein families in BEL
because it's useful to think of groups of proteins with the same domain having
the same (putative) molecular function (think GO annotations).

The [Zinc finger, AN1-type](https://identifiers.org/interpro:IPR000058) can be 
expressed using the `p()` tag as in:

```bel
// See also: https://identifiers.org/interpro:IPR000058
p(interpro:IPR000058 ! "Zinc finger, AN1-type")
```

The fact that the [AN1-type zinc finger protein 2B](https://identifiers.org/uniprot:B4DEN4) has the
[Zinc finger, AN1-type](https://identifiers.org/interpro:IPR000058) domain can be encoded with the
following:

```bel
p(uniprot:B4DEN4 ! "AN1-type zinc finger protein 2B") isA p(interpro:IPR000058 ! "Zinc finger, AN1-type")
```

In general, these kinds of relationships can be automatically extracted from InterPro and shouldn't
be manually curated.

#### Recommended Nomenclatures

| prefix     | name   |
| ---------- | --------- |
| prosite | ProSite |
| interpro | InterPro |

### Populations

Cells and Species

### Other Physical Entities

`abundance(ns:v)` or `a(ns:v)` denotes the abundance of the entity designated
by the value `v` in the namespace `ns`. abundance is a general abundance term
that can be used for chemicals or other molecules not defined by a more
specific abundance function. Gene, RNA, protein, and microRNA abundances should
be represented using the appropriate specific abundance function.

#### Example - drugs

#### Examples - small molecule and chemical

```
a(CHEBI:"oxygen atom")
a(CHEBI:thapsigargin)
```

#### Example - cellular structure

#### Example - cells

### Complexes of Physical Entities

The `complexAbundance()` or `complex()` function can be used with either a
namespace value or with a list of abundance terms.

`complexAbundance(ns:v)` or `complex(ns:v)` denotes the abundance of the
molecular complex designated by the value `v` in the namespace `ns`. This form
is generally used to identify abundances of named complexes.

#### Example - named complex

```
complex(SCOMP:"AP-1 Complex")
```

`complexAbundance(<abundance term list>)` denotes the abundance of the
molecular complex of members of the abundances denoted by
`<abundance term list>`, a list of abundance terms supplied as arguments.
The list is unordered, thus different orderings of the arguments should be
interpreted as the same term. Members of a molecular complex retain their
individual identities. The`complexAbundance()` function does not specify the
duration or stability of the interaction of the members of the complex.

#### Example - composed complex of proteins

```
complex(p(HGNC:FOS), p(HGNC:JUN))
```

#### Example - composed complex of protein and ligand

#### Example - composed complex of virus and receptor

TODO

#### Example - composed complex of two cell types

#### Example - Binding Interaction

The `complexAbundance()` function can be used to specify molecular interactions
between abundances. This function can take either a list of abundances that
define a molecular complex or a namespace value that represents a molecular
complex (e.g., many GO Cellular Component values) as an argument. These
examples demonstrate the use of the `complexAbundance()` function to represent
protein-protein, protein-chemical, and protein-DNA interactions.

* Protein – protein interactions
* Protein – DNA interactions
* Protein – small molecule interactions


#### Example - protein-protein interaction as BEL statement

This statement represents that MTOR and AKT1S1 proteins physically interact.
Note that this statement has only an object term and no subject term and
relationship.

```
SET Citation = {"PubMed", "17277771"}
SET Support = "Here, we identify PRAS40 (proline-rich Akt/PKB substrate 40 kDa) as a novel mTOR binding partner"
// disambiguation PRAS40 = HGNC AKT1S1
complex(p(hgnc:391 ! AKT1S1), p(hgnc:3942 ! MTOR))
```

#### Example - protein-protein interaction as Statement object

Here, a protein-protein interaction is the object of a BEL Statement. This
statement expresses that the MTOR and STAT3 proteins associate and that
increases in the protein abundance of BMP4 can increase the abundance of
the complex comprised of MTOR and STAT3.

```
SET Citation = {"PubMed", "12796477"}
SET Support = "Upon BMP4 treatment, the serine-threonine kinase FKBP12/rapamycin-associated protein (FRAP), mammalian target of rapamycin (mTOR), associates with Stat3 and facilitates STAT activation."
p(hgnc:1071 ! BMP4) -> complex(p(hgnc:3942 ! MTOR), p(hgnc:11364 ! STAT3))
```

###### Protein – DNA interactions

#### Example - transcription factor protein binding to DNA

This statement expresses that STAT3 protein binds to the CCL11 gene DNA,
and that this association is increased by IL17A.

```
SET Citation = {"PubMed", "19265112"}
SET Support ="IL-17A induced at 1 h a marked enrichment of STAT3- associated CCL11 promoter DNA"
p(hgnc:5981 ! IL17A) -> complex(p(hgnc:11364 ! STAT3), g(hgnc:10610 ! CCL11))
```

###### Protein – small molecule interactions

#### Example - protein binding to a small molecule

This statement represents that PIP3 binds AKT1 protein.

```
SET Citation = {"PubMed", "15987444"}
SET Support = "After PIP3 binding, Akt1 is activated"
// disambiguation PIP3 = CHEBI 1-phosphatidyl-1D-myo-inositol 3,4,5-trisphosphate
complex(a(CHEBI:"1-phosphatidyl-1D-myo-inositol 3,4,5-trisphosphate"), p(hgnc:391 ! AKT1))
```

## Process Functions

The following BEL Functions represent classes of events or phenomena taking place at the level of the cell or the organism which do not correspond to molecular abundances, but instead to a biological process like angiogenesis or a pathology like cancer.

#### Biological Processes

`biologicalProcess(ns:v)` or `bp(ns:v)` denotes the process or population of events designated by the value +v+ in the namespace +ns+.

### Examples

```
bp(GO:"cell cycle arrest")
bp(GO:angiogenesis)
```

#### Pathologies

`pathology(ns:v)` or `path(ns:v)` denotes the disease or pathology process
designated by the value +v+ in the namespace +ns+. The `pathology()` function
is included to facilitate the distinction of pathologies from other biological
processes because of their importance in many potential applications in the
life sciences.

*Note*, though it is named pathology, this function is also used for phenotypes,
psychiatric conditions, side effects, and other higher order, organism-level
phenomena. 

### Examples

```
pathology(MESH:"Pulmonary Disease, Chronic Obstructive")
pathology(MESH:adenocarcinoma)
```

##### Biological Processes and Pathologies Term Examples

Biological phenomena that occur at the level of the cell or the organism are considered processes. These terms are represented by values from namespaces like GO and MeSH.

* Biological Processes
* Diseases and Pathologies

###### Biological Processes

Cellular senescence can be represented by:

###### Long Form

```
// long form
biologicalProcess(GO:"cellular senescence")

// short form
bp(GO:"cellular senescence")
```

###### Diseases and Pathologies

Disease pathologies like muscle hypotonia can be represented by:
```
// long form
pathology(MESH:"Muscle Hypotonia")

// short form
path(MESH:"Muscle Hypotonia")
```

## Reified Entities

### Composites

The `compositeAbundance(<abundance term list>)` function takes a list of
abundance terms. The `compositeAbundance()` or `composite()` function is used
to represent cases where multiple abundances synergize to produce an effect.
The list is unordered, thus different orderings of the arguments should be
interpreted as the same term. This function should not be used if any of the
abundances alone are reported to cause the effect. `compositeAbundance()` terms
should be used only as subjects of statements, not as objects.

### Example - BEL Statement with compositeAbundance term

```
composite(p(HGNC:IL6), complex(GO:"interleukin-23 complex")) increases bp(GO:"T-helper 17 cell differentiation")
```

In the above example, IL-6 and IL-23 synergistically induce Th17 differentiation.

### Reactions

`reaction(reactants(<abundance term list1>), products(<abundance term list2>))`
denotes the frequency or abundance of events in which members of the abundances
in `<abundance term list1>` (the reactants) are transformed into members of the
abundances in `<abundance term list2>` (the products).


### Example

The reaction in which superoxides are dismutated into oxygen and hydrogen peroxide can be represented as:

```
rxn(reactants(a(CHEBI:superoxide)),products(a(CHEBI:"hydrogen peroxide"), a(CHEBI: "oxygen")))
```

###### Reactions

The `reaction()` or `rxn()` function expresses the transformation of products into reactants, each defined by a list of abundances.

#### Example

This BEL Term represents the reaction in which the reactants
phosphoenolpyruvate and ADP are converted into pyruvate and ATP.

```
// long form
reaction(reactants(abundance(CHEBI:phosphoenolpyruvate), abundance(CHEBI:ADP)), products(abundance(CHEBI:pyruvate), abundance(CHEBI:ATP)))

// short form
rxn(reactants(a(CHEBI:phophoenolpyruvate), a(CHEBI:ADP)), products(a(CHEBI:pyruvate), a(CHEBI:ATP)))
```

### Fusions

`fusion()` or `fus()` expressions can be used in place of a namespace value within a gene, RNA, or protein abundance function to represent a hybrid gene, or gene product formed from two previously separate genes. `fusion()` expressions take the general form:

 fus(ns5':v5', "range5'", ns3':v3', "range3'")

where `ns5':v5'` is a namespace and value for the 5' fusion partner, `range5'` is the sequence coordinates of the 5' partner, `ns3':v3'` is a namespace and value for the 3' partner, and `range3'` is the sequence coordinates for the 3' partner.  Ranges need to be in quotes.

### Example

###### RNA abundance of fusion with known breakpoints

r(fus(HGNC:TMPRSS2, "r.1_79", HGNC:ERG, "r.312_5034"))

The __r.__ designation in the range fields indicates that the numbering uses the RNA sequence as the reference. RNA sequence numbering starts at the transcription initiation site.  You use __c.___ for g() fusions and __p.___ for p() fusions.  These __r.__, __c.__, and __p.__ designations come from http://www.hgvs.org[HGVS variation description] convention.

###### RNA abundance of fusion with unspecified breakpoints

 r(fus(HGNC:TMPRSS2, "?", HGNC:ERG, "?"))


###### Fusion Proteins

The abundances of fusion proteins resulting from chromosomal translocation mutations can be specified by using the `fusion()` or `fus()` function within a protein abundance term.

#### Example

```
// long form
p(fusion(HGNC:BCR, "p.1_426", HGNC:JAK2, "p.812_1132"))

// short form
p(fus(HGNC:BCR, "p.1_426", HGNC:JAK2, "p.812_1132"))
```

This term represents the abundance of a fusion protein of the 5' partner BCR and 3' partner JAK2, with the breakpoint for BCR at amino acid 426 and JAK2 at 812. _p._ indicates that the protein sequence is used for the range coordinates provided. If the breakpoint is not specified, the fusion protein abundance can be represented as:

```
p(fus(HGNC:BCR, "?", HGNC:JAK2, "?"))
```

The `fusion()` function can also be used within `geneAbundance` and `rnaAbundance` terms to represent genes and RNAs modified by fusion mutations.

## Examples

Measurable entities like genes, RNAs, proteins, and small molecules are represented as abundances in BEL. BEL Terms for abundances have the general form `a(ns:v)`, where `a` is an abundance function, `ns` is `a` namespace reference and `v` is a value from the namespace vocabulary. See Namespaces Used in Examples.

* Chemicals and Small Molecules
* Genes, RNAs, microRNAs, and Proteins
* Protein families
* Complexes
* Composite abundances

###### Chemicals and Small Molecules

The general abundance function `abundance()` is used to represent abundances
of chemicals, small molecules, and any other entities that cannot be
represented by a more specific abundance function.

#### Examples

###### Long Form

```
// long forms
abundance(CHEBI:"nitrogen atom")
abundance(CHEBI:"prostaglandin J2")

// short forms
a(CHEBI:"nitrogen atom")
a(CHEBI:"prostaglandin J2")
```

These BEL Terms represent the abundance of the entities specified by
_nitrogen atom_ and by _prostaglandin J2_ in the CHEBI namespace.

###### Genes, RNAs, and Proteins

The abundance functions `geneAbundance()`, `rnaAbundance()`, and
`proteinAbundance()` are used with namespace values like HGNC human gene
symbols, EntrezGene IDs, SwissProt accession numbers to designate the type of
molecule represented.

#### Examples

Abundances of the gene, RNA, and protein encoded by the human AKT1 gene are
represented as:

```
// long forms
geneAbundance(hgnc:391 ! AKT1)
rnaAbundance(hgnc:391 ! AKT1)
proteinAbundance(hgnc:391 ! AKT1)

// short forms
g(hgnc:391 ! AKT1)
r(hgnc:391 ! AKT1)
p(hgnc:391 ! AKT1)
```

These BEL Terms represent the gene, RNA, and protein abundances of the entity
specified by _AKT1_ in the HGNC namespace. Equivalent terms can be constructed
using a corresponding value from a different namespace. For example, the
abundance of the human AKT1 RNA can also be represented by referencing the
EntrezGene ID or SwissProt accession namespaces:

```
r(NCBIGENE:207)
r(UNIPROT:P31749)
```

###### microRNAs

The abundance function `microRNAAbundance()` is used to represent the fully processed, active form of a microRNA. The specific abundance functions allow distinct representations of the gene, RNA, and microRNA abundances for a given namespace value.

#### Example

These BEL Terms represent the abundances of the gene, RNA, and processed microRNA, respectively, for the entity specified by _Mir21_ in the MGI mouse gene symbol namespace.

```
// long forms
geneAbundance(MGI:Mir21)
rnaAbundance(MGI:Mir21)
microRNAAbundance(MGI:Mir21)

// short forms
g(MGI:Mir21)
r(MGI:Mir21)
m(MGI:Mir21)
```

###### Complexes

The abundances of molecular complexes are represented using the `complexAbundance()` function. This function can take either a list of abundance terms or a value from a namespace of molecular complexes as its argument.

#### Example

Both BEL Terms represent the IkappaB kinase complex. The first by referencing
a named protein complex within the [GO namespace](http://geneontology.org/page/cellular-component-ontology-guidelines).

```
// long form
complexAbundance(GO:"IkappaB kinase complex")

// short form
complex(GO:"IkappaB kinase complex")
```

Define the enumerating the IkappaB kinase complex by composition of 
its member proteins: CHUK, IKBKB, and IKBKG.

```
// long form
complexAbundance(proteinAbundance(HGNC:CHUK), proteinAbundance(HGNC:IKBKB), proteinAbundance(HGNC:IKBKG))

// short form
complex(p(HGNC:CHUK), p(HGNC:IKBKB), p(HGNC:IKBKG))
```

###### Composite abundances

Multiple abundance terms can be represented together as the subject of a BEL Statement by using the `compositeAbundance()` function. This function takes a list of abundances as its argument and is used when the individual abundances do not act alone, but rather synergize to produce an effect.

#### Example

This term represents the combined abundances of TGFB1 and IL6 proteins.

```
// long form
compositeAbundance(proteinAbundance(HGNC:TGFB1), proteinAbundance(HGNC:IL6))

// short form
 composite(p(HGNC:TGFB1), p(HGNC:IL6))
```
