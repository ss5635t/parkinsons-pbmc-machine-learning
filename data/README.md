# Data

This project uses publicly available gene expression data from the NCBI Gene Expression Omnibus (GEO).

## GSE22491

GSE22491 was used as the development dataset.

- **Samples:** 18 total — 10 Parkinson's disease and 8 controls
- **Sample type:** Peripheral blood mononuclear cells (PBMCs)
- **Platform:** Agilent whole-human-genome microarray
- **Use in this project:** Exploratory analysis, model development, internal validation, feature analysis and gene annotation

## GSE100054

GSE100054 was used as the independent external validation dataset.

- **Samples:** 19 total — 10 Parkinson's disease and 9 controls
- **Sample type:** Peripheral blood mononuclear cells (PBMCs)
- **Platform:** Affymetrix Human Clariom D Assay
- **Use in this project:** Independent cross-cohort validation

## Data Availability

The raw datasets are not included in this repository. The analysis notebook downloads the required public expression and annotation data during execution.

This keeps the repository lightweight while allowing the analysis to be reproduced from the original public data sources.
