# BEL Terms

Throughout this tutorial, most functions have both a long and short form.
For brevity, the long forms will be introduced, but thereafter the short
forms will be used.

Two general categories of biological entities are represented as BEL Terms:
**abundances** and **processes**.

### Physical Entities

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

The protein-coding gene [TMTC1](https://identifiers.org/hgnc:24099) can be
encoded in BEL like:

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

| prefix   | name | species   |
| -------- | ---- | --------- |
| [ncbigene](https://registry.identifiers.org/registry/ncbigene) | NCBI Entrez Gene | all       |
| [hgnc](https://registry.identifiers.org/registry/hgnc)     | HGNC | human     |
| [fb](https://registry.identifiers.org/registry/fb)  | FlyBase | drosophila melanogaster |
| [mgi](https://registry.identifiers.org/registry/mgi) | Mouse Genome Informatics | mouse     |
| [rgd](https://registry.identifiers.org/registry/rgd) | Rat Genome Database | rat       |
| [sgd](https://registry.identifiers.org/registry/sgd)      | Saccharomyces Genome Database | Saccharomyces cerevisiae (baker's yeast) |
| [pombase](https://registry.identifiers.org/registry/pombase)  | PomBase | Schizosaccharomyces pombe (fission yeast) |
| [wormbase](https://registry.identifiers.org/registry/wormbase)  | WormBase | C elegans (nematode) |
| [xenbase](https://registry.identifiers.org/registry/xenbase)  | Xenbase | xenopus (frogs) |
| [zfin](https://registry.identifiers.org/registry/zfin)     | Zebrafish Information Network | zebrafish |
| [dbsnp](https://registry.identifiers.org/registry/dbsnp)   | dbSNP | human |

Some nomenclatures are species-specific (e.g. HGNC covers human genes),
and some cover many species (e.g. NCBI Entrez Gene xocrse many.

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
// long form
g(hgnc:1884 ! CFTR, variant("c.1521_1523delCTT"))

// short form
g(hgnc:1884 ! CFTR, var("c.1521_1523delCTT"))
```

In general, any variant can be encoded like:

```
var("HGVS string")
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
over the HGNC gene symbol (when possible). The __c.__ within the HGVS string
indicates that the numbering is based on a coding DNA reference sequence.
The coding DNA reference sequence covers the part of the transcript that is
translated into protein; numbering starts at the A of the initiating ATG codon,
and ends at the last nucleotide of the translation stop codon.

The CFTR gene can be written with a RefSeq identifier as:

```bel
g(refseq:"NM_000492.3", var("c.1521_1523delCTT"))
```

##### Example - substitution



##### Example - double substitution

```bel
g(hgnc:2928 ! DMD, var("c.[145C>T;147C>G]")
```


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

| prefix     | name | species |
| ---------- | ---- | ------- |
| [ensembl](https://registry.identifiers.org/registry/ensembl) | Ensembl | all |
| [rnacentral](https://registry.identifiers.org/registry/rnacentral) | RNA Central | all |

#### Nt Recommended Nomenclatures
| prefix     | reason |
| ---------- | ------ |
| snornabase | not maintained |

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

##### Example - Protein substitution 1

```
p(hgnc:1884 ! CFTR, var("p.Gly576Ala"))
```

CFTR substitution variant Glycine 576 Alanine (HGVS __NP_000483.3:p.Gly576Ala__).
Because a specific position is referenced, a namespace value for a
non-ambiguous sequence like the [RefSeq](http://www.ncbi.nlm.nih.gov/refseq/about/)
ID in the lower example is preferred over the HGNC gene symbol. The __p.__
within the `var("")` expression indicates that the numbering is based on a
protein sequence.

```bel
p(refseq:"NP_000483.3", var("p.Gly576Ala"))
```

##### Example - Protein Substitution 2

This term represents the abundance of the human PIK3CA protein in which the
glutamic acid residue at position 545 has been substituted with a lysine.

```
// long form
p(HGNC:PIK3CA, variant("p.Glu545Lys"))

// short form
p(HGNC:PIK3CA, var("p.Glu545Lys"))
```

##### Example - Protein frameshift

```
p(hgnc:1884 ! CFTR, var("p.Thr1220Lysfs"))
```

CFTR frameshift variant __(__HGVS__ NP_000483.3:p.Thr1220Lysfs*7). __Because a
specific position is referenced, a namespace value for a non-ambiguous sequence
RefSeq ID in the lower example is preferred over the HGNC gene symbol. The
__p.__ within the `var("")` expression indicates that the numbering is based on
a protein reference sequence.

```bel
p(refseq:"NP_000483.3", var("p.Thr1220Lysfs"))
```

##### Example - Protein Truncation

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

The `proteinModification()` / `pmod()` function can be inside the `p()`
function to indicate modification of the specified protein. Multiple
modifications can be applied to the same protein abundance. For example,
the phosphorylated Tau protein would look like this:

```bel
// long form
p(uniprot:P10636, proteinModification(Phosphorylation))

// short form
p(uniprot:P10636, pmod(Ph))
```

The first argument of `pmod()` is the protein modification type.
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

However, any identifier with the form `prefix:identifier [! name]` can be used.

Optionally, the amino acid residue which is modified can be specified with its
three letter code:

```bel
p(uniprot:P10636, pmod(Ph, Ser))
```

The following amino acids are supported in BEL:

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

Optionally, the position which is modified can be specified

```bel
p(uniprot:P10636, pmod(Ph, Ser, 519))
```

More information about this phosphorylation can be found on
[PhosphoSitePlus](https://www.phosphosite.org/siteAction.action?id=2977).


Modified protein abundance term expressions have the general form:

```
p(prefix:identifier [! name], pmod(prefix:identifier [! name], <code>, <pos>))
```


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

##### Example - Hydroxylation

This term represents the abundance of human HIF1A protein
hydroxylated at asparagine 803.

```
// long form
p(hgnc:4910 ! HIF1A, proteinModification(Hy, Asn, 803))

// short form
p(hgnc:4910 ! HIF1A, pmod(Hy, Asn, 803))
```

##### Example - Phosphorylation

This term represents the phosphorylation of the human AKT protein family at an
unspecified amino acid residue.

```
p(fplx:AKT, pmod(Ph))
```

##### Example - Acetylation

This term represents the abundance of mouse RELA protein acetylated at lysine
315.

```
p(mgi:103290 ! Rela, pmod(Ac, Lys, 315))
```

##### Example - Glycosylation

This term represents the abundance of human SP1 protein glycosylated at an
unspecified amino acid residue.

```
p(hgnc:11205 ! SP1, pmod(Glyco))
```

##### Example - Methylation

This term represents the abundance of rat STAT1 protein methylated at an
unspecified arginine residue:

```
p(rgd:3771 ! STAT1, pmod(Me, Arg))
```

##### Ubiquitination

This term represents the abundance of human MYC protein ubiquitinated at an
unspecified lysine residue:

```
p(hgnc:7553 ! MYC, pmod(Ub, Lys))
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

Protein families may share a position that is post-translationally modified
or a variant. Both the `pmod()` and `var()` functions are allowed for protein
families, but use with care. For example, the entire AKT family shares the
Ser473 phosphorylation.

```bel
p(fplx:AKT, pmod(Ph, Ser, 473))
```

#### Recommended Nomenclatures

| prefix     | name   |
| ---------- | --------- |
| [fplx](https://registry.identifiers.org/registry/fplx) | FamPlex |
| [interpro](https://registry.identifiers.org/registry/interpro) | InterPro |
| signor | SIGNOR |
| pro | Protein Ontology |

#### Not Recommended Nomenclatures

| prefix     | reason   |
| ---------- | --------- |
| [ncit](https://registry.identifiers.org/registry/ncit) | no mappings available to other resources |
| [mesh](https://registry.identifiers.org/registry/mesh) | no mappings available to other resources |
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
| [prosite](https://registry.identifiers.org/registry/prosite) | ProSite |
| [interpro](https://registry.identifiers.org/registry/interpro) | InterPro |

### Populations

The `populationAbundance()` / `pop()` function allow for capturing Taxon and
Cell population level changes due to environment or treatments. For example,
a population of Streptococcus entericus can be encoded as:

```bel
// long form
populationAbundance(taxonomy:1123302 ! "Streptococcus entericus DSM 14446")

// short form
pop(taxonomy:1123302 ! "Streptococcus entericus DSM 14446")
```

Similarly, a population of white adipocytes can be encoded as:

```bel
pop(mesh:D052436 ! "Adipocytes, White")
```

Unlike the gene, RNA, and protein, there are no `var()` or modification
functions for `pop()`.

This function was added in BEL 2.1. The following are example use cases:

```bel
# Penicillin decreases the population of Streptococcus entericus
a(chebi:17334 ! penicillin) decreases pop(taxonomy:1123302 ! "Streptococcus entericus DSM 14446")

# Firmicutes bacteria increases obesity
pop(taxonomy:1239 ! Firmicutes) increases path(mesh:D009765 ! Obesity)

# A drug decreases the population of adipocytes
a(chebi:6801 ! metformin) decreases pop(mesh:D052436 ! "Adipocytes, White")

# P. falciparum invasion of RBCs increases malaria
complex(pop(taxonomy:5833 ! "Plasmodium falciparum"), pop(bto:0000424 ! erythrocyte)) increases path(doid:12365 ! malaria)

#S. typhimurium in complex with L-ficolin (an opsonin) enhances phagocytosis [PMID:8576206]
complex(pop(taxonomy:90371), p(hgnc:3624 ! FCN2)) increases bp(go:0006909 ! phagocytosis)
```

#### Recommended Nomenclatures

| prefix | name |
| ------ | ---- |
| [taxonomy](https://registry.identifiers.org/registry/taxonomy)  | NCBI Taxonomy |
| [bto](https://registry.identifiers.org/registry/BTO) | Brenda Tissue Ontology |
| [cl](https://registry.identifiers.org/registry/CTO) | Cell Type Ontology |
| clo | Cell Line Ontology | 
| [efo](https://registry.identifiers.org/registry/efo) | Experimental Factor Ontology | 

### Other Physical Entities

Small molecules, chemicals, and drugs can be represented with the
`abundance()` / `a()` function. For example, the drug Viagra can be
encoded as:

```bel
// long form
abundance(chebi:58987 ! "sildenafil citrate")

// short form
a(chebi:58987 ! "sildenafil citrate")
``` 

Classes of chemicals can be encoded with `a()` as well:

```bel
a(chebi:26523 ! "reactive oxygen species") 
```

Chemicals that are part of a class can be denoted using the `isA`
relationship, though these relationships can often be programatically
extracted from ontologies such as ChEBI:

```bel
a(chebi:16240 ! hydrogen peroxide) isA a(chebi:26523 ! "reactive oxygen species") 
a(chebi:25935 ! hydroperoxyl) isA a(chebi:26523 ! "reactive oxygen species") 
a(chebi:29191 ! hydroxyl) isA a(chebi:26523 ! "reactive oxygen species") 
...
```

In general, an abundance can be written like:

```bel
a(prefix:identifier [! name])
```

Abundances do not have `variant()` modifiers like genes, RNAs, and proteins.

#### Recommended Chemical Nomenclature

| prefix | name |
| ------ | ---- |
| [chebi](https://registry.identifiers.org/registry/chebi)  | Chemical Entities of Biological Interest |
| [drugbank](https://registry.identifiers.org/registry/drugbank) | DrugBank |
| [pubchem.compound](https://registry.identifiers.org/registry/pubchem.compound) | PubChem |
| [chembl.compound](https://registry.identifiers.org/registry/chembl.compound) | ChEMBL | 

#### Not Recommended Chemical Nomenclature

| prefix | name | reason |
| ------ | ---- | ------ |
| [mesh](https://registry.identifiers.org/registry/mesh) | Medical Subject Headings | no cross-references to other nomenclatures or structural information
| schem  | Selventa chemicals | not maintained

#### Other Entity Types

Other entity types can be encoded as `abundances()` that do not fit well into
the paradigm of protein, such as functional nucleic acids, cellular structures,
and small functional peptides.


```bel
// cellular structure
a(go:0022904 ! "respiratory electron transport chain")
```

```bel
// aggregates of proteins
a(conso:CONSO00358 ! TDP-43 oligomers)
```

### Complexes of Physical Entities

The `complexAbundance()` or `complex()` function can be used with either a
namespace value or with a list of abundance terms.

`complexAbundance(ns:v)` or `complex(ns:v)` denotes the abundance of the
molecular complex designated by the value `v` in the namespace `ns`. This form
is generally used to identify abundances of named complexes.

`complexAbundance(<abundance term list>)` denotes the abundance of the
molecular complex of members of the abundances denoted by
`<abundance term list>`, a list of abundance terms supplied as arguments.
The list is unordered, thus different orderings of the arguments should be
interpreted as the same term. Members of a molecular complex retain their
individual identities. The`complexAbundance()` function does not specify the
duration or stability of the interaction of the members of the complex.

The abundances of molecular complexes are represented using the
`complexAbundance()` function. This function can take either a list of
abundance terms or a value from a namespace of molecular complexes as
its argument.

Both BEL Terms represent the IkappaB kinase complex. The first by referencing
a named protein complex within the [GO namespace](http://geneontology.org/page/cellular-component-ontology-guidelines).

```
// long form
complexAbundance(go:0008385 ! "IkappaB kinase complex")

// short form
complex(go:0008385 ! "IkappaB kinase complex")
```

Define the enumerating the IkappaB kinase complex by composition of 
its member proteins: CHUK, IKBKB, and IKBKG.

```
// long form
complexAbundance(p(hgnc:1974 ! CHUK), p(hgn:c5960 ! IKBKB), p(hgnc:5961 ! IKBKG))

// short form
complex(p(hgnc:1974 ! CHUK), p(hgnc:5960 ! IKBKB), p(hgnc:5961 ! IKBKG))
```

Members of a complex can be annotated to it using the `partOf` like in

```bel
p(hgnc:1974 ! CHUK)  partOf complex(go:0008385 ! "IkappaB kinase complex")
p(hgnc:5960 ! IKBKB) partOf complex(go:0008385 ! "IkappaB kinase complex")
p(hgnc:5961 ! IKBKG) partOf complex(go:0008385 ! "IkappaB kinase complex")
```

However, resources like FamPlex usually provide these relationships that
can be automatically added.

##### Example - composed complex of proteins

```bel
complex(p(hgnc:3796 ! FOS), p(hgnc:6204 ! JUN))
```

##### Example - composed complex of protein and ligand

```bel
complex(a(chebi:132964 ! "fluazifop-P-butyl"), p(ec-code:6.4.1.2 ! "acetyl-CoA carboxylase"))
```

##### Example - composed complex of virus and cell

```bel
complex(pop(taxonomy:5833 ! "Plasmodium falciparum"), pop(bto:0000424 ! erythrocyte))
```

##### Example - a protein bound to gene

```bel
complex(p(hgnc:11364 ! STAT3), g(hgnc:10610 ! CCL11))
```

## Process Functions

The following BEL Functions represent classes of events or phenomena taking
place at the level of the cell or the organism which do not correspond to
molecular abundances, but instead to a biological process like angiogenesis
or a pathology like cancer.

### Biological Processes

Pathways, biological processes, and high-order molecular-level phenomena can
be represented with the `biologicalProcess()` / `bp()` function.

For example, the process of angiogenesis can be represented with:

```bel
// long form
biologicalProcess(go:0001525 ! angiogenesis)

// short form
bp(go:0001525 ! angiogenesis)
```

Likewise, the Signaling by Hippo pathway from Reactome can be represented with:

```bel
bp(reactome:R-HSA-2028269 ! "Signaling by Hippo")
```

There are no modifiers to the `bp()` function.

#### Recommended Nomenclatures

| prefix | name |
| ------ | ---- |
| [go](https://registry.identifiers.org/registry/go)  | Gene Ontology |
| [wikipathways](https://registry.identifiers.org/registry/wikipathways)  | WikiPathways |
| [reactome](https://registry.identifiers.org/registry/reactome)  | Reactome |

#### Not Recommended Nomenclature

| prefix | name | reason |
| ------ | ---- | ------ |
| [mesh](https://registry.identifiers.org/registry/mesh)   | Medical Subject Headings | no cross-references to other nomenclatures
| [ncit](https://registry.identifiers.org/registry/ncit)  | National Cancer Institute Thesaurus | few cross-references maintained
| [kegg.pathway](https://registry.identifiers.org/registry/kegg.pathway)  | Kyoto Encyclopedia of Genes and Genomes | data not easily accessible
| [pid.pathway](https://registry.identifiers.org/registry/pid.pathway)  | NCI Pathway Interaction Database | not maintained

If you need help mapping between pathway databases, see [ComPath](https://www.nature.com/articles/s41540-018-0078-8).

### Pathologies

Diseases, pathologies, phenotypes, psychiatric conditions, side effects,
and organism-level phenomena can be represented with the `pathology()` /
`path()` function. We're aware that "pathology" is not only inappropriate,
but also indelicate in some situations, so an update to the more general
term "phenotype" will come with the next backwards-incompatible language
update.

For now, disease pathologies like muscle hypotonia can be represented by:

```bel
// long form
pathology(mesh:D009123 ! "Muscle Hypotonia")

// short form
path(mesh:D009123 ! "Muscle Hypotonia")
```

In general, a pathology can be encoded like an abundance, population,
or biological process like:

```bel
path(prefix:identifier [! name])
```

#### Recommended Nomenclatures

| prefix | name |
| ------ | ---- |
| [doid](https://registry.identifiers.org/registry/doid)  | Disease Ontology |
| [efo](https://registry.identifiers.org/registry/efo)  | Experimental Factor Ontology |
| [hpo](https://registry.identifiers.org/registry/hpo)  | Human Phenotype Ontology |
| [mondo](https://registry.identifiers.org/registry/mondo)  | Monarch Disease Ontology |
| [mesh](https://registry.identifiers.org/registry/mesh) | Medical Subject Headings |

#### Not Recommended Nomenclatures

| prefix | name | reason |
| ------ | ---- | ------ |
| icd9  | ICD 9 | Not maintained |
| [icd](https://registry.identifiers.org/registry/icd)  | ICD 10 | Almost impossible to get data |
| icd11  | ICD 11 | You didn't even know there was a version 11, did you? |
| sdis  | Selventa Diseases | Not maintained |

## Reified Entities

### Composites

The `compositeAbundance()` / `composite()` is the BEL version of an
`AND` statement. Like `complex()`, you can put a list of physical entities
(or biological processes?) inside it to denote that two things have to be
present/active at the same time.

The `compositeAbundance(<abundance term list>)` function takes a list of
abundance terms. The `compositeAbundance()` or `composite()` function is used
to represent cases where multiple abundances synergize to produce an effect.
The list is unordered, thus different orderings of the arguments should be
interpreted as the same term. This function should not be used if any of the
abundances alone are reported to cause the effect. `compositeAbundance()` terms
should be used only as subjects of statements, not as objects.


For example, IL-6 and IL-23 synergistically induce Th17 differentiation as in:

```bel
composite(p(HGNC:IL6), complex(GO:"interleukin-23 complex")) increases bp(GO:"T-helper 17 cell differentiation")
```

#### Example

```
// long form
compositeAbundance(proteinAbundance(HGNC:TGFB1), proteinAbundance(HGNC:IL6))

// short form
 composite(p(HGNC:TGFB1), p(HGNC:IL6))
```


### Reactions

The `reaction()` / `rxn()` function denotes the transformations from a set of
abundances to another set of abundances. It is written like:

```
reaction(reactants(<abundance term list1>), products(<abundance term list2>))
```

where the frequency or abundance of events in which members of the abundances
in `<abundance term list1>` (the reactants) are transformed into members of the
abundances in `<abundance term list2>` (the products).

For example, the reaction in which superoxides are dismutated into oxygen and
hydrogen peroxide can be represented as:

```bel
rxn(reactants(a(CHEBI:superoxide)), products(a(CHEBI:"hydrogen peroxide"), a(CHEBI: "oxygen")))
```

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

Fusions are when two gene, RNA, or proteins are combine. The `fusion()` /
`fus()` functions  can be used in place of an identifier within a gene, RNA,
or protein abundance function to represent a hybrid gene, or gene product
formed from two previously separate genes.

The `fusion()` expressions take the general form:

```bel
fus(prefix:identifier [! name], "range5'", prefix:identifier [! name], "range3'")
```

where the first `prefix:identifier` is for the 5-prime fusion partner,
`range5'` is the sequence coordinates of the 5-prime partner, the second
`prefix:identifier` is for the 3-prime partner, and `range3'` is the
sequence coordinates for the 3' partner.  Ranges need to be in quotes.

For example, the fusion between the RNA of TMPRSS2 and ERG can
be encoded as:

```
r(fus(hgnc:11876 ! TMPRSS2, "r.1_79", hgnc:3446 ! ERG, "r.312_5034"))
```

The __r.__ designation in the range fields indicates that the numbering
uses the RNA sequence as the reference. RNA sequence numbering starts at the
transcription initiation site.  You use __c.___ for `g()` fusions and __p.___
for `p()` fusions.  These __r.__, __c.__, and __p.__ designations come from
HGVS.

If the breakpoints are unspecified for the 5-prime range or the
3-prime range, the `"?"` can be used like in:

```
r(fus(hgnc:11876 ! TMPRSS2, "?", hgnc:3446 ! ERG, "?"))
```

###### Example - Fusion of Proteins

The abundances of fusion proteins resulting from chromosomal translocation 
mutations can be specified by using the `fusion()` or `fus()` function within
a protein abundance term.

```
// long form
p(fusion(hgnc:1014 ! BCR, "p.1_426", hgnc:6192 ! JAK2, "p.812_1132"))

// short form
p(fus(hgnc:1014 ! BCR, "p.1_426", hgnc:6192 ! JAK2, "p.812_1132"))
```

This term represents the abundance of a fusion protein of the 5' partner BCR
and 3' partner JAK2, with the breakpoint for BCR at amino acid 426 and JAK2
at 812. _p._ indicates that the protein sequence is used for the range
coordinates provided. If the breakpoint is not specified, the fusion protein
abundance can be represented as:

##### Example - Unspecified Fusion of Proteins

```
p(fus(HGNC:1014 ! BCR, "?", HGNC:6192 ! JAK2, "?"))
```
