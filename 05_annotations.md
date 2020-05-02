# BEL Annotations

## BEL Statement Annotation Examples

Annotations associate context information with BEL Statements, including citation of the source material, evidence text supporting the statement, and the experimental context for the scientific observations represented by the statement. To associate Annotations with statements, Annotations are `SET` and `UNSET` within a BEL Document. In the BEL Script syntax, once an Annotation has been `SET` all following statements inherit the annotation until is explicitly `UNSET` or a new Annotation of the same type is `SET`.

* &lt;&gt;
* &lt;&gt;
* &lt;&gt;
* &lt;&gt;

### Citation

Citations are a special type of annotation that references the knowledge source that reports the observation that the statement is based on. Citations are composed of a document type, a document name, a document reference ID, and an optional publication date, authors list and comment field. For example, the citation for a journal article indexed by PubMed would be encoded as:

SET Citation = {"PubMed", "Genes Cancer. 2010 Jun;1\(6\):560-567.", "21533016"}

The document name is a text string containing the reference information, the type is PubMed, and the document reference is the PubMed ID.

The citation for a Reactome pathway would be encoded as:

SET Citation = {"Online Resource", "p53-Dependent G1 DNA Damage Response", "REACT\_1625.1"}

In this case, the document name is the pathway name, the type is _Online Resource_, and the reference is the Reactome identifier.

### Support \(previously known as Supporting Text\)

Support annotations provide the specific text that the statement is derived from. Text should come directly from the abstract or full text of the source referenced by the citation annotation. For example, a support line from the Reactome pathway cited above is:

SET Support ="The p53 protein activates the transcription of cyclin-dependent kinase inhibitor, p21. p21 inactivates the CyclinE:Cdk2 complexes, and prevent entry of the cell into S phase, leading to G1 arrest."

### Species

Species annotations indicate the species context for experimental observation represented by the statement. It is good practice to unambiguously assign species context to BEL Statements, even though many BEL Terms are derived from a species-specific namespace \(e.g., HGNC, MGI, RGD\). Species annotation uses the [http://www.ncbi.nlm.nih.gov/Taxonomy/taxonomyhome.html/\[NCBI](http://www.ncbi.nlm.nih.gov/Taxonomy/taxonomyhome.html/[NCBI) taxonomy ID\]:

SET Species = "9606"

Sets the species as Homo sapiens.

SET Species = "10090"

Sets the species as Mus musculus

SET Species = "10116"

Sets the species as Rattus norvegicus.

### Other Annotation Types

Other types of annotations can be added to statements to indicate the context of the experimental observation supported by the statement, including cell line, cell type, and cellular location. For example:

SET Cell = "Adipocytes, White" SET CellLine = "LoVo" SET Disease = "Lupus Erythematosus, Systemic" SET Anatomy = "Pulmonary Artery"

\[TIP\]

In a BEL Document each Annotation Type that will be used, except for Citation and SupportingText, must be defined in the document header, along with the values allowed for each.

