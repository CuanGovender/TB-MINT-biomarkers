# Cross-cohort transcriptional biomarkers for tuberculosis

Code for the Honours project report "Identification of host transcriptional biomarkers of latent
and active tuberculosis through integrative analysis of publicly available transcriptomic datasets"
by Cuan Govender (Division of Molecular Biology and Human Genetics, Stellenbosch University).

The analysis integrates four public whole-blood datasets with MINT sparse partial least squares
discriminant analysis (sPLS-DA; mixOmics) to classify healthy controls, latent TB infection (LTBI)
and active TB (ATB). Three cohorts form the training pool and one is held out for external validation.
The findings are hypothesis-generating.

## Data (not included; download from NCBI GEO)
- GSE19439 - UK, microarray (Illumina HumanHT-12 v3), training
- GSE84076 - Brazil, RNA-sequencing (Illumina HiSeq 2500), training
- GSE107994 - Leicester, UK, RNA-sequencing (Illumina HiSeq 4000), training
- GSE19444 - UK, microarray (Illumina HumanHT-12 v3), held-out external validation

## Scripts (run in this order)
1. GSE19439.qmd, GSE19444.qmd, GSE84076.qmd, GSE107994-RNASeq.qmd - cleaning, filtering and
   gene-level preparation of each dataset
2. MINT_Integration_Pipelineupdate.qmd - harmonisation, limma filter, MINT models, cross-validation
   (leave-one-group-out) and evaluation on the held-out cohort
3. Per_Gene_ROCAUC_Analysis__Core_Biomarker_Set_.qmd - single-gene ROC and AUC analysis of the
   21 shared genes

## Software
R 4.5.2 (Bioconductor 3.22); mixOmics 6.34.0; limma 3.66.0; edgeR 4.8.2; pROC 1.19.0.1;
clusterProfiler 4.18.4. Full list of versions: Addendum B of the report.

## Notes
File paths in the scripts point to the author's local folders and must be changed to run them.
