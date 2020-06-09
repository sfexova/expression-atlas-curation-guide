# Curation Best Practice Guide

This is a collection of rules for curating the biological or sample metadata of an Expression Atlas experiment, 
hence the rules apply to all types of experiments (microarray, sequencing, proteomics). 
The main purpose is to make sure curation is consistent between curators and terms can be mapped to ontologies, 
which helps to retrieve experiment by metadata searches and makes visualisations in Expression Atlas clean and consistent 
(e.g. a list of organism parts doesn't contain duplicates with slightly different terms). 

## General rules

These rules are basic curation style guidelines and apply to virtually all experiments. 

#### **Keep values simple.** 
 For sample characteristics and factors, keep the values as concise as possible. 

#### **Single concept in a value.** 
Split values in sample characteristics and factors or else it will be very hard to map the values cleanly to EFO terms.

"Characteristics [genotype] => wild type C57BL/6J" must be annotated as:

    Characteristics [strain] => C57BL/6J
    Characteristics [genotype] => wild type

"Characteristics [organism part] => murine bone marrow" must be annotated as:

    Characteristics [organism] => Mus musculus
    Characteristics [organism part] => bone marrow

"Characteristics [cell type] => thymic epithelial cell" must be annotated as:

    Characteristics [organism part] => thymus
    Characteristics [cell type] => epithelial cell

#### Write terms in EFO style.
Lower case, use spaces (not underscores) between words, and singular. 
E.g. `hour` not `hours`, `embryonic stem cell` not `Embryonic Stem Cell`.

#### Avoid acronyms.
Especially for gene names and disease names, except for terms which mean the same thing across disciplines, 
e.g. DNA, RNA, PBS (for phosphate buffered saline), SDS (for sodium dodecyl sulphate), EDTA (chelating agent), 
DMEM, RPMI (the latter two are common cell culture media). 

`NA` for blank fields is also very problematic, since it can mean either not applicable or not available.

Here are some examples of spelling out terms in full: 

    Characteristics [compound] => ANIT should be 1-naphthyl isothiocyanate
    Characteristics [compound] => AIR should be 5-amino-1-(5-phospho-D-ribosyl)imidazole
    Characteristics [compound] => PMA should be phorbol 13-acetate 12-myristate

Other common cases of acronyms:

    MEF => mouse embryonic fibroblast 
    TGF-beta => transforming growth factor beta
    IGF-1 => insulin-like growth factor 1
    TNF => tumor necrosis factor
    DMSO => dimethyl sulfoxide


#### Use characteristics and factor types from the allowed list.
List is specified as `atlas_property_types` in perl-modules GitHub in the file 
[ae_atlas_controlled_vocabulary.yml](https://github.com/ebi-gene-expression-group/perl-atlas-modules/blob/7c8c05a577b68f6433ec5b19f6fe896d94a5e478/supporting_files/ae_atlas_controlled_vocabulary.yml). 

For example, terms like sample lot number and time unit are not really attributes of samples but are technical information of samples, or simply measurement units.

#### Age ranges.
If age is a range use 'to' between the values rather than a dash e.g. `3 to 5` not `3-5`.

#### Make sure your annotation is not ambiguous. 
Those are examples of ambiguous annotations that you should avoid:

    Characteristics [treatment] => limited
    Characteristics [cell type] => resistant
    Characteristics [stimulus] => stimulated

#### Consider intent of experiment when adding factors. 
Choose which characteristics should be factors by looking at the intent of the experiment, not just what is variable. E.g. A clinical study may have included samples healthy individuals and patients from both sexes, but the researcher may not interested in sex-specific differences between the healthy and affected individuals; including samples from both sexes may be simply for eliminating sex-specific bias in data analysis.

#### Use agreed control terms. 
The allowed values should align with the values recognised by the atlas configuration generation script 
as "reference" (listed in atlas-prod config file [mapped_reference_assay_group_factor_values.xml](https://github.com/ebi-gene-expression-group/atlas-prod/blob/develop/analysis/differential/mapped_reference_assay_group_factor_values.xml)).
For empty terms it is good to have consistency e.g. compound - none; stimulus - none; infect - none.

| Attribute | Values to use |
|:----------|:--------------|
| genotype | wild type genotype |
| phenotype | wild type |
| disease | normal |
| compound / irradiate | none |
| transfection | mock or control or vehicle |
| RNA interference | scrambled siRNA |
| infection | sham or none |
| diet (for mouse) | chow |
| immunoprecipitate | input DNA |
| stimulus | none |
| immunophenotype | live cell or unsorted |




## Rules for specific annotation types

Some experiment designs require special rules to follow to make annotations consistent. 

#### Compound-induced gene expression or repression
E.g. cells carry a construct that causes repression of gene expression when activated by doxycycline. 
Use `Characteristics[genotype]` (and `Factor Value[genotype]` if variable) to annotate with info about the construct. 
Use `Characteristics[phenotype]` (and `Factor Value[phenotype]` if variable) to annotate with info about the
gene expression/repression caused by compound. For example:
| Characteristics[genotype] | Characteristics[phenotype] | ... | FactorValue[phenotype] |
|:--------------------------|:--------------------------|:-----|:-----------------------|
| FOXP3-tet-off	| expression of FOXP3 |	... | expression of FOXP3 |
| FOXP3-tet-off	| doxycycline-mediated repression of FOXP3 | ... | doxycycline-mediated repression of FOXP3 |


#### Chemical compounds
We use factor values `compound` and `dose` for any chemical. `Stimulus` is the preferred term for peptide and protein 
stimulations (e.g. Interleukin-2). Compound should always be accompanied by the dose and its unit. 
The compound should be found in CHEBI ontology and the unit must be in EFO.

| Factor Value[compound] | Factor Value[dose] | Unit[concentration unit] |
|:-----------------------|:-------------------|:-------------------------|
| phorbol 13-acetate 12-myristate | 10 | millimolar |
	

If we have multiple compounds as a single factor, we can combine them into a single value under "compound" (and include the respective dose), for example `bovine serum albumin; 0.06 millimolar and sodium hydroxide; 0.4 millimolar`. The same applies for two doses of the same compound:  methotrexate; first dose 120 milligram per kilogram; second dose 60 milligram per kilogram. Leave the dose factor blank in such a case. 


#### Normal and tumour samples

If a study involves both normal and tumor samples from the same patient, this should be coded as:
| Characteristics[disease] | Characteristics[individual] | Characteristics[sampling site] |
|:-----------|:-----------|:------------|
| breast cancer	| patient 1 | normal tissue adjacent to neoplasm |
| breast cancer	| patient 1 | neoplasm |
| normal | healthy_guy_X | normal tissue |

`Characteristic[Individual]` is used to show which samples come from the same patient. 
Disease refers to the disease of the donor and will be `cancer` even for the sample from the healthy tissue. 


#### Organism parts and cell types not in ontology

Generally use the main ontology label, not the synonyms and the most specific term, e.g. `heart ventricle` instead of `heart`.
If the term is not in any ontology, look up the [previous zooma mapping](https://github.com/ebi-gene-expression-group/curated-metadata).


#### Technical annotations

Sometime submitters add technical information, such as `replicate`, `sequencing batch`, `run date` etc. 
These values should not be sample characteristics or factors for Atlas experiments. 
To keep the information in the sample annotation, turn the attributes into Comments 
(`Characteristics[replicate] => Comment[replicate]`).


