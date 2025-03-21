# EWAS_PHIV_EBioMedicine

This repository contains the code and scripts used in article: "Isolating the effects of HIV infection and HIV exposure on epigenetic profiles in infants" to generate the primary analysis results.
The workflow describes the following: 1) pre-processing procedures 2) site-wide and regional EWAS 3) Gene-specific analyses and 4) pathway enrichment. 

Below, you'll find a description of each file and the order in which they should be executed.

## File Execution Order

### 1. **EWAS-log1_Pre-processing**
   - **Description**: This script performs standard pre-processing procedures using ewastools.
   - **What it does**: This file prepares raw IDAT files for EWAS analysis by completing the following: filtering, quality control, dye-bias correction, sex check, etc.
   - **How to run**:
     ```bash
     Rscript EWAS-log1_Pre-processing.Rmd
     ```
### 1a. **cpg.assoc_mod**
   - **Description**: This script is a modified cpg.assoc() function from Bioconductor package CpGAssoc
   - **What it does**: This function performs the same analysis as the Bioconductor package, but removes the use of the memory.limit() function. After R version 4.0.0, this function is not supported. The original code specifies a max memory using memory.limit()/15 for windows OS. The new code specifies a memory limit of 50000/15 for windows OS.
   - **How to run**:
     ```bash
     Rscript cpg.assoc_mod.R
     ```

### 2. **EWAS-log2_EWAS Timepoint 3**
   - **Description**: This script performs the main EWAS analyses for Timepoint 3 biospecimens.
   - **What it does**: This file performs site-wide and regional EWAS, regional GO enrichment analysis across all 3 comparison groups (PHIV vs. HEU, PHIV vs. HUU, & HEU vs. HUU). Manhattan and Volcano plots are also generated. Similar code can be used to generate results for Timepoint 12 biospecimens.
   - **How to run**:
     ```bash
     Rscript EWAS-log2_EWAS Timepoint 3.Rmd
     ```

### 3. **EWAS-log2_Targeted-gene-analysis**
   - **Description**: This script performs association test for genes identified apriori from previous EWAS on children with HIV on ART.
   - **What it does**: This file assesses differential methylation at sites on targeted genes for Timepoint 3 & Timepoint 12 biospecimens. All results are bonferroni corrected per gene. Barplots of differential methylation estimates are also generated for signficant sites.
   - **How to run**:
     ```bash
     Rscript EWAS-log2_Targeted-gene-analysis.Rmd
     ```

### 4. **EWAS-log2_GO enrichment**
   - **Description**: This script performs an enrichment analysis of the significant genes found in `EWAS-log2_EWAS Timepoint 3` for the site-wide analysis.
   - **What it does**: This file conducts an enrichment analysis to determine if any biological processes, pathways, or molecular functions are significantly enriched in the identified genes. The code is also provided for Timepoint 12 biospecimens.
   - **How to run**:
     ```bash
     Rscript EWAS-log2_GO enrichment.Rmd
     ```

## File Dependencies

- The pre-processing script must be run first. All log 2 files can be run independently after that, excluding EWAS-log2_GO enrichment which must run after EWAS-log2_EWAS Timepoint 3.
- The cpgassoc_mod file should be saved and accessible prior to running all log2 files.
- **Dependencies**: Please ensure that you have the necessary packages installed. You can install them via the following commands:
  ```R
  #R packages available on CRAN:
  install.packages(c("tidyverse", "limma", "data.table", "magrittr", "ggplot2","stringi","stringr","qqman"))

  # Packages from Github:
  # Install the remotes package if not already installed
  install.packages("remotes")
  remotes::install_github("username/ewastools")

  #Bioconductor packages:
  install.packages("BiocManager")
  BiocManager::install(c("CpGAssoc","minfi","IlluminaHumanMethylationEPICanno.ilm10b4.hg19","DMRcate"))
