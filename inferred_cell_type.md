# Inferred cell type curation rules

This is a general guideline on how we curate inferred cell types for SCEA, at the moment SCEA published two types of inferred cell type (ICT) annotations on the web - *inferred cell type - ontology labels* and *inferred cell type - authors labels*. How they these are technically captured in files differs for plate-based and droplet-based experiments (as the former has sdrf demultiplexed by cell while the latter does not) but the curation rules are the same for both.

**Always keep the absolute untouched original annotations exactly as they came from the authors in a separate column from the two ICT columns** (see below for details)

In this guide:
1. Curation rules
   - Inferred cell type - ontology labels
   - Inferred cell type - authors labels
2. Technical implementation
	 - plate-based experiments
	 - droplet-based experiments
	 - untouched absolute original annotations

## 1. Curation rules
### Inferred cell type - ontology labels
These are ontologised cell type annotations - map to a matching ontology term (from EFO/CL/UBERON or one of the approved specialist model organism/plant ontologies) or a parent ontology term if a matching term is not available and for some reason it is not advisable to create one. Use caution and your best judgment to decide when this still makes sense biologically and when the nearest available parent term is too distant/confounding.

For example:

It is reasonable to map all these to this one term:

But in other cases ontologization would mean flattening the data too much and binning very distinct cell types (even across experiments) together if the nearest available ontology term is quite general:

In such cases we forego ontologization as it would basically obscure all the data and just curate for style, conciseness etc as best as possible (SCEA cannot chain multiple ontology terms together, only one term has to be chosen.) In the example above, since the information provided didn’t appear sufficient to justify creating new terms such as ‘primary visual area 1-4 GABAergic neuron’, the choice would be to annotate these with the nearest ontology terms matching either the location - primary visual cortex - or the cell type - GABAergic neuron. Neither of these two options is satisfactory and sufficient, therefore SCEA would not ontologize these annotations and leave them as text strings. In the ontologized [inferred cell type - ontology labels] column we do, however, omit any marker gene annotations. The idea is that although SCEA cannot chain ontology labels and any such annotations will effectively become just text-strings, in the [inferred cell type - ontology labels] we only use ontology concepts - even if these are just component parts of what will end up being read as a text string by the pipelines. In this particular case we would therefore keep the location annotation and the cell type annotation in the [inferred cell type - ontology labels] but without the marker gene (and as concise as possible) > e.g. ‘primary visual cortex layer 5 and 6 GABAergic neuron’.

Take care with ‘xy-like cell’ cell type annotations - we do not map them to their respective ‘xy’ terms as it completely equates two cell types that may be similar but are still biologically distinct (NK-like cell, although the authors obviously discovered some similarity, is still NOT an NK cell - or it would have been annotated as such - so it would be giving the users wrong and misleading information to label it with the ‘natural killer cell’ term). These will have to remain unmapped (until/unless the cell type is better defined and receives a fitting term of its own).

‘Unknown' type of values (like 'not specified', 'N/A' etc), need to be replaced with blanks in the [inferred cell type - ontology labels] column. The web prod pipeline will then interpret these as 'false' and automatically assign the label 'not available' for web display. Please note that this is just for the [inferred cell type - ontology labels] column, not the [inferred cell type - authors labels].

### Inferred cell type - authors labels
These are authors' original annotations edited for style and consistency - can expand acronyms if appropriate/reasonable, but more stylistic prettifying than deep curation (no need to spend tons of time on this column as one of the reasons for having this is to make it possible for the users to make an easy connection between the annotations here and the terminology used in the publication, so we do not want to depart too far from what the authors supplied).
