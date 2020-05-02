# Examples in BEL Script


    *   <<Causal Statement Examples>>
    *   <<Correlative Statement Examples>>
    *   <<Direct Causal Statement Examples>>
    *   <<Nested Statement Example>>

##### Causal Statement Examples

Causal statements connect subject and object terms with a causal `<<Xincreases, increases>>`, `<<Xdecreases, decreases>>`, or `<<Xcnc, causesNoChange>>` relationship. Subject terms can be an abundance or process (including activities and transformations) and object terms can be either an abundance, a process, or a second BEL Statement.

* <<Causal increase>>
* <<Causal decrease>>
* <<Causes no change>>

###### Causal increase

###### Example

These statements use the causal `<<Xincreases, increases>>` relationship. These statements are annotated with a citation and supporting evidence text, as well as with the cell line and species context for the experimental observations represented by the statements. These two statements represent the observation that increases in IL6 protein abundance cause increases in the RNA abundance of ENO1 and XBP1. These statements are annotated with CellLine and Species to indicate that the experimental observation was made in the context of the cell line "U266" and species "9606" (Homo sapiens).

###### Long Form

 SET Citation = {"PubMed", "Int J Oncol 1999 Jul 15(1) 173-8", "10375612"}

 SET Support = "Northern blot analysis documented that two
  transcription factor genes chosen for further study, c-myc
  promoter-binding protein (MBP-1) and X-box binding protein 1
  (XBP-1), were up-regulated in U266 cells about 3-fold relative
  to the cell cycle-dependent beta-actin gene 12 h after IL-6
  treatment"

 SET CellLine = "U266"

 SET Species = "9606"

```
// disambiguation MBP-1 = HNGC ENO1
proteinAbundance(HGNC:IL6) increases rnaAbundance(HGNC:ENO1)
proteinAbundance(HGNC:IL6) increases rnaAbundance(HGNC:XBP1)
```

###### Short Form

```
p(HGNC:IL6) -> r(HGNC:ENO1)
p(HGNC:IL6) -> r(HGNC:XBP1)
```

###### Causal decrease

###### Example

This statement demonstrates a causal statement using the `<<Xdecreases, decreases>>` relationship. The statement expresses that increases in the abundance of corticosteroid molecules cause decreases in the frequency or intensity of the biological process inflammation. This statement is annotated with an Anatomy and Disease to indicate that the relationship was observed in the context of the _cardiovascular system_ and the disease _Stroke_.

###### Long Form

```
 SET Citation = {"PubMed", "J Mol Med. 2003 Mar;81(3):168-74. Epub 2003 Mar 14.", "12682725"}

 SET Support ="high-dose steroid treatment decreases vascular
  inflammation and ischemic tissue damage after myocardial
  infarction and stroke through direct vascular effects involving
  the nontranscriptional activation of eNOS"

 SET Anatomy = "cardiovascular system"

 SET MeSHDisease = "Stroke"

 abundance(CHEBI:corticosteroid) decreases biologicalProcess(MESHD:Inflammation)
```

###### Short Form

```
a(CHEBI:corticosteroid) -| path(MESHD:Inflammation)
```

###### Causes no change

The `<<Xcnc, causesNoChange>>` relationship can be used to record the lack of an observed effect.

###### Example

The epidermal growth factor receptor (EGFR) ligand amphiregulin (AREG) is
observed to increase NF-kappaB transcriptional activity while the EGFR ligand
EGF has no effect.These statements express that an increase of AREG protein
abundance causes an observed increase in the transcriptional activity of the
NF-kappaB complex, and that an increase EGF does not.

###### Long Form

```
 SET Citation = {"PubMed", "Mol Cancer Res 2007 Aug 5(8) 847-61", "17670913"}

 SET Support ="Furthermore, EGFR, activated by amphiregulin but not
  epidermal growth factor, results in the prompt activation of the
  transcription factor nuclear factor-kappaB (NF-kappaB)"

 # disambiguation Amphiregulin = HGNC AREG

 proteinAbundance(HGNC:AREG) increases activity(complexAbundance(GOCC:"NF-kappaB complex"), molecularActivity(tscript))

 proteinAbundance(HGNC:EGF) causesNoChange activity(complexAbundance(GOCC:"NF-kappaB complex"), molecularActivity(tscript))
```

###### Short Form

```
p(HGNC:AREG) -> act(complex(GOCC:"NF-kappaB complex"), ma(tscript))
p(HGNC:EGF) causesNoChange act(complex(GOCC:"NF-kappaB complex"), ma(tscript))
```

##### Correlative Statement Examples

<<XcorRels, Correlative Relationships>> link abundances and biological processes when no causal relationship is known.

* <<negativeCorrelation>>
* <<XassociationCR, association>>

###### negativeCorrelation

This statement expresses that an increase in cytoplasmic FGF2 protein
positively correlates with an increase in the pathology Chronic Obstructive
Pulmonary Disease. The subject and object terms of correlative statements are
interchangeable. The `<<XnegCor, negativeCorrelation>>` relationship is used to
represent inverse correlative relationships, i.e., a decrease in A is
correlated with an increase in B.

```
 SET Citation = {"PubMed", "J Pathol. 2005 May;206(1):28-38.", "15772985"}

 SET Support ="Quantitative digital image analysis revealed
 increased cytoplasmic expression of FGF-2 in bronchial epithelium
 (0.35 +/- 0.03 vs 0.20 +/- 0.04, p < 0.008) and nuclear
 localization in ASM (p < 0.0001) in COPD patients compared with
 controls."

 SET Tissue = "epithelium"

 proteinAbundance(HGNC:FGF2, location(GOCC:cytoplasm)) positiveCorrelation \
  pathology(MESHD:"Pulmonary Disease, Chronic Obstructive")
```

###### association

The direction of causal effect or correlation of two abundance or biological process terms is not always specified. The `<<Xassociation, association>>` relationship can be used in these cases.

This statement represents that abundance of protein designated by the name Nr2f2 in the MGI namespace is associated in an unspecified manner with the biological process angiogenesis.

###### Long Form

```
 SET Citation = {"PubMed", "Mech Ageing Dev. 2004 Oct-Nov;125(10-11):719-32.", "15541767"}

 SET Support ="COUP-TFII is involved in the angiogenic process in the developing embryos."

 # disambiguation - COUP-TFII refers to MGI Nr2f2

 SET MeSHAnatomy = "Embryo, Mammalian"

 proteinAbundance(MGI:Nr2f2) association biologicalProcess(GO:angiogenesis)
```

###### Short Form

```
p(MGI:NR2F2) -- bp(GO:angiogenesis)
```

##### Direct Causal Statement Examples

The following examples demonstrate the use of <<Direct causal relationships, direct casual relationships>> in causal statements. The direct causal relationships `<<XdIncreases, directlyIncreases>>` and `<<XdDecreases, directlyDecreases>>` are special forms of the causal `<<Xincreases, increases>>` and `<<Xdecreases, decreases>>` relationships where the mechanism of the causal relationship involves the physical interaction of entities related to the BEL Statement subject and object terms.

* <<Example - Ligand and Receptor>>
* <<Example - Kinase and Substrate>>
* <<Example - Catalyst and Reaction>>
* <<Example - Self-Referential Relationships>>
* <<Example - Direct Transcriptional Control>>

###### Example - Ligand and Receptor

In this example, the `<<XdIncreases, directlyIncreases>>` relationship is used to represent activation of a receptor by its ligand. This statement expresses that amphiregulin (AREG) activates its receptor, the Epidermal Growth Factor Receptor (EGFR). This relationship is direct because ligands directly interact with their receptors.

###### Long Form

```
SET Citation = {"PubMed", "Mol Cancer Res 2007 Aug 5(8) 847-61", "17670913"}
SET Support ="Furthermore, EGFR, activated by amphiregulin"
// disambiguation Amphiregulin = HGNC AREG
// EGFR is known to have kinase activity
proteinAbundance(HGNC:AREG) directlyIncreases activity(proteinAbundance(HGNC:EGFR), molecularActivity(kin))
```

###### Short Form
p(HGNC:AREG) => act(p(HGNC:EGFR), ma(kin))

###### Example - Kinase and Substrate

In this example, the `<<XdIncreases, directlyIncreases>>` relationship is used
to represent the phosphorylation of a protein substrate by a kinase. This
statement expresses that the kinase activity of CDK1 protein causes an increase
in the modification of FOXO1 protein by phosphorylation at serine 249. The
relationship is direct because the kinase physically interacts with its target.

###### Long Form

```
SET Citation = {"PubMed", "Science 2008 Mar 21 319(5870) 1665-8.", "18356527"}
SET Support ="We found that Cdk1 phosphorylated the transcription factor FOXO1 at Ser249 in vitro and in vivo."
activity(p(HGNC:CDK1), ms(kin)) => p(HGNC:FOXO1, pmof(Ph, Ser, 249))
```

###### Example - Catalyst and Reaction

In this example, the direct activation of a reaction by a catalytic enzyme is
represented. The statement indicates that an increase in the catalytic activity
of ALOX5 increase the transformation of the reactant '5(S)-HPETE' to the products
'leukotriene A4' and 'water'. The relationship is considered direct because
ALOX5 protein is the catalyzing enzyme.

###### Long Form

```
 SET Citation = {"Other", "Reactome: Leukotriene synthesis", "REACT_15354.1"}

 SET Support ="Dehydration of 5-HpETE to leukotriene A4. In the
  second step, 5-lipoxygenase converts 5-HpETE to an allylic
  epoxide, leukotriene A4."

 activity(proteinAbundance(HGNC:ALOX5), molecularActivity(cat)) directlyIncreases \
  reaction(reactants(abundance(CHEBI:"5(S)-HPETE")), \
  products(abundance(CHEBI:"leukotriene A4"), abundance(CHEBI:water)))
```

###### Short Form

```
 act(p(HGNC:ALOX5), ma(cat)) => rxn(reactants(a(CHEBI:"5(S)-HPETE")), products(a(CHEBI:"leukotriene A4"), a(CHEBI:water)))
```

###### Example - Self-Referential Relationships

In this example, the `<<XdDecreases, directlyDecreases>>` relationship is used to represent the effect of a protein modification on the activity of the same protein. These statements express that the modification of GSK3A and GSK3B protein by phosphorylation on serines 9 and 21, respectively, inhibits the activity of GSK3A and GSK3B. These relationships are considered direct, because they are self-referential. The modification of the protein abundance by phosphorylation inhibits the activity of the same protein abundance.

###### Long Form

```
 SET Citation = {"PubMed", "Proc Natl Acad Sci U S A 2000 Oct 24 97(22) 11960-5", "11035810"}

 SET Support ="GSK-3 activity is inhibited through phosphorylation
  of serine 21 in GSK-3 alpha and serine 9 in GSK-3 beta."

 proteinAbundance(HGNC:GSK3A, proteinModification(Ph, Ser, 21)) \
  directlyDecreases activity(proteinAbundance(HGNC:GSK3A))

 proteinAbundance(HGNC:GSK3B, proteinModification(Ph, Ser, 9)) \
  directlyDecreases activity(proteinAbundance(HGNC:GSK3B))
```

###### Short Form

```
p(HGNC:GSK3A, pmod(Ph, S, 21)) =| act(p(HGNC:GSK3A))
p(HGNC:GSK3B, pmod(Ph, S, 9)) =| act(p(HGNC:GSK3B))
```

###### Example - Direct Transcriptional Control

In this example, the direct activation of a RNA transcription is encoded. The statement expresses that increases in the transcriptional activity of FOXO1 protein directly increase the RNA abundance of CEBPB. This relationship is considered direct because the transcription factor, FOXO1, directly binds the promoter of the CEBPB gene, increasing the expression of CEBPB RNA.

###### Long Form

 SET Citation = {"PubMed", "Biochem Biophys Res Commun. 2009 Jan 9;378(2):290-5. Epub 2008 Nov 21.", "19026986"}

 SET Support ="We found that Foxo1 increased the expression of
  CCAAT/enhancer binding protein (C/EBPbeta, a positive regulator
  of monocyte chemoattractant protein (MCP)-1 and interleukin
  (IL)-6 genes, through directly binding to its promoter."

 activity(proteinAbundance(HGNC:FOXO1), molecularActivity(tscript)) \
  directlyIncreases rnaAbundance(HGNC:CEBPB)

###### Short Form

 act(p(HGNC:FOXO1), ma(tscript)) => r(HGNC:CEBPB)

##### Nested Statement Example

This example demonstrates use of a nested causal statement in which the object of a causal statement is itself a causal statement.
In the relationship described by the evidence text, CLSPN specifically increases the activity of ATR to phosphorylate the target protein CHEK1 and does not affect the kinase activity of ATR towards its other targets. The use of the nested statement allows the representation of the information that CLSPN increases the phosphorylation of CHEK1 via the kinase activity of ATR, without incorrectly indicating that CLSPN generally increases the kinase activity of ATR.

###### Long Form

 SET Citation = {"PubMed", "Mol Cell Biol 2006 Aug 26(16) 6056-64.", "16880517"}

 SET Species = "9606"

 SET Support ="Consistently, the RNAi-mediated ablation of Claspin
  selectively abrogated ATR's ability to phosphorylate Chk1 but not
  other ATR targets."

 proteinAbundance(HGNC:CLSPN) increases \
 (activity(proteinAbundance(HGNC:ATR), molecularActivity(kin)) directlyIncreases proteinAbundance(HGNC:CHEK1, proteinModification(Ph)))

###### Short Form

p(HGNC:CLSPN) -> (act(p(HGNC:ATR), ma(kin)) => p(HGNC:CHEK1, pmod(Ph)))

#### Other Examples


##### Membership Assignment Examples

These examples demonstrate the assignment of members to groups. Because all BEL terms denote classes, membership in a group is an important special case where subsets of a class that define the class are designated.

[TIP]
####
The BEL Framework adds family members to protein families and complex components to named complexes during network compilation.
####

* <<Protein Family>>
* <<Complex Component>>

###### Protein Family

In this example, members of a protein family are assigned using the `<<hasMember>>` and `<<hasMembers>>` relationships.

The `hasMembers` relationship is used to assign a list of protein abundances as members of a protein family. This relationship is a syntactic convenience that is equivalent to the set of two statements using the `hasMember` relationship. These statements designate the protein abundances of MAPK8 and MAPK9 as members of the JNK MAPK protein family. The term representing the JNK family is a protein abundance based on the name _MAPK JNK Family_ in the Selventa Protein Families namespace.
p(SFAM:"MAPK JNK Family") hasMembers list(p(HGNC:MAPK8), p(HGNC:MAPK9))

The `hasMember` relationship is used to assign individual protein abundances to a protein family.
p(SFAM:"MAPK JNK Family") hasMember p(HGNC:MAPK8)
p(SFAM:"MAPK JNK Family") hasMember p(HGNC:MAPK9)

###### Complex Component

In this example components are assigned to a named protein complex using the <<hasComponent>> and <<hasComponents>> relationships.

The `hasComponents` relationship is similar to the `hasMembers` relationship and is used to assign a list of abundances as components of a complex.These statements designate the protein abundances of RAD9A, RAD1, and HUS1 as components of the complex abundance of the _checkpoint clamp complex_.

 complex(GOCC:"checkpoint clamp complex") hasComponents list(p(HGNC:RAD9A), p(HGNC:RAD1), p(HGNC:HUS1))

The `hasComponent` relationship is used to assign individual abundances to a named protein complex.

 complex(GOCC:"checkpoint clamp complex") hasComponent p(HGNC:RAD9A)

 complex(GOCC:"checkpoint clamp complex") hasComponent p(HGNC:RAD1)

 complex(GOCC:"checkpoint clamp complex") hasComponent p(HGNC:HUS1)

The single `hasComponents` statement is equivalent to the set of three `hasComponent` statements.
