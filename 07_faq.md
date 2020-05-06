# Best Practices

These pages contain suggestions and guidelines for representing scientific findings in BEL.

* Representation of Experimental Data
* Statement Annotations
* Modified Proteins
* Reactions
* Protein-Protein Interactions
* Protein Families

#### Representation of Experimental Data

In a causal BEL Statement, the subject term frequently represents an experimentally manipulated entity while the object term represents a measured entity. Our best practices apply different levels of inference for mapping subject and object terms, particularly for representing 'omic data.

*  Subject Terms (Perturbations)
*  Relationships
*  Object Terms (Measurements)

##### Subject Terms (Perturbations)

* BELv2How should I represent chemical inhibitor experiments?
* How do I represent experiments that use site-directed mutants?
* How do I represent observations resulting from manipulation of two or more entities?
* How should I represent gene knock out or RNAi experiments?
* How should I represent overexpression experiments?
* When should I use the protein abundance vs. the activity of a protein?

###### BELv2 How should I represent chemical inhibitor experiments?

For experiments where protein activity is perturbed with a chemical inhibitor, we generally use the chemical as the subject term and not the activity of the target protein. In many cases, the effects of the chemical are not specific to the intended target. This representation approach avoids unintended attribution of off-target effects of a chemical to the target protein.

For example, treatment of cells with the PI3 kinase inhibitor LY294002 significantly decreases expression of TGFB2 RNA (http://www.ncbi.nlm.nih.gov/pubmed?term=20629536[PMID 20629536]):

```
a(SCHEM:"LY 294002") -| r(HGNC:TGFB2)
```
In a case where more information is available, the protein activity targeted by the inhibitor can be used as the subject term. For example, if the effect of LY 29004 on TGFB2 RNA expression was demonstrated to require the PIK3CA gene, we could represent the subject term as the kinase activity of the PIK3CA protein.

```
act(p(HGNC:PIK3CA), ma(kin)) -> r(HGNC:TGFB2)
```

###### How do I represent experiments that use site-directed mutants?

The artificial (laboratory) creation of sequence variants is often used to investigate the effects of protein activity or specific post-translational modifications. These include proteins altered to be constitutively active or dominant negative, as well as proteins with specific amino acid residues altered to prevent phosphorylation. While many of these sequence alterations can be precisely represented with BEL, this may not be the best approach to capturing the observations from experiments that use these constructs.

###### Non-phosphorylatable mutant

In this example, mutation of FOXO1 serine 256 to alanine is used to block phosphorylation at 256 (S256A), a site phosphorylated by AKT. The S256A mutation was found to impair phosphorylation of threonine 24 and serine 319 by AKT (http://www.ncbi.nlm.nih.gov/pubmed?term=11237865[PMID 11237865]).
We could represent this observation as follows:

```
p(HGNC:FOXO1, var("p.Ser256Ala")) =| p(HGNC:FOXO1, pmod(Ph, Ser, 256))
```

```
p(HGNC:FOXO1, var("p.Ser256Ala")) =| (p(SFAM:"AKT Family") => p(HGNC:FOXO1, pmod(Ph, Thr, 24)))
```

```
p(HGNC:FOXO1, var("p.Ser256Ala")) =| (p(SFAM:"AKT Family") => p(HGNC:FOXO1, pmod(Ph, Ser, 319)))
```
The first statement indicates that phosphorylation at S256 is blocked by mutation of S256 to alanine. The next two statements indicate that the S256A mutation decreases phosphorylation of FOXO1 threonine 24 and serine 319 by AKT.
However, we are not generally interested in the effects of a lab-created mutant like S256A so much as the role of phosphorylation at serine 256 on phosphorylation of the other two sites. Thus, we recommend the following representation:

```
p(HGNC:FOXO1, pmod(Ph, Ser, 256)) => (p(SFAM:"AKT Family") => p(HGNC:FOXO1, pmod(Ph, Thr, 24)))
p(HGNC:FOXO1, pmod(Ph, Ser, 256)) => (p(SFAM:"AKT Family") => p(HGNC:FOXO1, pmod(Ph, Ser, 319)))
```

Here, the statements indicate that phosphorylation of FOXO1 at S256 increases the phosphorylation of T24 and S319 by the kinase activity of AKT.
While both representations are accurate, the second version is better suited to integrating other information about the role of FOXO1 phosphorylation at S256 into a cohesive, traversable model.

###### How do I represent observations resulting from manipulation of two or more entities?

In some cases an experiment has a complex perturbation, where manipulations of multiple biological entities are required for an effect. Multiple BEL abundance terms can be represented together as the subject of a BEL Statement by using the `compositeAbundance()` or `composite()` function.

In this example, TGF-beta cooperates with IL-6 to generate T-helper 17 cells (http://www.ncbi.nlm.nih.gov/pubmed?term=17918200[PMID 17918200]):

```
composite(p(MGI:Tgfb1), p(MGI:Il6)) -> bp(GO:"T-helper 17 cell differentiation")
```

If the two manipulated components are known to physically interact (such as a receptor and it's ligand), we recommend inferring their effects rather than using a composite term.

In this example, both Met and Hgf (the Met ligand) are required for increased expression of integrin Itgav RNA (http://www.ncbi.nlm.nih.gov/pubmed?term=16710476[PMID 16710476]):

Not recommended:
```
 composite(p(MGI:Hgf), p(MGI:Met)) -> r(MGI:Itgav)
```

Recommended:

 kin(p(MGI:Met)) -> r(MGI:Itgav)

p(MGI:Hgf) -> r(MGI:Itgav)

Because Hgf binds to and directly activates Met, the effect of Met and Hgf together on Itgav RNA expression can be inferred to result from Met activity.

###### How should I represent gene knock out or RNAi experiments?

Our general practice is to represent the subject term for experiments where the perturbation is a gene deletion or RNAi knockdown as the abundance of the corresponding protein.

###### Gene knockouts

In this example, mice with a gene deletion of Nfe2l2 express reduced mRNA of the glutathione S transferase Gsta1 compared to wild-type mice (http://www.ncbi.nlm.nih.gov/pubmed?term=11991805[PMID 11991805]):

p(MGI:Nfe2l2) -> r(MGI:Gsta1)

###### RNA interference

In this example, knockdown of PTEN using RNA interference results in increased CDKN1A protein levels (http://www.ncbi.nlm.nih.gov/pubmed?term=17300726[PMID 17300726]):

p(HGNC:PTEN) -| p(HGNC:CDKN1A)

We assume that the effects of PTEN RNAi are due to knock down of PTEN protein. Decreased PTEN protein resulting in increased CDKN1A protein is interpreted as PTEN decreases CDKN1A protein.

It is generally preferable to represent the subject term as the protein abundance and not an activity of the protein, particularly for 'omic experiments. See also When should I use the protein abundance vs. the activity of a protein?

###### How should I represent overexpression experiments?

For experiments where the perturbation is overexpression of DNA or RNA for the purpose of overexpressing a protein, we generally represent the subject term as a protein abundance.

In this example, SIAH2 and repp86 (TPX2) proteins interact, and overexpression of SIAH2 by transfection increases degradation of TPX2 protein (http://www.ncbi.nlm.nih.gov/pubmed?term=17716627[PMID 17716627]):

```
p(HGNC:SIAH2) => deg(p(HGNC:TPX2))
```

The statement is modeled as direct because the subject and object term proteins interact.

While it would be technically correct to represent overexpressions achieved via DNA transfection as gene abundances and those from mRNA transfections as RNA abundances, this distinction is not useful for applications like Whistle and pathfinding. It is generally preferable to represent the subject term as the protein abundance and not an activity of the protein, particularly for 'omic experiments, if it is not clear that the activity is required or responsible for the effect.

###### When should I use the protein abundance vs. the activity of a protein?

Many experimental perturbations involve the overexpression or knockdown of protein abundance. We generally represent this type of experiment using a protein abundance as the subject term instead of an activity (e.g., kinase or phosphatase) of the protein. While many proteins have a known activity, this activity is not always responsible for all downstream effects of overexpression or knockdown; the same effects occur when either wild-type or catalytically inactive forms are expressed.

Below are examples of BEL statements for cases where:

. the effects of increased or decreased protein abundance are not due to the catalytic activity of the protein,
. effects are likely due to an increase or decrease in the activity of the protein, and
. not enough information is available.

###### Effects are not due to the catalytic activity of the protein

_Example 1._ A mutant telomerase (TERT) protein lacking telomerase activity retains its effects on keratinocyte proliferation (http://www.ncbi.nlm.nih.gov/pubmed/18208333[PMID 18208333]):

```
p(MGI:Tert) -> bp(GO:"cell proliferation")
act(p(MGI:Tert), ma(cat)) causesNoChange bp(GO:"cell proliferation")
```

_Example 2._ The serine-threonine kinase RIPK1 activates NF-kappaB through a mechanism that does not involve the protein's kinase activity (http://www.ncbi.nlm.nih.gov/pubmed/20354226[PMID 20354226]).

```
p(HGNC:RIPK1) -> act(p(GOCC:"NF-kappaB complex"), ma(tscript))
act(p(HGNC:RIPK1), ma(kin)) causesNoChange act(p(GOCC:"NF-kappaB complex"), ma(tscript))
```

###### Effects are likely due to the activity of the protein

_Example._ Genes are differentially expressed in wild-type vs. Met knock out mouse hepatocytes, only after treatment with Hgf, the Met ligand (http://www.ncbi.nlm.nih.gov/pubmed/16710476[PMID 16710476]). These genes include integrins Itgav, Itga3, and Itgb1.

```
act(p(MGI:Met), ma(kin)) -> r(MGI:Itgav)
act(p(MGI:Met), ma(kin)) -> r(MGI:Itga3)
act(p(MGI:Met), ma(kin)) -> r(MGI:Itgb1)
```

Because both Met and the Met ligand are required for the increase in gene expression, we use the activity of Met as the subject term.

###### Not enough information is available

_Example._ Genes are differentially expressed in the pancreas of mice with a pancreas-specific beta-catenin deletion (http://www.ncbi.nlm.nih.gov/pubmed/17222338[PMID 17222338]). These include decreased expression of the hedgehog interacting protein Hhip in the knock-out compared to wild-type.

```
p(MGI:Ctnnb1) -> r(MGI:Hhip)
```

In this case, not enough information is available to determine if the change in Hhip expression is due to Ctnnb1 function in transcription, cell adhesion, or another role.

##### Relationships

###### When should I use a correlative relationship?

Correlative Relationships, Correlative relationships are more appropriate than Causal Relationships, causal relationships to represent observations that do not clearly result from an experimental perturbation. BEL causal relationships include `increases`, `decreases`, `directlyIncreases`, `directlyDecreases`, and `regulates`. Correlative relationships include `positiveCorrelation` and `negativeCorrelation`.

###### Correlative

If the observation comes from the comparison of human tumors grouped by the occurrence of a specific mutation, then the relationship should generally be expressed as correlative. In this case there is no experimental perturbation.
In this example, most patient tumor samples with an EGFR L858R mutation were observed to exhibit a reduction in ERBB2 tyrosine 1248 phosphorylation compared to wild-type samples (http://www.ncbi.nlm.nih.gov/pubmed/?term=18687633[PMID 18687633]):

```
p(HGNC:EGFR, var("p.Leu858Arg")) negativeCorrelation p(HGNC:ERBB2, pmod(Ph,Tyr,1248))
```

In this case, no evidence is presented to suggest that the differences in ERBB2 phosphorylation are causally related to the EGFR mutation, only that the two observations are inversely correlated. Note that the subject and object terms are interchangeable for correlative relationships.

###### Causal

If the observation comes from the comparison of experimentally controlled states, like gene deletion, overexpression, or introduction of a mutant allele into a cell line or animal, the experimental perturbation can generally be represented as the subject term of a causal statement.
In this example, DUSP6 RNA is observed to be upregulated in immortalized human bronchial epithelial cells transfected with EGFR mutant L858R, as compared to WT EGFR (http://www.ncbi.nlm.nih.gov/pubmed/16489012[PMID 16489012]):

```
p(HGNC:EGFR, var("p.Leu858Arg")) -> r(HGNC:DUSP6)
```

In this case, the EGFR mutation is introduced as an experimentally-controlled perturbation.

##### Object Terms (Measurements)

###### How should I represent microarray data?

We record the results of experiments like microarrays and RT-PCR, which measure RNA abundances, by representing the object terms as RNA abundances. Only significant effects (e.g., meeting minimum criteria for fold change and statistical significance) should be recorded in BEL Statements.

In a causal BEL Statement, the subject term generally represents an experimentally manipulated entity while the object term represents a measured entity. Our general practice is to represent the object terms in BEL Statements with the terms most closely related to the experimental measurement.

This direct representation of the measurement in BEL supports the creation of KAMs to which 'omic data can be mapped directly and analyzed using automated reasoning applications like Whistle. Inference of the potential downstream consequences of RNA expression changes is supported by connection of RNA abundances to the corresponding proteins during knowledge network compilation

#### Statement Annotations

##### How do I annotate a relationship observed in multiple biological contexts?

Often, the scientific literature reports a relationship as occurring across several biological contexts.

Our general practice is to represent each observation with a separate statement. Several annotations can be used to describe the same context, e.g., 'lung' and 'fibroblast', but distinct BEL statements should be used to describe each experimental context that the relationship is observed in.

###### Example

http://www.ncbi.nlm.nih.gov/pubmed/18650932[PMID 18650932] - siRNA knockdown of the atypical PKC-interacting protein Par-4 (PAWR) increases phosphorlyation of AKT at Serine 473 in both human 293 and A549 cells.

"To test whether this is also true in human cells, we used a Par-4 siRNA to deplete endogenous Par-4 levels in human 293 cells and in the A549 human lung adenocarcinoma cell line. Cells were treated with control or Par-4-specific siRNAs, after which they were kept for 24 h in serum-free medium conditions and then stimulated with serum. Data in http://www.ncbi.nlm.nih.gov/pmc/articles/PMC2519103/figure/f5/[Figure 5E and F] clearly demonstrate that the knockdown of Par-4 provokes enhanced serum-activated phospho-Akt-Ser473 levels in A549 and 293 human cells, respectively."

Not recommended:

```
SET CellLine = {A549, 293}
p(HGNC:PAWR) -| p(SFAM:"AKT Family", pmod(Ph, Ser, 473))
```

Recommended:

```
SET CellLine = A549

p(HGNC:PAWR) -| p(SFAM:"AKT Family", pmod(Ph, Ser, 473))

SET CellLine

p(HGNC:PAWR) -| p(SFAM:"AKT Family", pmod(Ph, Ser, 473))
```

#### Modified Proteins

    * How do I represent a protein modification when specific information is not available?
    * How do I represent a protein modification within a complex?
    * Xmultphosphforprotein, How do I represent a situation where multiple phosphorylations are required for a protein's activity?
    * How do I represent a situation where one protein modification initiates additional modifications?
    * Xremovalofproteinmod, How do I represent removal of a protein modification (e.g., dephosphorylation, deubiquitination)?

##### How do I represent a protein modification when specific information is not available?

BEL terms for Protein Modifications, post-translational modifications of proteins specify the type of modification, the modified amino acid, and the position of the modified amino acid. The modified amino acid and position are not required, so protein modifications can be represented with less specific information.

###### Example

Human AKT1 protein modified by phosphorylation at serine 473

p(HGNC:AKT1, pmod(Ph, Ser, 473))

Human AKT1 protein modified by phosphorylation at an unspecified serine residue

p(HGNC:AKT1, pmod(Ph, Ser))

Human AKT1 protein that has been modified by phosphorylation at an unspecified amino acid residue

p(HGNC:AKT1, pmod(Ph))

As a general rule, if specific information is available, it should be used. In some cases, this involves investigation sections of a paper outside of the evidence text or other referenced papers to determine which specific modifications have been measured.

Non-specific protein modification terms have limited value in the context of a knowledge network. For example, phosphorylation at different sites of the same protein can have opposing effects.
For example: "Akt-phosphorylated FOXO interacts with the ubiquitin ligase Skp2 and is targeted for proteasomal degradation" (http://www.ncbi.nlm.nih.gov/pubmed?term=15917664[PMID 15917664])

###### Example

Recommended:

 act(p(SFAM:"AKT Family"), ma(kin)) => (act(p(HGNC:SKP2)) => deg(p(SFAM:"FOXO Family")))

Not recommended:

```
p(SFAM:"FOXO Family", pmod(Ph)) => (act(p(HGNC:SKP2)) => deg(p(SFAM:"FOXO Family")))
```

The first BEL Statement indicates that the kinase activity of AKT increases the degradation of FOXO by SKP2. The second statement indicates that phosphorylation of FOXO increases the degradation of FOXO by SKP2. In this case, more information is captured by using the phosphorylating kinase AKT as the subject term instead of the non-specified phosphorylation of FOXO.

##### How do I represent a protein modification within a complex?

In many cases, complexes include proteins with post-translational modifications and these modifications influence complex formation. For example, HIF1A that has been hydroxylated on proline residues 402 and 564 interacts with VHL (http://www.ncbi.nlm.nih.gov/pubmed?term=17925579[PMID 17925579]).

Our general practice is to represent this type of event as a causal statement in BEL, with the modified protein as the subject term and the complex with no specified modifications as the object term. Because the modified protein is a component of the complex, we use a direct causal relationship:

```
p(HGNC:HIF1A, pmod(Hy, Pro, 402)) => complex(p(HGNC:HIF1A), p(HGNC:VHL))

p(HGNC:HIF1A, pmod(Hy, Pro, 564)) => complex(p(HGNC:HIF1A), p(HGNC:VHL))
```


While BEL allows representation of complexes with the modified proteins as components, we do not recommend this approach:

```
complex(p(HGNC:HIF1A, pmod(Hy, Pro, 402)), p(HGNC:VHL))
```

The practice of composing a complex using protein abundances without any specified modifications provides a standardized representation for complexes and allows the effects of modifications on complex formation to be captured as causal relationships. The modified forms of HIF1A, `p(HGNC:HIF1A, pmod(Hy, Pro, 402))` and `p(HGNC:HIF1A, pmod(Hy, Pro, 564))` are considered a subset of the total `p(HGNC:HIF1A)`.

This approach enables representation of the effects of multiple protein modifications on complex formation by using a causal statement for each modification.

BELv2.0 does not provide a specific representation of unmodified protein abundances.

###### *Exception – modified histones bound to the promoter of a specific gene*

One exception to our general practice of not specifying protein modifications within complex abundances is the interaction of specific modified histones with the promoter of a specific gene. In this example, cigarette smoke is observed to increase H3K27me3 levels at the DKK1 promoter (http://www.ncbi.nlm.nih.gov/pubmed?term=19351856[PMID 19351856]).

```
 a(SCHEM:"smoke condensate, cigarette (gas phase)") -> \
    complex(p(PFH:"Histone H3 Family",pmod(Me3, Lys, 27)), g(HGNC:DKK1))
```

In this example, the modification of the histone by trimethylation does not affect its binding to the gene DKK1. In addition, cigarette smoke does not increase or decrease the overall abundance of the modified histone, only the abundance of the modified histone at the DKK1 promoter.

##### How do I represent a situation where multiple phosphorylations are required for a protein's activity?

In many cases two or more distinct modifications are required simultaneously for protein activity, and neither modification alone is sufficient. For example, MAPK3 must be phosphorylated at two sites, Threonine 202 and Tyrosine 204, to be active. Our general practice is to take the simple, most general approach, and model the effect of each site separately:

```
p(HGNC:MAPK3, pmod(Ph, Thr, 202)) => act(p(HGNC:MAPK3), ma(kin))

p(HGNC:MAPK3, pmod(Ph, Tyr, 204)) => act(p(HGNC:MAPK3), ma(kin))
```

A multiply-modified abundance term can be used if it is of high importance to capture the requirement for both modifications:

```
p(HGNC:MAPK3, pmod(Ph, Thr, 202)), pmod(Ph, Tyr, 204)) => act(p(HGNC:MAPK3), ma(kin))
```

##### How do I represent a situation where one protein modification initiates additional modifications?

In many cases a specific protein modification may be dependent on another modification of the same protein. In this case, the first protein modification can be modeled as the upstream cause of the second. In this example, phosphorylation of CTNNB1 at Serine 45 initiates phosphorylation of CTNNB1 at other sites including Threonine 41 by GSK3 (http://www.ncbi.nlm.nih.gov/pubmed/?term=16618120[PMID 16618120]):

```
p(HGNC:CTNNB1, pmod(Ph, Ser, 45)) => p(HGNC:CTNNB1, pmod(Ph, Thr, 41))
```

We represent this relationship as direct, because the subject and object terms have the same root abundance node.

Because the kinase mediating the second phosphorylation is known, this relationship can be modeled alternatively as a nested statement:

```
p(HGNC:CTNNB1, pmod(Ph, Ser, 45)) => \
    (kin(p(SFAM:"GSK3 Family")) => p(HGNC:CTNNB1, pmod(Ph, Thr, 41)))
```

##### How do I represent removal of a protein modification (e.g., dephosphorylation, deubiquitination)?

Removal of a specific protein modification is represented simply as a decrease in the abundance of the modified protein.

###### Deubiquitination

In this example, STAMBP deubiquitinates F2RL1 protein (http://www.ncbi.nlm.nih.gov/pubmed?term=19684015[PMID 19684015]):

```
act(p(HGNC:STAMBP)) =| p(HGNC:F2RL1, pmod(Ub))
```

Deubiquitination is represented simply as a decrease in the ubiquitinated form of the protein. Because in this example STAMBP is the deubiquitinating enzyme, we used a directlyDecreases relationship.

###### Dephosphorylation

In this example, the phosphatase CDC25C dephosphorylates CDK1 at tyrosine 15 (http://www.ncbi.nlm.nih.gov/pubmed?term=1384126[PMID 1384126]):

```
act(p(HGNC:CDC25C), ma(phos)) =| p(HGNC:CDK1, pmod(Ph, Tyr, 15))
```

Similar to deubiquitination, dephosphorylation is represented simply as a decrease in the modified form of the protein.

#### Reactions

    * How can I represent a reversible metabolic reaction?
    * When and why should I use a reaction term?

##### How can I represent a reversible metabolic reaction?

A reversible reaction can be represented by modeling the reaction with the products and reactants interchanged.

For example, HSD11B1 acts primarily to convert cortisone to active cortisol, but in some cell types the reverse reaction is favored (http://www.ncbi.nlm.nih.gov/pubmed?term=12530648[PMID 12530648]):

```
 act(p(HGNC:HSD11B1), ma(cat)) => \
  rxn(reactants(a(CHEBI:NADPH), a(CHEBI:cortisone)), products(a(CHEBI:"NADP(+)"), a(CHEBI:cortisol)))

 act(p(HGNC:HSD11B1), ma(cat)) => \
  rxn(reactants(a(CHEBI:"NADP(+)"), a(CHEBI:cortisol)), products(a(CHEBI:NADPH), a(CHEBI:cortisone)))
```

The top statement represents the forward reaction and the bottom statement represents the reverse reaction.

##### When and why should I use a reaction term?

###### When_ should I use a reaction term

Reaction terms allow the representation of a transformation of a list of reactants into a list of products. In this example, the superoxide dismutase SOD1 converts superoxide to hydrogen peroxide:

```
act(p(HGNC:SOD1), ma(cat)) => rxn(reactants(a(CHEBI:superoxide)), products(a(CHEBI:"hydrogen peroxide")))
```

It is not necessary to include all reactants and products, especially if they are ubiquitous small molecules. In the above example the reactant hydrogen and product oxygen have been omitted from the reaction.

###### Why_ should I use a reaction term

It is possible to represent the above reaction with separate statements linking the activity of SOD1 to decreased abundances of the reactants and increased abundances of the products:

```
act(p(HGNC:SOD1), ma(cat)) =| a(CHEBI:superoxide)
act(p(HGNC:SOD1), ma(cat)) => a(CHEBI:"hydrogen peroxide")
```

While this representation describes the function of the catalytic enzyme SOD1, it does not link the product hydrogen peroxide to the reactant superoxide.

#### Protein-Protein Interactions

##### How do I represent a physical interaction between two entities?

You can use a complex abundance to represent binding events between two or more abundance terms. The subject term of your BEL Statement will be the complex. BEL Statements do not require a relationship and object term.

The order in which the members of the complex are listed is not important.

###### Examples

EPOR physically interacts with CSF2RB, the common beta-receptor (http://www.ncbi.nlm.nih.gov/pubmed?term=15456912[PMID 15456912])

```
 complex(p(MGI:EPOR), p(MGI:CSF2RB))
```

KEAP1 binds 15-deoxy-Delta(12,14)-prostaglandin J2 (http://www.ncbi.nlm.nih.gov/pubmed?term=15917255[PMID 15917255])

```
 complex(p(HGNC:KEAP1), a(CHEBI:"15-deoxy-Delta(12,14)-prostaglandin J2"))
```

KEAP1, CUL3, and RBX1 copurify and are part of a functional E3 ubiquitin ligase complex (http://www.ncbi.nlm.nih.gov/pubmed?term=15572695[PMID 15572695])

```
 complex(p(HGNC:KEAP1), p(HGNC:RBX1), p(HGNC:CUL3))
```

The AP-1 transcription complex binds the CCL23 promoter (http://www.ncbi.nlm.nih.gov/pubmed?term=17368823[PMID 17368823])

```
 complex(p(GOCC:"AP1 complex"), g(HGNC:CCL23))
```

#### Protein Families

##### When should I use a protein family instead of a specific protein?

Protein families can be used to represent protein abundances in cases where the information presented by the source does not allow identification of the specific protein. For example:

###### Example 1:

```
SET Citation = "pubmed:11715018"
SET Evidence = "Akt physically associates with MDM2 and phosphorylates it at Ser166 and Ser186."

act(p(fplx:AKT), ma(kin)) => p(HGNC:MDM2, pmod(Ph,Ser,166))
act(p(fplx:AKT), ma(kin)) => p(HGNC:MDM2, pmod(Ph,Ser,186))
```

Here, Akt may refer to AKT1, AKT2, and/or AKT3.

###### Example 2:

```
SET Citation = "pubmed:17003045"
SET Evidence = "We show that Siah2 is subject to phosphorylation by p38 MAPK ... Phosphopeptide mapping identified T24 and S29 as the primary phospho-acceptor sites."

act(p(PFH:"MAPK p38 Family"), ma(kin)) => p(HGNC:SIAH2, pmod(Ph, Thr, 24))
act(p(PFH:"MAPK p38 Family"), ma(kin)) => p(HGNC:SIAH2, pmod(Ph, Ser, 29))
```
Here, it is not clear which specific p38 MAPK is responsible (MAPK11, MAPK12, MAPK13, or MAPK14).

###### Example 3:

"Hip encodes a membrane glycoprotein that binds to all three mammalian Hedgehog proteins." (http://www.ncbi.nlm.nih.gov/pubmed?term=10050855[PMID 10050855])

 complex(p(SFAM:"Hedgehog Family"), p(MGI:Hhip))

 complex(p(MGI:Ihh), p(MGI:Hhip))

 complex(p(MGI:Shh), p(MGI:Hhip))

 complex(p(MGI:Dhh), p(MGI:Hhip))

In this case, all three hedgehog family members are reported to bind to the hedgehog interacting protein (Hhip). Statements can be modeled using the family as well as each individual member.
