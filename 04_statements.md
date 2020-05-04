# BEL Statements

## Edge Modifiers

### Molecular Activity

`activity(<abundance>)` or `act(<abundance)` is used to specify events resulting from the molecular activity of an abundance. The `activity()` function provides distinct terms that enable differentiation of the increase or decrease of the molecular activity of a protein from changes in the abundance of the protein. `activity()` can be applied to a protein, complex, or RNA abundance term, and modified with a ``molecularActivity()` argument to indicate a specific type of molecular activity.

```
act(p(HGNC:AKT1))
```

#### Process Modifier Function

`molecularActivity(ns:v)` or `ma(ns:v)` is used to denote a specific type of activity function within an `activity()` term.

NOTE - The default BEL namespace (DEFAULT) includes commonly used molecular activity types, mapping directly to the BEL v1.0 activity functions.

##### Examples

###### default BEL namespace, transcriptional activity (DEFAULT namespace is optional)

```
 act(p(HGNC:FOXO1), ma(DEFAULT:tscript))
```

###### GO molecular function namespace, transcriptional activity
```
 act(p(HGNC:FOXO1), ma(GO:"nucleic acid binding transcription factor activity"))
```
###### default BEL namespace, kinase activity
```
 act(p(HGNC:AKT1), ma(kin))
```
###### GO molecular function namespace, kinase activity
```
 act(p(HGNC:AKT1), ma(GO:"kinase activity"))
```
##### Activity Term Examples

Term activity functions are applied to protein, complex, and RNA abundances to specify the frequency of events resulting from the molecular activity of the abundance. This distinction is particularly useful for proteins whose activities are regulated by post-translational modification. Specific activity types can be indicated using the `<<XmolecularA, molecularActivity()>>` process modifier function. The default BEL namespace includes molecular activity values corresponding to the http://wiki.openbel.org/display/BLD/Activities[BEL v1.0 activity functions], and http://geneontology.org/page/molecular-function-ontology-guidelines[GO Molecular Function] namespace values can be used to indicate more specific molecular activities.

* <<Non-Specified Activities>>
* <<Catalytic Activity>>
* <<Peptidase Activity>>
* <<G-proteins in the active (GTP-bound) state>>
* <<Transporter Activity>>
* <<Chaperone Activity>>
* <<Transcription Activity>>

###### Non-Specified Activities

If the type of molecular activity is not reported, it does not need to be specified. The `activity()` function is sufficient for distinguishing the frequency of events mediated by an abundance from the amount of the abundance. This term represents the ligand-bound activity of the human non-catalytic receptor protein TLR7.

###### Long Form
```
 activity(proteinAbundance(HGNC:TLR7))
```
###### Short Form
```
 act(p(HGNC:TLR7))
```
###### Catalytic Activity

A protein, complex, or ribozymes has catalytic activity when it acts as an enzymatic catalyst of biochemical reactions. Catalytic activity includes kinase, phosphatase, peptidase, and ADP-ribosylase activities, though these can be represented by more specific molecular activity terms.

This term represents the frequency of events in which the protein abundance of rat Sod1 acts as a catalyst.

###### Long Form - default BEL namespace
```
 activity(proteinAbundance(RGD:Sod1), ma(cat))
```
###### Long Form - GO Molecular Function (GOMF) namespace
```
 activity(proteinAbundance(RGD:Sod1), molecularActivity(GO:"catalytic activity"))
```
###### Short Form - default BEL namespace
```
 act(p(RGD:Sod1), ma(cat))
```
###### short Form - GO Molecular Function namespace
```
 act(p(RGD:Sod1), ma(GO:"catalytic activity"))
```
###### Peptidase Activity

This term represents the frequency of events in which the protein abundance of mouse Casp3 acts as a peptidase.The more specific GO Molecular Function term "cysteine-type endopeptidase activity" is also applicable.

###### Long Form - default BEL namespace
```
 activity(proteinAbundance(MGI:Casp3), molecularActivity(pep))
```
###### Long Form - GO Molecular Function namespace
```
 activity(proteinAbundance(MGI:Casp3), molecularActivity(GO:"peptidase activity"))
```
###### Short Form - default BEL namespace
```
 act(p(MGI:Casp3), ma(pep))
```
###### Short Form - GO Molecular Function namespace
```
 act(p(MGI:Casp3), ma(GO:"peptidase activity"))
```
###### G-proteins in the active (GTP-bound) state

The activity of guanine nucleotide-binding proteins (G-proteins) like RAS in the active, GTP-bound state. This term represents the frequency of events caused by the active, GTP-bound form of the RAS protein family.

###### Long Form - default BEL namespace
```
 activity(proteinAbundance(SFAM:"RAS Family"), molecularActivity(gtp))
```
###### Long Form - GO Molecular Function namespace
```
 activity(proteinAbundance(SFAM:"RAS Family"), molecularActivity(GO:"GTP binding"))
```
###### Short Form - default BEL namespace
```
 act(p(SFAM:"RAS Family"), ma(gtp))
```
###### Short Form - GO Molecular Function namespace
```
 act(p(SFAM:"RAS Family"), ma(GO:"GTP binding"))
```
###### Transporter Activity

Molecular translocation events mediated by transporter proteins like ion channels or glucose transporters. This term represents the frequency of ion transport events mediated by the epithelial sodium channel (ENaC) complex.

###### Long Form - default BEL namespace
```
 activity(complexAbundance(SCOMP:"ENaC Complex"), molecularActivity(tport))
```
###### Long Form - GO Molecular Function namespace
```
 activity(complexAbundance(SCOMP:"ENaC Complex"), molecularActivity(GO:"transporter activity"))
```
###### Short Form - default BEL namespace
```
 act(complex(NCH:"ENaC Complex"), ma(tport))
```
###### Short Form - GO Molecular Function namespace
```
 act(complex(NCH:"ENaC Complex"), ma(GO:"transporter activity"))
```
###### Chaperone Activity

This term represents the events in which the human Calnexin protein functions as a chaperone to aid the folding of other proteins.

###### Long Form - default BEL namespace
```
 activity(proteinAbundance(HGNC:CANX), molecularActivity(chap))
```
###### Short Form - default BEL namespace
```
 act(p(HGNC:CANX), ma(chap))
```
###### Transcription Activity

Events in which a protein or molecular complex acts to directly control transcription, including proteins acting directly as transcription factors, as well as transcriptional co-activators and co-repressors. This term represents the frequency of events in which the mouse p53 protein controls RNA expression.

###### Long Form - default BEL namespace
```
 activity(proteinAbundance(MGI:Trp53), molecularActivity(tscript))
```
###### Long Form - GO Molecular Function Namespace
```
 activity(proteinAbundance(MGI:Trp53), molecularActivity(GO:"nucleic acid binding transcription factor activity"))
```
###### Short Form - default BEL namespace
```
 act(p(MGI:Trp53), ma(tscript))
```
###### Long Form - GO Molecular Function Namespace
```
 act(p(MGI:Trp53), ma(GO:"nucleic acid binding transcription factor activity"))
```

### Transformation Functions

The following BEL functions represent transformations. Transformations are processes or events in which one class of abundance is transformed or changed into a second class of abundance by translocation, degradation, or participation in a reaction. All types of abundance terms except compositeAbundance() may be used within these transformation functions.

#### Translocations

BEL translocation functions include `translocation()` as well as `cellSurfaceExpression()` and `cellSecretion()`, two functions intended to provide a simple, standard means of expressing commonly represented translocations.

For the abundance term A, `translocation(<abundance>, fromLocation(ns1:v1), toLocation(ns2:v2))` or `tloc(<abundance>, fromLoc(ns1:v1), toLoc(ns2:v2))` denotes the frequency or number of events in which members of `<abundance>` move from the location designated by the value +v1+ in the namespace +ns1+ to the location designated by the value `v2` in the namespace `ns2`. Translocation is applied to represent events on the cellular scale, like endocytosis and movement of transcription factors from the cytoplasm to the nucleus.  Special case translocations are handled by the BEL functions: `cellSecretion()`, `cellSurfaceExpression()`.

##### Example

endocytosis (translocation from the cell surface to the endosome) of the epidermal growth factor receptor (EGFR) protein can be represented as:
```
 tloc(p(HGNC:EGFR), fromLoc(GOCC:"cell surface"), toLoc(GOCC:endosome))
```
#### Cellular Secretion
```
cellSecretion(), sec()
```

For the abundance term A, `cellSecretion(<abundance>)` or `sec(<abundance>)` denotes the frequency or number of events in which members of `<abundance>` move from cells to regions outside of the cells. `cellSecretion(<abundance>` can be equivalently expressed as:
```
 tloc(<abundance>, fromLoc(GOCC:intracellular), toLoc(GOCC:"extracellular space"))
```
The intent of the `cellSecretion()` function is to provide a simple, standard means of expressing a commonly represented translocation.

#### Cellular Surface Expression

`cellSurfaceExpression(<abundance>)` or `surf(<abundance>)` denotes the frequency or abundance of events in which members of `<abundance>` move to the surface of cells. `cellSurfaceExpression(<abundance>)` can be equivalently expressed as:
```
 tloc(<abundance>, fromLoc(GOCC:intracellular), toLoc(GOCC:"cell surface"))
```
The intent of the `cellSurfaceExpression()` function is to provide a simple, standard means of expressing a commonly represented translocation.

###### Translocations

Translocations, or the movement of abundances from one location to another, are represented in BEL Terms by the `translocation()` or `tloc()` function. For convenience, the frequently used translocations of abundances from inside the cell to cell surface or extracellular space are represented by the `cellSurface()` and `cellSecretion()` functions, respectively.

###### Example

This term represents the event in which human NFE2L2 protein is translocated from the cytoplasm to the nucleus.

###### Long Form
```
 translocation(proteinAbundance(HGNC:NFE2L2), fromLoc(MESHCS:Cytoplasm), toLoc(MESHCS:"Cell Nucleus"))
```
###### Short Form
```
 tloc(p(HGNC:NFE2L2), fromLoc(MESHCL:Cytoplasm), toLoc(MESHCL:"Cell Nucleus"))
```
###### Example - cell secretion

This term represents secretion of mouse IL6 protein.

###### Long Form
```
 cellSecretion(proteinAbundance(MGI:Il6))
```
###### Short Form
```
 sec(p(MGI:Il6))
```
###### Example - cell surface expression

This term represents cell surface expression of rat Fas protein.

###### Long Form
```
 cellSurfaceExpression(proteinAbundance(RGD:Fas))
```
###### Short Form
```
 surf(p(RGD:Fas))
```


### Degradation

Events in which an abundance is degraded can be represented by the `degradation()` or `deg()` function.

`degradation(<abundance>)` or `deg(<abundance>)` denotes the frequency or number of events in which a member of `<abundance>` is degraded in some way such that it is no longer a member of `<abundance>`. For example, `degradation()` is used to represent proteasome-mediated proteolysis. The BEL Framework automatically connects +deg(<abundance>)+ to `<abundance>` such that:
```
 deg(<abundance>) directlyDecreases <abundance>
```

###### Example

This term represents the degradation of MYC RNA. Degradation decreases the amount of the abundance - when degradation statements are compiled, a directlyDecreases relationship edge is added between the degradation term and the degraded entity.

###### Long Form
```
 degradation(rnaAbundance(HGNC:MYC))
```
###### Short Form
```
 deg(r(HGNC:MYC))
```

### Cellular Location

`location()` or `loc()` can be used as an argument within any abundance
function except `compositeAbundance()` to represent a distinct subset of the
abundance at that location. Location subsets of abundances have the general
form:

```
f(ns:v, loc(ns:v))
```

##### Cytoplasmic pool of AKT1 protein

```
p(hgnc:391 ! AKT1, loc(MESHCS:Cytoplasm))
```

##### Endoplasmic Reticulum pool of Ca^2+^

```
a(CHEBI:"calcium(2+)", loc(GO:"endoplasmic reticulum"))
```




## Nested Statements

BEL Terms are denoted by expressions composed of a BEL Function and a list of
arguments. BEL v2.0 specifies a set of approximately 20 functions allowed in
term expressions.

The combination of a BEL function and its arguments fully specifies a BEL Term. The BEL Term expression `f(a)` denotes a BEL Term defined by function `f()` applied to an argument `a`. Wherever the same function is applied to the same arguments, the resulting BEL Term references the same biological entity.

The semantics of a BEL Term are determined by the function used in the term expression. For example, the function `proteinAbundance()` is defined such that any term expression using `proteinAbundance()` represents a class of abundance of protein. Many BEL functions take only single values as arguments, providing a structured method of using ontologies and vocabularies in BEL. For example, values in the HUGO Gene Nomenclature Committee (HGNC) vocabulary of official human gene symbols can be used to designate gene, RNA, and protein abundances. The function `proteinAbundance()` could then be applied to an HGNC gene symbol, __AKT1__ for example, to indicate the class of protein abundances produced by the AKT1 gene, producing the BEL Term `proteinAbundance(HGNC:AKT1)`.


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
