# Cross-cohort host-blood transcriptomic biomarkers for tuberculosis

R code for an Honours project (Division of Molecular Biology and Human Genetics, Stellenbosch University) that integrates four public whole-blood gene-expression datasets to look for host transcriptional signatures that distinguish healthy controls, latent TB infection (LTBI) and active TB disease (ATB).

The work is exploratory and hypothesis-generating. The genes reported are candidates for follow-up, not validated diagnostic markers.

**Author:** Cuan Govender
**Supervisor:** Dr Anandi Bierman

## Approach

Three datasets are pooled as a training set and a fourth is kept out for external validation. A multivariate model that accounts for study of origin (MINT sPLS-DA, `mixOmics`) is tuned by leave-one-group-out cross-validation, where each fold holds out one whole study. Two model structures are compared: a flat three-class model and a two-step classifier (Stage 1: ATB versus non-ATB; Stage 2: Control versus LTBI). An exploratory Control versus ATB model is also fitted. Genes selected by the models are then summarised, tested for Gene Ontology enrichment and assessed one at a time by AUC on the held-out cohort.

## Datasets

All data are public on the NCBI Gene Expression Omnibus (GEO). No data are stored in this repository.

| Accession | Cohort | Platform | Role |
|---|---|---|---|
| GSE19439 | UK, whole blood | Illumina HumanHT-12 v3 microarray | Training |
| GSE84076 | Brazil, whole blood | RNA-seq (Illumina HiSeq 2500) | Training |
| GSE107994 | Leicester, UK, whole blood | RNA-seq (Illumina HiSeq 4000) | Training |
| GSE19444 | UK, whole blood | Illumina HumanHT-12 v3 microarray | Held out for external validation |

## Scripts

Each script is a Quarto document (`.qmd`) with an overview at the top that lists its inputs and outputs.

| Script | What it does |
|---|---|
| `GSE19439_tidy.qmd` | Cleans and checks the UK microarray training cohort |
| `GSE84076_tidy.qmd` | Cleans and checks the Brazilian RNA-seq training cohort |
| `GSE107994_RNASeq_tidy.qmd` | Cleans and checks the Leicester RNA-seq training cohort |
| `GSE19444_tidy.qmd` | Cleans and checks the held-out UK microarray cohort |
| `MINT_Integration_Pipeline_tidy.qmd` | Harmonises the four datasets, builds the training pool, fits and tunes the MINT models, validates on the held-out cohort, and extracts the selected genes |
| `Per_Gene_ROCAUC_Analysis_tidy.qmd` | Per-gene AUC of the 21 genes shared by all three models, on the held-out cohort |
| `Pathway_Enrichment_Analysis_tidy.qmd` | Gene Ontology enrichment of the selected genes and the table of all selected genes |

## Order to run

1. The four cleaning scripts, in any order. Each saves a gene-by-sample matrix and a clinical table as `.rds` files.
2. `MINT_Integration_Pipeline_tidy.qmd`, which reads those files and saves the fitted models and gene lists.
3. `Per_Gene_ROCAUC_Analysis_tidy.qmd` and `Pathway_Enrichment_Analysis_tidy.qmd`, which read the saved models and gene lists.

## Before you run

- **Paths:** each script sets a working directory or file path for the author's computer (`C:/Users/27826/Documents/HonoursProject`). Change these to your own folder.
- **Downloads:** GEO series matrices are downloaded by `GEOquery`. The raw supplementary files must be downloaded from GEO by hand: the Illumina raw archives for GSE19439 and GSE19444, `GSE84076_ExpressionDataMatrix.txt.gz`, and the Leicester count file for GSE107994.
- **Packages:** `GEOquery`, `limma`, `edgeR`, `mixOmics`, `pROC`, `clusterProfiler`, `org.Hs.eg.db`, `enrichplot`, `tidyverse`, `janitor`, `sva`, `SummarizedExperiment`, `readxl`, `knitr`. Run `sessionInfo()` to record the versions you used.
- **Known gaps:** the per-gene AUC and pathway scripts use some objects that are created in the MINT session and are not saved to files. They are listed in the overview of each script.

## Outputs

The scripts write, among others, `gene_matrix_final_*.rds`, `clin_*.rds`, `log2_cpm_full_107994.rds`, the fitted model objects, `per_gene_auc_results.csv`, `Supplementary_Table_S3_all_genes.csv` (all selected genes and the models that chose them) and `GO_terms_all_sets.csv` (enriched terms for each gene set).

## Limitations

- The limma filter that reduces the genes to 208 is fitted once on the whole training pool, outside the cross-validation, so the cross-validated error may be slightly optimistic.
- Only one dataset is held out, and the number of samples per group is small.
- The data are retrospective and come from cohorts with different designs, platforms and group definitions.
