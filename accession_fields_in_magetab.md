# Accession fields in MAGE-TAB

MAGE-TAB files include several fields, usually comments, that contain accessions to data in other primary archives. Here is an overview of all accession fields and what content is expected.

## Accession fields in the IDF

### Comment[ArrayExpressAccession]
The accession of the experiment in ArrayExpress with the pattern `E-[A-Z]{4}-\d+`. 

### Comment[ExpressionAtlasAccession]
For experiments that are not from ArrayExpress, we'll add the accession number used for Expression Atlas. Usually in same style as ArrayExpress accessions (see above).

### Comment[SecondaryAccession]
Accessions for the _same_ experiment in another archive. For sequencing experiments this will always contain the accession of the underlying study in the INSDC (so the ENA or SRA study accession). Other archives and accession codes include 
* GEO series accession (GSE number) for GEO-imported experiments
* Human Cell Atlas (UUID of the project)
* EGA study or dataset accession for access-controlled data
* PRIDE proteomics archive (for Expression Atlas proteomics experiments)

### Comment[RelatedExperiment]
Accessions of studies that are similar to the experiment or use the same samples for a different type of analysis, e.g. in a multi-omics study. Thus accessions in this field can be other ArrayExpress, Expression Atlas accessions or point to different archives, such as PRIDE, MetaboLights or ENA. 


## Accession Fields in the SDRF

### Comment[BioSD_SAMPLE]
The BioSamples database accession of the sample.

### Comment[ENA_SAMPLE]
The ENA sample accession of the sample.

### Comment[ENA_EXPERIMENT]
The ENA experiment accession of the library (extract node).

### Comment[EGA_EXPERIMENT]
The EGA experiment accession for access-controlled data.

### Comment[ENA_RUN]
The ENA run accession of the assay replicate (assay node).

### Comment[EGA_RUN]
The EGA run accession for access-controlled data. 

### Comment[EGA_DATASET] 
The EGA dataset accession for access-controlled data.

### Comment[study]
For EGA data or if there is a mix of ENA studies, we'll include the ENA study accession (ERP) per sample in the SDRF.
