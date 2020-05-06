# Predicates

The following relationship types are included in BEL:

* Causal relationships
* Correlative Relationships
* Associative Relationships
* Genomic Relationships
* Ontological Relationships
* Deprecated Relationships

## Causal Relationships

These relationship types denote a causal relationship, or the absence of a causal relationship between a subject and an object term.

### Regulates

For terms A and B, `A regulates B` or `A reg B` indicate that A is reported to have an effect on B, but information is missing about whether A increases B or A decreases B. This relationship provides more information than association, association, because the upstream entity \(source term\) and downstream entity \(target term\) can be assigned.

This relationship is equivalent to RO:0002211.

There isn't currently a differentiation between direct regulation and indirect regulation, so right now this should be considered as indirect.

## Increases

For terms A and B, `A increases B` or `A -> B` indicate that increases in A have been observed to cause increases in B.

Depending on the terms A and B, this relationship can be used to describe a phosphorylation event, the increase in the amount of a protein, the activation of a protein, the transportation of a protein, or several other things.

### Directly Increases

For terms A and B, `A directlyIncreases B` or `A => B` indicates that increases in A have been observed to cause increases in B and that the mechanism of the causal relationship is based on physical interaction of entities related to A and B. This is a Direct Relationships, direct version of the increases relationship.

### Decreases

For terms A and B, `A decreases B` or `A -| B` indicate that increases in A have been observed to cause decreases in B.

Depending on the terms A and B, this relationship can be used to describe a dephosphorylation event, the decrease in the amount of a protein, the deactivation of a protein, the inhibition of the transportation of a protein, or several other things.

### Directly Decreases

For terms A and B, `A directlyDecreases B` or `A =| B` indicates that increases in A have been observed to cause decreases in B and that the mechanism of the causal relationship is based on physical interaction of entities related to A and B. This is a Direct Relationship, direct version of the decreases relationship.

For example, the inhibition of the Patched 1 receptor signaling activity by Hedgehog is represented as direct, because Hedgehog and Patched 1 physically interact:

```text
# long form
p(fplx:Hedgehog) directlyDecreases act(p(hgnc:9585 ! PTCH1))

# short form
p(fplx:Hedgehog) =| act(p(hgnc:9585 ! PTCH1))
```

#### Example - Transcription Factors

In the case of transcriptional activity, if the protein performing the transcriptional activity interacts with the gene that the RNA is transcribed from, the relationship is considered direct. For example, repression of the transcription of miR-21 by FOXO3 protein transcriptional activity is represented as direct because FOXO3 binds the miR-21 promoter:

```text
act(p(hgnc:3821 ! FOXO3), ma(tscript)) =| r(hgnc:31586 ! MIR21)
```

#### Example - Self-referential relationships

Self-referential causal relationships are generally represented as direct. For example, phosphorylation of GSK3B at serine 9 inhibiting the kinase activity of GSK3B can be represented as:

```text
p(hgnc:4617 ! GSK3B, pmod(Ph, Ser, 9)) =| act(p(hgnc:4617 ! GSK3B), ma(kin))
```

### Rate Limiting Step

For process, activity, or transformation term A and process term P, `A rateLimitingStepOf P` indicates both:

```text
A partOf bp(B)
A -> bp(B)
```

For example, the catalytic activity of HMG CoA reductase is a rate-limiting step for cholesterol biosynthesis:

```text
act(p(hgnc:5006 ! HMGCR), ma(go:0003824 ! "catalytic activity")) rateLimitingStepOf bp(go:0006695 ! "cholesterol biosynthetic process")
```

### Causes No Change

For terms A and B, `A causesNoChange B` or `A cnc B` indicate that B was observed not to change in response to changes in A.

Statements using this relationship correspond to cases where explicit measurement of B demonstrates lack of significant change, not for cases where the state of B is unknown.

## Correlative Relationships

These relationship types link abundances and biological processes when no causal relationship is known. The order of subject and object terms does not matter in a statement with a correlative relationship, unlike a statement with a causal relationship.

### negativeCorrelation, neg

For terms A and B, `A negativeCorrelation B` or `A neg B` indicates that changes in A and B have been observed to be negatively correlated. The order of the subject and object does not affect the interpretation of the statement, thus `B negativeCorrelation A` is equivalent to `A negativeCorrelation B`.

### positiveCorrelation, pos

For terms A and B, `A positiveCorrelation B` or `A pos B` indicates that changes in A and B have been observed to be positively correlated. The order of the subject and object does not affect the interpretation of the statement, thus `B positiveCorrelation A` is equivalent to `A positiveCorrelation B`.

### correlation, cor

For terms A and B that are stated to be correlated, but not signified whether it is positive or negative. This is more specific than the [association](03_relationships.md#association) relationship.

Will be added in BEL v2.3 with [BEP-0012](https://github.com/belbio/bep/pull/33).

### noCorrelation

For terms A and B that have been measured to not be correlated. The lack of correlation between A and B can also be used to infer a lack of causation.

Added in BEL v2.1 with [BEP-0003](http://bep.bel.bio/published/BEP-0003.html)

## Associative Relationships

### Association

For terms A and B, `A association B` or `A -- B` indicates that A and B are associated in an unspecified manner. This relationship is used when not enough information about the association is available to describe it using more specific relationships, like `increases` or `positiveCorrelation`. The order of the subject and object does not affect the interpretation of the statement, thus `B -- A` is equivalent to `A -- B`.

A common example of association relationships is from literature co-occurrence, such as genes being annotated to diseases.

## Genomic Relationships

These relationship types link related terms, like orthologous terms from two different species or the `geneAbundance()` and `rnaAbundance()` terms for the same namespace value.

In most cases, these relationships will be introduced by the BEL Namespace resources, and are not needed for creation of BEL Statements and BEL Documents.

### orthologous

For terms A and B, `A orthologous B` indicates that A and B represent entities in different species which are sequence similar and which are therefore presumed to share a common ancestor. For example,

```text
g(hgnc:391 ! AKT1) orthologous g(mgi:87986 ! Akt1)
```

indicates that the mouse and human AKT1 genes are orthologs.

### Transcription

For RNA abundance term R and gene abundance term G, `G transcribedTo R` or `G :> R` indicates that members of R are produced by the transcription of members of G. For example:

```text
g(hgnc:391 ! AKT1) :> r(hgnc:391 ! AKT1)
```

indicates that the human AKT1 RNA is transcribed from the human AKT1 gene.

### Translation

For RNA abundance term R and protein abundance term P, `R translatedTo P` or `R P` indicates that members of P are produced by the translation of members of R. For example:

```text
# long form
r(HGNC:AKT1) translatedTo p(HGNC:AKT1)

# short form
r(HGNC:AKT1) >> p(HGNC:AKT1)
```

## Ontological Relationships

In most cases, these relationships will be introduced by the BEL Namespace resources, and are not needed for creation of BEL Statements and BEL Scripts.

### isA

For terms A and B, `A isA B` indicates that A is a subset of B.

All terms in BEL 1.0 represent classes, but given that classes implicitly have instances, `A isA B` is interpreted to mean that any instance of A must also be an instance of B. This relationship can be used to represent GO and MeSH hierarchies:

```text
pathology(MESH:Psoriasis) isA pathology(MESH:"Skin Diseases")
```

### partOf

For terms A and B, `A partOf B` indicates that A is a part of B.

```text
p(HGNC:CHUK)  partOf complex(GO:"IkappaB kinase complex")
p(HGNC:IKBKB) partOf complex(GO:"IkappaB kinase complex")
p(HGNC:IKBKG) partOf complex(GO:"IkappaB kinase complex")
```

## Deprecated Relationships

These BEL v1.0 relationships are supported in BEL v2.0, but are slated to be removed in the next major version.

### analogous

For terms A and B, `A analogousTo B` indicates that A and B represent abundances or molecular activities which function in a similar manner, but do not share sequence similarity or a common ancestor.

### biomarkerFor

For term A and process term P, `A biomarkerFor P` indicates that changes in or detection of A is used in some way to be a biomarker for pathology or biological process P.

### prognosticBiomarkerFor

For term A and process term P, `A prognosticBiomarkerFor P` indicates that changes in or detection of A is used in some way to be a prognostic biomarker for the subsequent development of pathology or biological process P.

### hasMember

For term abundances A and B, `A hasMember B` designates B as a member class of A. A member class is a distinguished sub-class. A is defined as a group by all of the members assigned to it. The member classes may or may not be overlapping and may or may not entirely cover all instances of A. A term may not appear in both the subject and object of the same hasMember statement.

### hasMembers

The `hasMembers` relationship is a special form which enables the assignment of multiple member classes in a single statement where the object of the statement is a set of abundance terms. A statement using `hasMembers` is exactly equivalent to multiple `hasMember` statements. A term may not appear in both the subject and object of the same `hasMembers` statement.

For the abundance terms A, B, C and D, `A hasMembers list(B, C, D)` indicates that A is defined by its member abundance classes B, C and D.

### hasComponent

For complex abundance term A and abundance term B, `A hasComponent B` designates B as a component of A, that complexes that are instances of A have instances of B as possible components. Note that, the stoichiometry of A is not described, nor is it stated that B is a required component. The use of `hasComponent` relationships is complementary to the use of functionally composed complexes and is intended to enable the assignment of components to complexes designated by names in external vocabularies. The assignment of components can potentially enable the reconciliation of equivalent complexes at knowledge assembly time.

### hasComponents

The `hasComponents` relationship is a special form which enables the assignment of multiple complex components in a single statement where the object of the statement is a set of abundance terms. A statement using `hasComponents` is exactly equivalent to multiple `hasComponent` statements. A term may not appear in both the subject and object of the same +hasComponents+ statement.

For the abundance terms A, B, C and D, `A hasComponents list(B, C, D)` indicates that A has components B, C and D.

### subProcessOf

For process, activity, or transformation term A and process term P, `A subProcessOf P` indicates that instances of process P, by default, include one or more instances of A in their composition. For example, the reduction of HMG-CoA to mevalonate is a subprocess of cholesterol biosynthesis:

```text
rxn( \
 reactants(a(CHEBI:"(S)-3-hydroxy-3-methylglutaryl-CoA"), a(CHEBI:NADPH), a(CHEBI:hydron)), \
 products(a(CHEBI:mevalonate), a(CHEBI:"CoA-SH"), a(CHEBI:"NADP(+)")) \
) subProcessOf bp(GOBP:"cholesterol biosynthetic process")
```

