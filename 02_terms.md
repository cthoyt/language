# BEL Terms

## Physical Entities

The following BEL Functions represent classes of abundances of specific types of biological entities like RNAs, proteins, post-translationally modified proteins, and small molecules. Biological experiments frequently involve the manipulation and measurement of entities in samples. These BEL functions specify the type of entity referred to by a namespace value. For example, `geneAbundance(HGNC:AKT1)`, `rnaAbundance(HGNC:AKT1)`, and `proteinAbundance(HGNC:AKT1)`, represent the abundances of the AKT1 gene, RNA, and protein, respectively.

#### Genes

`geneAbundance(ns:v)` or `g(ns:v)` denotes the abundance of the gene designated by the value v in the namespace ns. `geneAbundance()` terms are used to represent the DNA encoding the specified gene. `geneAbundance()` is considered decreased in the case of a homozygous or heterozygous gene deletion, and increased in the case of a DNA amplification mutation. Events in which a protein binds to the promoter of a gene can be represented using the `geneAbundance()` function.

**Example - promoter binding event represented using geneAbundance**

complex\(p\(HGNC:TP53\), g\(HGNC:CDKN1A\)\)

In the above example, the p53 protein binds the CDKN1A gene.

#### RNAs

`rnaAbundance(ns:v)` or `r(ns:v)` denotes the abundance of the RNA designated by the value v in the namespace +ns+, where +v+ references a gene. This function refers to all RNA designated by +ns:v+, regardless of splicing, editing, or polyadenylation stage.

**Example - RNA abundance**

```text
r(HGNC:AKT1)
```

#### Micro-RNAs

`microRNAAbundance(ns:v)` or `m(ns:v)` denotes the abundance of the processed, functional microRNA designated by the value +v+ in the namespace +ns+.

**Example - microRNA abundance**

```text
m(HGNC:MIR21)
```

#### Proteins

`proteinAbundance(ns:v)` or `p(ns:v)` denotes the abundance of the protein designated by the value +v+ in the namespace +ns+, where +v+ references a gene or a named protein family.

**Examples - protein abundances**

```text
p(HGNC:AKT1)
p(SFAM:"AKT Family")
```

#### Populations

Cells and Species

#### Other Physical Entities

`abundance(ns:v)` or `a(ns:v)` denotes the abundance of the entity designated by the value `v` in the namespace `ns`. abundance is a general abundance term that can be used for chemicals or other molecules not defined by a more specific abundance function. Gene, RNA, protein, and microRNA abundances should be represented using the appropriate specific abundance function.

**Examples - small molecule and chemical**

```text
a(CHEBI:"oxygen atom")
a(CHEBI:thapsigargin)
```

**Example - cells**

#### Complexes of Physical Entities

The `complexAbundance()` or `complex()` function can be used with either a namespace value or with a list of abundance terms.

`complexAbundance(ns:v)` or `complex(ns:v)` denotes the abundance of the molecular complex designated by the value `v` in the namespace `ns`. This form is generally used to identify abundances of named complexes.

**Example - named complex**

```text
complex(SCOMP:"AP-1 Complex")
```

`complexAbundance(<abundance term list>)` denotes the abundance of the molecular complex of members of the abundances denoted by `<abundance term list>`, a list of abundance terms supplied as arguments. The list is unordered, thus different orderings of the arguments should be interpreted as the same term. Members of a molecular complex retain their individual identities. The `complexAbundance()` function does not specify the duration or stability of the interaction of the members of the complex.

**Example - composed complex of proteins**

```text
complex(p(HGNC:FOS), p(HGNC:JUN))
```

**Example - composed complex of protein and ligand**

**Example - composed complex of virus and receptor**

TODO

**Example - composed complex of two cell types**

**Binding Interaction Term Examples**

The `<<XcomplexA, complexAbundance()>>` function can be used to specify molecular interactions between abundances. This function can take either a list of abundances that define a molecular complex or a namespace value that represents a molecular complex \(e.g., many GO Cellular Component values\) as an argument. These examples demonstrate the use of the `complexAbundance()` function to represent protein-protein, protein-chemical, and protein-DNA interactions.

* &lt;&gt;
* &lt;&gt;
* &lt;&gt;

**Protein – protein interactions**

**Example - protein-protein interaction as BEL statement**

This statement represents that MTOR and AKT1S1 proteins physically interact. Note that this statement has only an object term and no subject term and relationship.

**Long Form**

SET Citation = {"PubMed", "Nat Cell Biol 2007 Mar 9\(3\) 316-23", "17277771"}

SET Support = "Here, we identify PRAS40 \(proline-rich Akt/PKB substrate 40 kDa\) as a novel mTOR binding partner"

```text
// disambiguation PRAS40 = HGNC AKT1S1
complexAbundance(proteinAbundance(HGNC:AKT1S1), proteinAbundance(HGNC:MTOR))
```

**Short Form**

complex\(p\(HGNC:AKT1S1\), p\(HGNC:MTOR\)\)

**Example - protein-protein interaction as Statement object**

Here, a protein-protein interaction is the object of a BEL Statement.This statement expresses that the MTOR and STAT3 proteins associate and that increases in the protein abundance of BMP4 can increase the abundance of the complex comprised of MTOR and STAT3.

**Long Form**

SET Citation = {"PubMed", "J Cell Biol. 2003 Jun 9;161\(5\):911-21.", "12796477"}

SET Support ="Upon BMP4 treatment, the serine-threonine kinase FKBP12/rapamycin-associated protein \(FRAP\), mammalian target of rapamycin \(mTOR\), associates with Stat3 and facilitates STAT activation."

proteinAbundance\(HGNC:BMP4\) increases complexAbundance\(proteinAbundance\(HGNC:MTOR\), proteinAbundance\(HGNC:STAT3\)\)

**Short Form**

p\(HGNC:BMP4\) -&gt; complex\(p\(HGNC:MTOR\), p\(HGNC:STAT3\)\)

**Protein – DNA interactions**

**Example - transcription factor protein binding to DNA**

This statement expresses that STAT3 protein binds to the CCL11 gene DNA, and that this association is increased by IL17A.

**Long Form**

SET Citation = {"PubMed", "19265112"}

SET Support ="IL-17A induced at 1 h a marked enrichment of STAT3- associated CCL11 promoter DNA"

proteinAbundance\(HGNC:IL17A\) increases  complexAbundance\(proteinAbundance\(HGNC:STAT3\), geneAbundance\(HGNC:CCL11\)\)

**Short Form**

p\(HGNC:IL17A\) -&gt; complex\(p\(HGNC:STAT3\), g\(HGNC:CCL11\)\)

**Protein – small molecule interactions**

**Example - protein binding to a small molecule**

This statement represents that PIP3 binds AKT1 protein.

**Long Form**

SET Citation = {"PubMed", "Breast Cancer Res 2005 7\(4\) R394-401", "15987444"}

SET Support ="After PIP3 binding, Akt1 is activated"

```text
// disambiguation PIP3 = CHEBI 1-phosphatidyl-1D-myo-inositol 3,4,5-trisphosphate
complexAbundance(abundance(CHEBI:"1-phosphatidyl-1D-myo-inositol 3,4,5-trisphosphate"), proteinAbundance(HGNC:AKT1))
```

**Short Form**

```text
complex(a(CHEBI:"1-phosphatidyl-1D-myo-inositol 3,4,5-trisphosphate"), p(HGNC:AKT1))
```

## Modifiers of Physical Entities

The following BEL functions are special functions that can be used only as an argument within an abundance function. These functions modify the abundance to specify sequence variations \(gene, RNA, microRNA, protein\), post-translational modifications \(protein\), fragment resulting from proteolytic processing \(protein\), or cellular location \(most abundance types\).

#### Protein Modifications

**proteinModification\(\),  pmod\(\)**

The `proteinModification()` or `pmod()` function can be used only as an argument within a `proteinAbundance()` function to indicate modification of the specified protein. Multiple modifications can be applied to the same protein abundance. Modified protein abundance term expressions have the general form:

```text
p(ns:protein_value, pmod(ns:type_value, <code>, <pos>))
```

`type_value` \(required\) is a namespace value for the type of modification, **`<code>`** \(optional\) is a &lt;&gt; for one of the twenty standard amino acids, and `<pos>` \(optional\) is the position at which the modification occurs based on the reference sequence for the protein. If **`<pos>`** is omitted, then the position of the modification is unspecified. If both **`<code>`** and **`<pos>`** are omitted, then the residue and position of the modification are unspecified. The default BEL namespace includes commonly used protein modification types.

**Examples**

**AKT1 phosphorylated at Serine 473**

default BEL namespace and 1-letter amino acid code:

```text
p(HGNC:AKT1, pmod(Ph, S, 473))
```

default BEL namespace and 3-letter amino acid code:

```text
p(HGNC:AKT1, pmod(Ph, Ser, 473))
```

[PSI-MOD](http://psidev.cvs.sourceforge.net/viewvc/psidev/psi/mod/data/PSI-MOD.obo) namespace and 3-letter amino acid code:

```text
p(HGNC:AKT1, pmod(MOD:PhosRes, Ser, 473))
```

**MAPK1 phosphorylated at both Threonine 185 and Tyrosine 187**

default BEL namespace and 3-letter amino acid code:

```text
p(HGNC:MAPK1, pmod(Ph, Thr, 185), pmod(Ph, Tyr, 187))
```

**Palmitoylated HRAS**

HRAS palmitoylated at an unspecified residue. Default BEL namespace:

```text
p(HGNC:HRAS, pmod(Palm))
```

**Modification Types Provided in Default BEL Namespace**

Additional modification types can be requested as needed, or an external vocabulary can be used. Like other BEL namespace values, these modification types can be equivalenced to values in other vocabularies.

| Label | Synonym |
| :--- | :--- |
| Ac | acetylation |
| ADPRib | ADP-ribosylation, ADP-rybosylation, adenosine diphosphoribosyl |
| Farn | farnesylation |
| Gerger | geranylgeranylation |
| Glyco | glycosylation |
| Hy | hydroxylation |
| ISG | ISGylation, ISG15-protein conjugation |
| Me | methylation |
| Me1 | monomethylation, mono-methylation |
| Me2 | dimethylation, di-methylation |
| Me3 | trimethylation, tri-methylation |
| Myr | myristoylation |
| Nedd | neddylation |
| NGlyco | N-linked glycosylation |
| NO | Nitrosylation |
| OGlyco | O-linked glycosylation |
| Palm | palmitoylation |
| Ph | phosphorylation |
| Sulf | sulfation, sulphation, sulfur addition, sulphur addition, sulfonation, sulphonation |
| Sumo | SUMOylation |
| Ub | ubiquitination, ubiquitinylation, ubiquitylation |
| UbK48 | Lysine 48-linked polyubiquitination |
| UbK63 | Lysine 63-linked polyubiquitination |
| UbMono | monoubiquitination |
| UbPoly | polyubiquitination |

**Supported One- and Three-letter Amino Acid Codes**

| **Amino Acid** | **1-Letter Code** | **3-Letter Code** |
| :--- | :--- | :--- |
| Alanine | A | Ala |
| Arginine | R | Arg |
| Asparagine | N | Asn |
| Aspartic Acid | D | Asp |
| Cysteine | C | Cys |
| Glutamic Acid | E | Glu |
| Glutamine | Q | Gln |
| Glycine | G | Gly |
| Histidine | H | His |
| Isoleucine | I | Ile |
| Leucine | L | Leu |
| Lysine | K | Lys |
| Methionine | M | Met |
| Phenylalanine | F | Phe |
| Proline | P | Pro |
| Serine | S | Ser |
| Threonine | T | Thr |
| Tryptophan | W | Trp |
| Tyrosine | Y | Tyr |
| Valine | V | Val |

**Post-Translationally Modified Protein Term Examples**

The `proteinModification()` or `pmod()` function is used within a protein abundance to specify post-translational modifications. Types of post-translational modification are specified by a namespace value; the default BEL namespace provides many commonly used modification types. Abundances of modified proteins take the form `p(ns:v, pmod(ns:type_value, <code>, <pos>))`, where `<type>` \(required\) is the kind of modification, `<code>` \(optional\) is the one- or three- letter &lt;&gt; for the modified residue, and `<pos>` \(optional\) is the sequence position of the modification.

* &lt;&gt;
* &lt;&gt;
* &lt;&gt;
* &lt;&gt;
* &lt;&gt;
* &lt;&gt;

**Hydroxylation**

This term represents the abundance of human HIF1A protein hydroxylated at asparagine 803.

**Long Form**

proteinAbundance\(HGNC:HIF1A, proteinModification\(Hy, Asn, 803\)\)

**Short Form**

p\(HGNC:HIF1A, pmod\(Hy, N, 803\)\)

**Phosphorylation**

This term represents the phosphorylation of the human AKT protein family at an unspecified amino acid residue.

p\(SFAM:"AKT Family", pmod\(Ph\)\)

**Acetylation**

This term represents the abundance of mouse RELA protein acetylated at lysine 315.

p\(MGI:Rela, pmod\(Ac, Lys, 315\)\)

**Glycosylation**

This term represents the abundance of human SP1 protein glycosylated at an unspecified amino acid residue.

p\(HGNC:SP1, pmod\(Glyco\)\)

**Methylation**

This term represents the abundance of rat STAT1 protein methylated at an unspecified arginine residue:

p\(RGD:STAT1, pmod\(Me, Arg\)\)

**Ubiquitination**

This term represents the abundance of human MYC protein ubiquitinated at an unspecified lysine residue:

p\(HGNC:MYC, pmod\(Ub, Lys\)\)

#### Variants

**variant\(""\), var\(""\)**

The `variant("<expression>")` or `var("<expression>")` function can be used as an argument within a `geneAbundance()`, `rnaAbundance()`, `microRNAAbundance()`, or `proteinAbundance()` to indicate a sequence variant of the specified abundance. The `var("")` function takes [HGVS](http://www.hgvs.org/mutnomen/) variant description expression, e.g., for a substitution, insertion, or deletion variant. Multiple `var("")` arguments may be applied to an abundance term.

**Protein examples**

**reference allele**

```text
p(HGNC:CFTR, var("="))
```

This is different than `p(HGNC:CFTR)`, the root protein abundance, which includes all variants.

**unspecified variant**

```text
p(HGNC:CFTR, var("?"))
```

**substitution**

```text
p(HGNC:CFTR, var("p.Gly576Ala"))
p(REF:"NP_000483.3", var("p.Gly576Ala"))
```

CFTR substitution variant Glycine 576 Alanine \(HGVS **NP\_000483.3:p.Gly576Ala**\). Because a specific position is referenced, a namespace value for a non-ambiguous sequence like the [RefSeq](http://www.ncbi.nlm.nih.gov/refseq/about/) ID in the lower example is preferred over the HGNC gene symbol. The **p.** within the `var("")` expression indicates that the numbering is based on a protein sequence.

**deletion**

```text
p(HGNC:CFTR, var("p.Phe508del"))
p(REF:"NP_000483.3", var("p.Phe508del"))
```

CFTR ΔF508 variant \(HGVS **NP\_000483.3:p.Phe508del**\). Because a specific position is referenced, a namespace value for a non-ambiguous sequence like the [http://www.ncbi.nlm.nih.gov/refseq/about/\[RefSeq](http://www.ncbi.nlm.nih.gov/refseq/about/[RefSeq)\] ID in the lower example is preferred over the HGNC gene symbol. The **p.** within the `var("")` expression indicates that the numbering is based on a protein reference sequence.

**frameshift**

```text
p(HGNC:CFTR, var("p.Thr1220Lysfs"))
p(REF:"NP_000483.3", var("p.Thr1220Lysfs"))
```

CFTR frameshift variant **\(**HGVS **NP\_000483.3:p.Thr1220Lysfs\*7\).** Because a specific position is referenced, a namespace value for a non-ambiguous sequence like the [http://www.ncbi.nlm.nih.gov/refseq/about/\[RefSeq](http://www.ncbi.nlm.nih.gov/refseq/about/[RefSeq)\] ID in the lower example is preferred over the HGNC gene symbol. The **p.** within the `var("")` expression indicates that the numbering is based on a protein reference sequence.

**Variant \(Mutant\) Protein Examples**

The abundances of mutated and variant proteins can be represented in BEL using the abundance modifier function `variant("")` and the other function `fusion()`.

* &lt;&gt;
* &lt;&gt;
* &lt;&gt;

**Amino Acid Substitutions**

The abundances of proteins with amino acid sequence variations, such as those resulting from missense mutations or polymorphisms can be specified by using the `variant("")` or `var("")` function within a protein abundance term.

**Example**

**Long Form**

proteinAbundance\(HGNC:PIK3CA, variant\("p.Glu545Lys"\)\)

**Short Form**

p\(HGNC:PIK3CA, var\("p.Glu545Lys"\)\)

This term represents the abundance of the human PIK3CA protein in which the glutamic acid residue at position 545 has been substituted with a lysine.

**Truncated Proteins**

The abundances of proteins that are truncated by the introduction of a stop codon can be specified by using the `variant("")` or `var("")` function within a protein abundance term.

**Example**

**Long Form**

proteinAbundance\(HGNC:ABCA1, variant\("p.Arg1851\*"\)\)

**Short Form**

p\(HGNC:ABCA1, var\("p.Arg1851\*"\)\)

This term represents the abundance of human ABCA1 protein that has been truncated by substitution of Arginine 1851 with a stop codon.

**DNA \(gene\) examples**

These are all representations of CFTR **ΔF508**.

**SNP**

```text
g(SNP:rs113993960, var("delCTT"))
```

**chromosome**

```text
g(REF:"NC_000007.13", var("g.117199646_117199648delCTT"))
```

**gene - coding DNA reference sequence**

```text
g(HGNC:CFTR, var("c.1521_1523delCTT"))
g(REF:"NM_000492.3", var("c.1521_1523delCTT"))
```

Because a specific position is referenced, a namespace value for a non-ambiguous sequence like the [http://www.ncbi.nlm.nih.gov/refseq/about/\[RefSeq](http://www.ncbi.nlm.nih.gov/refseq/about/[RefSeq)\] ID in the lower example is preferred over the HGNC gene symbol. The **c.** within the `var("")` expression indicates that the numbering is based on a coding DNA reference sequence.The coding DNA reference sequence covers the part of the transcript that is translated into protein; numbering starts at the A of the initiating ATG codon, and ends at the last nucleotide of the translation stop codon.

**RNA examples**

These are all representations of CFTR **ΔF508**.

**coding reference sequence**

```text
r(HGNC:CFTR, var("c.1521_1523delCTT"))
r(REF:"NM_000492.3", var("c.1521_1523delCTT"))
```

Because a specific position is referenced, a namespace value for a non-ambiguous sequence like the [http://www.ncbi.nlm.nih.gov/refseq/about/\[RefSeq](http://www.ncbi.nlm.nih.gov/refseq/about/[RefSeq)\] ID in the lower example is preferred over the HGNC gene symbol. The **c.** within the `var("")` expression indicates that the numbering is based on a coding DNA reference sequence. The coding DNA reference sequence covers the part of the transcript that is translated into protein; numbering starts at the A of the initiating ATG codon, and ends at the last nucleotide of the translation stop codon.

**RNA reference sequence**

```text
r(HGNC:CFTR, var("r.1653_1655delcuu"))
r(REF:"NM_000492.3", var("r.1653_1655delcuu"))
```

Because a specific position is referenced, a namespace value for a non-ambiguous sequence like the [http://www.ncbi.nlm.nih.gov/refseq/about/\[RefSeq](http://www.ncbi.nlm.nih.gov/refseq/about/[RefSeq)\] ID in the lower example is preferred over the HGNC gene symbol. The **r.** within the `var("")` expression indicates that the numbering is based on an RNA reference sequence. The RNA reference sequence covers the entire transcript except for the poly A-tail; numbering starts at the transcription initiation site and ends at the transcription termination site.

#### Proteolytic fragments

**fragment\(""\), frag\(""\)**

The `fragment()` or `frag()` function can be used within a `proteinAbundance()` term to specify a protein fragment, e.g., a product of proteolytic cleavage. Protein fragment expressions take the general form:

p\(ns:v, frag\(, \)\)

where `<range>` \(required\) is an amino acid range, and `<descriptor>` \(optional\) is any additional distinguishing information like fragment size or name.

**Examples**

For these examples, **HGNC:YFG** is ‘your favorite gene’. For the first four examples, only the `<range>` argument is used. The last examples include use of the optional `<descriptor>`.

**fragment with known start/stop**

p\(HGNC:YFG, frag\("5\_20"\)\)

**amino-terminal fragment of unknown length**

p\(HGNC:YFG, frag\("1\_?"\)\)

**carboxyl-terminal fragment of unknown length**

p\(HGNC:YFG, frag\("?\_\*"\)\)

**fragment with unknown start/stop**

p\(HGNC:YFG, frag\("?"\)\)

**fragment with unknown start/stop and a descriptor**

p\(HGNC:YFG, frag\("?", "55kD"\)\)

#### Cellular location

**location\(\), loc\(\)**

`location()` or `loc()` can be used as an argument within any abundance function except `compositeAbundance()` to represent a distinct subset of the abundance at that location. Location subsets of abundances have the general form:

f\(ns:v, loc\(ns:v\)\)

**Examples**

**Cytoplasmic pool of AKT1 protein**

p\(HGNC:AKT1, loc\(MESHCS:Cytoplasm\)\)

**Endoplasmic Reticulum pool of Ca^2+^**

a\(CHEBI:"calcium\(2+\)", loc\(GOCC:"endoplasmic reticulum"\)\)

## Process Functions

The following BEL Functions represent classes of events or phenomena taking place at the level of the cell or the organism which do not correspond to molecular abundances, but instead to a biological process like angiogenesis or a pathology like cancer.

#### biologicalProcess\(\), bp\(\)

`biologicalProcess(ns:v)` or `bp(ns:v)` denotes the process or population of events designated by the value +v+ in the namespace +ns+.

**Examples**

bp\(GOBP:"cell cycle arrest"\) bp\(GOBP:angiogenesis\)

#### pathology\(\), path\(\)

`pathology(ns:v)` or `path(ns:v)` denotes the disease or pathology process designated by the value +v+ in the namespace +ns+. The +pathology\(\)\` function is included to facilitate the distinction of pathologies from other biological processes because of their importance in many potential applications in the life sciences.

**Examples**

pathology\(MESHD:"Pulmonary Disease, Chronic Obstructive"\) pathology\(MESHD:adenocarcinoma\)

**Biological Processes and Pathologies Term Examples**

Biological phenomena that occur at the level of the cell or the organism are considered processes. These terms are represented by values from namespaces like GO and MeSH.

* &lt;&gt;
* &lt;&gt;

**Biological Processes**

Cellular senescence can be represented by:

**Long Form**

biologicalProcess\(GO:"cellular senescence"\)

**Short Form**

bp\(GO:"cellular senescence"\)

**Diseases and Pathologies**

Disease pathologies like muscle hypotonia can be represented by:

**Long Form**

pathology\(MESHD:"Muscle Hypotonia"\)

**Short Form**

path\(MESHD:"Muscle Hypotonia"\)

## Reified Entities

### Composites

The `compositeAbundance(<abundance term list>)` function takes a list of abundance terms. The `compositeAbundance()` or `composite()` function is used to represent cases where multiple abundances synergize to produce an effect. The list is unordered, thus different orderings of the arguments should be interpreted as the same term. This function should not be used if any of the abundances alone are reported to cause the effect. `compositeAbundance()` terms should be used only as subjects of statements, not as objects.

**Example - BEL Statement with compositeAbundance term**

```text
composite(p(HGNC:IL6), complex(GOCC:"interleukin-23 complex")) increases bp(GOBP:"T-helper 17 cell differentiation")
```

In the above example, IL-6 and IL-23 synergistically induce Th17 differentiation.

### Reactions

reaction\(\), rxn\(\)

`reaction(reactants(<abundance term list1>), products(<abundance term list2>))` denotes the frequency or abundance of events in which members of the abundances in `<abundance term list1>` \(the reactants\) are transformed into members of the abundances in `<abundance term list2>` \(the products\).

**Example**

The reaction in which superoxides are dismutated into oxygen and hydrogen peroxide can be represented as:

rxn\(reactants\(a\(CHEBI:superoxide\)\),products\(a\(CHEBI:"hydrogen peroxide"\), a\(CHEBI: "oxygen"\)\)\)

**Reactions**

The `reaction()` or `rxn()` function expresses the transformation of products into reactants, each defined by a list of abundances.

**Example**

This BEL Term represents the reaction in which the reactants phosphoenolpyruvate and ADP are converted into pyruvate and ATP.

**Long Form**

reaction\(reactants\(abundance\(CHEBI:phosphoenolpyruvate\), abundance\(CHEBI:ADP\)\), products\(abundance\(CHEBI:pyruvate\), abundance\(CHEBI:ATP\)\)\)

**Short Form**

rxn\(reactants\(a\(CHEBI:phophoenolpyruvate\), a\(CHEBI:ADP\)\), products\(a\(CHEBI:pyruvate\), a\(CHEBI:ATP\)\)\)

### Fusions

#### fusion\(\), fus\(\)

`fusion()` or `fus()` expressions can be used in place of a namespace value within a gene, RNA, or protein abundance function to represent a hybrid gene, or gene product formed from two previously separate genes. `fusion()` expressions take the general form:

fus\(ns5':v5', "range5'", ns3':v3', "range3'"\)

where `ns5':v5'` is a namespace and value for the 5' fusion partner, `range5'` is the sequence coordinates of the 5' partner, `ns3':v3'` is a namespace and value for the 3' partner, and `range3'` is the sequence coordinates for the 3' partner. Ranges need to be in quotes.

**Example**

**RNA abundance of fusion with known breakpoints**

r\(fus\(HGNC:TMPRSS2, "r.1\_79", HGNC:ERG, "r.312\_5034"\)\)

The **r.** designation in the range fields indicates that the numbering uses the RNA sequence as the reference. RNA sequence numbering starts at the transcription initiation site. You use **c.\_** for g\(\) fusions and **p.\_** for p\(\) fusions. These **r.**, **c.**, and **p.** designations come from [http://www.hgvs.org\[HGVS](http://www.hgvs.org[HGVS) variation description\] convention.

**RNA abundance of fusion with unspecified breakpoints**

r\(fus\(HGNC:TMPRSS2, "?", HGNC:ERG, "?"\)\)

**Fusion Proteins**

The abundances of fusion proteins resulting from chromosomal translocation mutations can be specified by using the `fusion()` or `fus()` function within a protein abundance term.

**Example**

**Long Form**

proteinAbundance\(fusion\(HGNC:BCR, "p.1\_426", HGNC:JAK2, "p.812\_1132"\)\)

**Short Form**

p\(fus\(HGNC:BCR, "p.1\_426", HGNC:JAK2, "p.812\_1132"\)\)

This term represents the abundance of a fusion protein of the 5' partner BCR and 3' partner JAK2, with the breakpoint for BCR at amino acid 426 and JAK2 at 812. _p._ indicates that the protein sequence is used for the range coordinates provided. If the breakpoint is not specified, the fusion protein abundance can be represented as:

p\(fus\(HGNC:BCR, "?", HGNC:JAK2, "?"\)\)

The `fusion()` function can also be used within `geneAbundance` and `rnaAbundance` terms to represent genes and RNAs modified by fusion mutations.

## Examples

Measurable entities like genes, RNAs, proteins, and small molecules are represented as abundances in BEL. BEL Terms for abundances have the general form `a(ns:v)`, where `a` is an abundance function, `ns` is `a` namespace reference and `v` is a value from the namespace vocabulary. See &lt;&gt;.

* &lt;&gt;
* &lt;&gt;
* &lt;&gt;
* &lt;&gt;
* &lt;&gt;
* &lt;&gt;

**Chemicals and Small Molecules**

The general abundance function `<<Xabundancea, abundance()>>` is used to represent abundances of chemicals, small molecules, and any other entities that cannot be represented by a more specific abundance function.

**Examples**

**Long Form**

abundance\(CHEBI:"nitrogen atom"\) abundance\(CHEBI:"prostaglandin J2"\)

**Short Form**

a\(CHEBI:"nitrogen atom"\) a\(CHEBI:"prostaglandin J2"\)

These BEL Terms represent the abundance of the entities specified by _nitrogen atom_ and by _prostaglandin J2_ in the CHEBI namespace.

**Genes, RNAs, and Proteins**

The abundance functions `<<XgeneA, geneAbundance()>>`, `<<XrnaA, rnaAbundance()>>`, and `<<XproteinA, proteinAbundance()>>` are used with namespace values like HGNC human gene symbols, EntrezGene IDs, SwissProt accession numbers to designate the type of molecule represented.

**Examples**

Abundances of the gene, RNA, and protein encoded by the human AKT1 gene are represented as:

**Long Form**

geneAbundance\(HGNC:AKT1\) rnaAbundance\(HGNC:AKT1\) proteinAbundance\(HGNC:AKT1\)

**Short Form**

g\(HGNC:AKT1\) r\(HGNC:AKT1\) p\(HGNC:AKT1\)

These BEL Terms represent the gene, RNA, and protein abundances of the entity specified by _AKT1_ in the HGNC namespace. Equivalent terms can be constructed using a corresponding value from a different namespace. For example, the abundance of the human AKT1 RNA can also be represented by referencing the EntrezGene ID or SwissProt accession namespaces:

r\(EGID:207\) r\(SP:P31749\)

**Protein families**

Protein families are used to represent a group of functionally similar proteins. For example, AKT1, AKT2, and AKT3 together form the AKT family. Like other proteins, abundances of protein families are represented using the `<<XproteinA, proteinAbundance()>>` function, with namespace values from the Selventa named protein families namespace.

**Example**

This term represents the protein abundance of the AKT protein family.

p\(SFAM:"AKT Family"\)

**microRNAs**

The abundance function `<<XmicroRNAA, microRNAAbundance()>>` is used to represent the fully processed, active form of a microRNA. The specific abundance functions allow distinct representations of the gene, RNA, and microRNA abundances for a given namespace value.

**Example**

These BEL Terms represent the abundances of the gene, RNA, and processed microRNA, respectively, for the entity specified by _Mir21_ in the MGI mouse gene symbol namespace.

**Long Form**

geneAbundance\(MGI:Mir21\) rnaAbundance\(MGI:Mir21\) microRNAAbundance\(MGI:Mir21\)

**Short Form**

g\(MGI:Mir21\) r\(MGI:Mir21\) m\(MGI:Mir21\)

**Complexes**

The abundances of molecular complexes are represented using the `<<XcomplexA, complexAbundance()>>` function. This function can take either a list of abundance terms or a value from a namespace of molecular complexes as its argument.

**Example**

Both BEL Terms represent the IkappaB kinase complex. The first by referencing a named protein complex within the [http://geneontology.org/page/cellular-component-ontology-guidelines\[GO](http://geneontology.org/page/cellular-component-ontology-guidelines[GO) Cellular Component namespace\], and the second by enumerating the individual protein abundances that compose the IkappaB kinase complex, CHUK, IKBKB, and IKBKG.

**Long Form**

complexAbundance\(GOCC:"IkappaB kinase complex"\) complexAbundance\(proteinAbundance\(HGNC:CHUK\), proteinAbundance\(HGNC:IKBKB\), proteinAbundance\(HGNC:IKBKG\)\)

**Short Form**

complex\(GOCC:"IkappaB kinase complex"\) complex\(p\(HGNC:CHUK\), p\(HGNC:IKBKB\), p\(HGNC:IKBKG\)\)

**Composite abundances**

Multiple abundance terms can be represented together as the subject of a BEL Statement by using the `<<XcompositeA, compositeAbundance()>>` function. This function takes a list of abundances as its argument and is used when the individual abundances do not act alone, but rather synergize to produce an effect.

**Example**

This term represents the combined abundances of TGFB1 and IL6 proteins.

**Long Form**

compositeAbundance\(proteinAbundance\(HGNC:TGFB1\), proteinAbundance\(HGNC:IL6\)\)

**Short Form**

composite\(p\(HGNC:TGFB1\), p\(HGNC:IL6\)\)

