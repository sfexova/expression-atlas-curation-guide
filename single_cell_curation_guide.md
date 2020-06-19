# Single Cell Curation Guide

* [SDRF fields](#SDRF-file)
* [IDF fields](#IDF-file)
* [Single cell quality control](#single-cell-quality-control)
* [Cell type](#cell-type-and-inferred-cell-type)
* [Experiments using spike-in RNAs](#experiments-using-spike-in-rnas)
* [Experiments with multiplexed data files (10x, Drop-seq)](#experiments-with-multiplexed-data-files-10x-Drop-seq)
* [Cell-level metadata for droplet-based experiments](#cell-level-metadata-for-droplet-based-experiments)


## SDRF file


Single-cell experiments require special Comments fields in the SDRF so that 
the single-cell processing pipeline can analyse them correctly.

If a field does not apply to a particular experiment it should be not included in the SDRF.

Currently, we are processing smart-like, smart-seq2; Drop-seq and 10xV2/10xV3 experiments.


### SDRF fields that all experiments should have

**single cell quality**
```
OK
OK, filtered
not OK
```

**single cell isolation** ([OBI_0000512](https://www.ebi.ac.uk/ols/ontologies/efo/terms?iri=http%3A%2F%2Fpurl.obolibrary.org%2Fobo%2FOBI_0000512))

The method that was used to singularise the cells in order to enable individual barcoding. 
```
FACS
Fluidigm C1
10x
Drop-seq
inDrop
Seq-Well
mouth pipette
cell picking
laser-capture microdissection
other
```

**library construction** ([EFO_0010183](https://www.ebi.ac.uk/ols/ontologies/efo/terms?iri=http%3A%2F%2Fwww.ebi.ac.uk%2Fefo%2FEFO_0010183&viewMode=All&siblings=false))

The general method that was followed to generate single cell libraries.
```
smart-like*
smart-seq
smart-seq2
10xV1
10xV1a
10xV1i
10xV2
10xV3
drop-seq
seq-well
SCRB-seq	
MARS-seq
CEL-seq
CEL-seq2
STRT-seq
other
```
*smart-like → Libraries prepared with SMARTer Ultra Low RNA Kit for Illumina Sequencing (Clontech Cat#634936) can be also analysed with "smart-seq2" pipeline

**end bias** ([EFO_0010187](https://www.ebi.ac.uk/ols/ontologies/efo/terms?iri=http%3A%2F%2Fwww.ebi.ac.uk%2Fefo%2FEFO_0010187))

If the single cell library method has a bias towards reads from one end of the RNA molecule. 
E.g. the 10x method does only sequence the 3' end of the mRNA, (so we use "3 prime tag"), 
whereas the Smart-seq2 method generally has no bias and generates reads throughout the full mRNA molecule 
("end bias: none").
```
none
3 prime tag
5 prime tag
3 prime end bias
5 prime end bias
```

**input molecule**

Child term from "biological macromolecule" EFO_0004446
```
polyA RNA
non polyA RNA
total RNA
```

**primer** ([EFO_0010192](https://www.ebi.ac.uk/ols/ontologies/efo/terms?iri=http%3A%2F%2Fwww.ebi.ac.uk%2Fefo%2FEFO_0010192))

The primer used for cDNA synthesis from RNA.
```
oligo-dT
random
```

**library strand**
```
second strand
first strand
not applicable
```

**spike in** ([EFO_0010193](https://www.ebi.ac.uk/ols/ontologies/efo/terms?iri=http%3A%2F%2Fwww.ebi.ac.uk%2Fefo%2FEFO_0010193))
```
ERCC
ERCC mix1
ERCC mix2
ERCC mix1 and mix2
```

**spike in dilution** ([EFO_0010217](https://www.ebi.ac.uk/ols/ontologies/efo/terms?iri=http%3A%2F%2Fwww.ebi.ac.uk%2Fefo%2FEFO_0010217))
```
e.g. 1:40000
```


**Comment[technical replicate group]**

The values will be the BioSample accession ("SAMEA..."). If there are no BioSamples accessions, 
use a pattern like "cell1", "cell2", ... because these labels will be displayed in the interface, 
for example when you hover over a cell in the tSNE plot.


In addition to the fields above, also the regular ENA/SRA information is required:

**Comment[ENA_RUN]** or **Comment[RUN]**
Not all experiments have ENA run accessions. To include these cases, we've decided to use the Comment[RUN] if there is no ENA_RUN comment. It should contain a unique run ID, e.g. custom run identifiers like "run 1", "run 2", "run 3", etc.

**Sequencing platform and model**
Hardware of the "nucleic acid sequencing protocol"

**Sequencing centre**
Performer of the "nucleic acid sequencing protocol"

**Factor Value[single cell identifier]**

Should be used as a experimental variable for all experiments that have individual cells as samples (i.e. not applicable for 10x/droplet-sequencing).



## IDF file


### Experiment type

All scRNA-seq experiments must be annotated as 
"[RNA-seq of coding RNA from single cells](https://www.ebi.ac.uk/ols/ontologies/efo/terms?iri=http%3A%2F%2Fwww.ebi.ac.uk%2Fefo%2FEFO_0005684)" under Comment[AEExperimentType]. 


### Protocols

The following protocols should be included:

| Protocol Type |  EFO accession | Description | 
|---|---|---| 
| **sample collection protocol** | [EFO_0005518](https://www.ebi.ac.uk/ols/ontologies/efo/terms?iri=http%3A%2F%2Fwww.ebi.ac.uk%2Fefo%2FEFO_0005518) | Describes the procedure whereby biological samples for an experiment are sourced |
| **dissection protocol** | [EFO_0005519](https://www.ebi.ac.uk/ols/ontologies/efo/terms?iri=http%3A%2F%2Fwww.ebi.ac.uk%2Fefo%2FEFO_0005519) | Describes the procedure which dissects biological materials into anatomical sub-components, e.g. specific organs or tissues |
| **single cell isolation protocol** | [EFO_0010214](https://www.ebi.ac.uk/ols/ontologies/efo/terms?iri=http%3A%2F%2Fwww.ebi.ac.uk%2Fefo%2FEFO_0010214) | Describes the procedure whereby single cells are isolated |
| **single cell nucleic acid library construction protocol** | [EFO_0010213](https://www.ebi.ac.uk/ols/ontologies/efo/terms?iri=http%3A%2F%2Fwww.ebi.ac.uk%2Fefo%2FEFO_0010213) | Describes the procedure which generates single cell sequencing libraries |
| **single cell nucleic acid sequencing protocol** | [EFO_0008439](https://www.ebi.ac.uk/ols/ontologies/efo/terms?iri=http%3A%2F%2Fwww.ebi.ac.uk%2Fefo%2FEFO_0008439) | Describes the processes performed and equipment used to determine the order of nucleotide bases in a nucleic acid sample from a single cell specimen. |


### Expression Atlas comments

**Comment[EAExpectedClusters]** 

For the visualisation of the data in single-cell Atlas, clustering is performed. For the algorithm to quickly find a reasonable number of clusters, we enter the expected number of clusters in the dataset. Since this on the level of experiment (not by library), we use a comment in the IDF file Comment[ExpectedClusters] and put the number of clusters. The best way of finding an estimate for this number is to check the publication related to the experiment, or asking the submitter (if possible).

**Comment[EAExperimentType]**

This has two fields the first with only two possible values: baseline or differential. The third optional field is trajectory for developmental datasets.

**Comment[EAAdditionalAttributes]**

This should contain the list of additional attributes (which are not already factors) that we would like to display in the portal, e.g. individual, sex, genotype. Tab-separated list (one value per column).

**Comment[EACurator]**

Name of the curator that curated and loaded the experiment into Single Cell Atlas.

**Comment[ExpressionAtlasAccession]**

This field will replace Comment[ArrayExpressAccession] for the experiments that won’t be loaded in ArrayExpress but will be loaded in Single Cell Atlas.



## More explanations and special rules

### Single cell quality control

Submitters usually do quality control (QC) at different levels:

(1) Before sequencing:

This can be visual inspection of the well (commonly used on Fluidigm C1 chips) or knowledge about the well content (e.g. when deliberately more than 1 cell is sorted into each well). We use Characteristics[single cell well quality] to store this kind of information.
The values for this attribute should be "OK" or "not OK" (i.e. the well should be discarded from data analysis). If you have more detailed information about the "bad" wells, the following terms should be used:

    debris
    multiple cells (or number of cells if known)
    dead cell
    no cell

(2) After sequencing:

This is usually information about the sequencing quality of each cell, e.g. if cells were excluded from the analysis due to bad sequencing quality. If the submitter provided such information, we use Characteristics[post analysis well quality] and annotate each cell with "pass" or "fail".

For Atlas processing we also want to filter the cells that were anything other than one single cell from the start. For this we create Comment[single cell quality] and combine the two levels of QC if both are given. We allow the three values:

* **OK**: This cell was identified as a single cell and the sequencing reads passed the submitter's post-analysis quality control
* **OK, filtered**: This cell was identified as a single cell but the sequencing reads did not pass the submitters post-analsysis quality control
* **not OK**: This cell was identified as anything other than a single cell. It does not matter if it passed post-analysis quality control or not.

This means if there are some samples that are not "OK" under "single cell well quality", we annotated them as "not OK" regardless if this sample passed the sequencing QC or not. If there is no "post analysis single cell quality" we only use the values "OK", "not OK" based on the annotation under "single cell well quality". If no information about either pre- or post-sequencing QC is given (and cannot be obtained from the authors) we assume QC for all cells is fine and annotate them as "OK".

(3) Unknown:

If it is unknown at what stage the quality filtering was done, we also use the attribute Characteristics[single cell quality] with the values above.


### Cell type and inferred cell type

This is supplied by the data submitter. If the experiment involves discovering new cell types or pooled cells from a whole organ or organism, the precise cell type might be undefined at the start of the experiment but inferred later during their analysis. Different cell types are inferred based upon their expression profile to other known cell types or known functions.


### Experiments using spike-in RNAs

Spike-in RNAs are sometimes added to single-cell libraries to prevent loss of molecules, for quality control, and/or normalisation. There are two possibilities: (1) commercial spike-in kits or (2) custom spike-in mix. In case of a commercial spike-in mixture, we ask for the name of the kit, and the final dilution factor. We put the details like full name and catalogue number in the library construction protocol and insert the name under the Comment[spike in].

The Smart-seq2 protocol usually includes the commercial ERCC spike-in mix.

Note that in the absence of spike-ins columns should not be provided, or should contain empty values.
Experiments with multiplexed data files (10x, Drop-seq)


### Experiments with multiplexed data files (10x, Drop-seq)

To map each fastq file, we add the following comments in the SDRF and fill in the file name. 
These comments are also used to create BAM files for ENA brokering and Atlas analysis.

| Comment | 10xV1a | 10xV1i | 10xV2 | Drop-seq |
|---|---|---|---|---|
| [read1 file]  | R1 | -RA (one interleaved file with both reads) | R1 | R1 |
| [read2 file] | R3 | -RA (one interleaved file with both reads) | R2 | R2 |
| [index1 file] (if present) | R2 | -I1 | I1 | I1 |
| [index2 file] (if present) | I1 | -I2 | |	
	


Additional info: The file contents for each version of 10x (with default read size and offset):

| Comment | 10xV1a | 10xV1i | 10xV2 | Drop-seq |
|---------|--------|--------|-------|----------|
| [read1 file] | cDNA (98bp, offset 0) | read 1: cDNA (98bp, offset 0) | cell barcode (16bp, offset 0) + UMI (10bp, offset 16) | cell barcode (12bp, offset 0) + UMI (8bp, offset 12) |
| [read2 file] | UMI (10bp, offset 0) | read 2: UMI (10bp, offset 0) | cDNA (98bp, offset 0) | cDNA (50bp, offset 0) | 
| [index1 file] (if present) | cell barcode (14bp, offset 0) | cell barcode (14bp, offset 0) | sample barcode (8bp, offset 0) | | 	
| [index2 file] (if present) | sample barcode (8bp, offset 0) | sample barcode (8bp, offset 0) | | | 



To describe the library layout, i.e. the positions of the different barcodes within the read, we use the following Comments fields in the SDRF:

| Comment | Description | Possible values |
|---------|-------------|---|
| [cDNA read] | the file that contains the cDNA read | index1/index2/read1/read2 |
| [cDNA read offset] | offset in sequence for cDNA read (in bp) | 0 for start of read |
| [cDNA read size] | length of cDNA read (in bp) | |
| [umi barcode read] | the file that contains the UMI barcode read | index1/index2/read1/read2 |
| [umi barcode offset] | offset in sequence for UMI barcode read (in bp) | 0 for start of read |
| [umi barcode size] | length of UMI barcode read (in bp) | 0 for no barcode |
| [umi barcode file]| URI to the file containing known UMI barcodes	||
| [cell barcode read] | the file that contains the cell barcode read | index1/index2/read1/read2 |
| [cell barcode offset] | offset in sequence for cell barcode read (in bp) | 0 for start of read |
| [cell barcode size] | length of cell barcode read (in bp) | 0 for no barcode |
| [cell barcode file] | URI to the file containing known cell barcodes ||
| [sample barcode read] | the file that contains the sample barcode read | index1/index2/read1/read2 |
| [sample barcode offset] | offset in sequence for sample barcode read | 0 for start of read |
| [sample barcode size] | length of sample barcode read (in bp) | 0 for no barcode |
| [sample barcode file] | URI to the file containing known sample barcodes | https://www.ebi.ac.uk/arrayexpress/files/E-MTAB-6153/E-MTAB-6153.additional.1.zip/E-MTAB-6153_sample_barcodes.txt |


For droplet-based experiments, the number of cells per library is not know before the data analysis. Therefore to compare our results with the results from the submitters/authors, we record the number of cells per library in the SDRF with the following comments.
To state the final cell count that was used for further analysis, we use the Comment term "cell count" and "cell count pre filtering" for the number of cells before QC filtering. Both comments are optional.
[cell count] 	Number of cells detected in the library after any filtering steps, i.e. the final number
[cell count pre filtering]	Number of cells detected in the library before quality filtering was applied
Cell-level metadata for droplet-based experiments


## Cell-level metadata for droplet-based experiments

Some droplet-based sequencing submissions will have additional metadata such as inferred cell type. As the curated SDRF is not demultiplexed by cell only by sample, we cannot annotate at the level of the cell. So we need to provide a separate file with additional metadata.

The file should be named experiment-accession.cells.txt (e.g. E-GEOD-130148.cells.txt) and placed in the single cell experiment loading directory along with the IDF/SDRF for analysis.


The cells.txt file should be a tab-separated file with the Cell ID in the first column and annotations in the following columns:

**Cell ID**

This should be a list of the run ID of the library and the cell barcode connected by a hyphen.

It should map to the AssayName found in the first column of the Experiment design file (expanded SDRF for droplet experiments produced by Pablo)

SRR8942433-ATATCCGCCGCA

The run ID of the library shall be taken from the ENA_RUN column in the SDRF if there are no technical replicates.

If there are technical replicates ENA_RUN should be replaced with the value in the technical replicate group column:
usually the BioSampleID, e.g. SAMN123045-AAGCTTACCGG
If there are no BioSampleIDs then these should be mapped to the technical group name i.e. group1-cell barcode (no spaces allowed)

If there are no technical replicates and no ENA_RUN, use the value from the RUN column.

**inferred cell type** (and **authors inferred cell type**)

This should be the curated list of inferred cell types (curated where possible from submitter supplied inferred cell types mapped to EFO terms). The header should be the same as those for sample characteristics of the Experiment design file.

	

