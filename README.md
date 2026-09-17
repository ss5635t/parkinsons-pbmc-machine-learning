# Parkinson's Disease PBMC Gene Expression: Cross-Cohort Machine Learning Analysis

An exploratory machine learning analysis of peripheral blood mononuclear cell (PBMC) gene expression data in Parkinson's disease, with a particular focus on **independent cross-cohort validation and model generalisability**.

This project uses two publicly available transcriptomic datasets from the NCBI Gene Expression Omnibus (GEO). A machine learning pipeline was developed and internally validated using GSE22491, then evaluated on the independent GSE100054 cohort generated using a different microarray platform.

The project demonstrates an important challenge in biomedical machine learning: **strong internal validation performance does not necessarily translate into successful external generalisation**.

## Project Objectives

The main objectives of this project were to:

- Explore and preprocess PBMC gene expression data from a Parkinson's disease cohort.
- Build a leakage-conscious machine learning pipeline for Parkinson's disease classification.
- Evaluate internal model performance using repeated stratified cross-validation and permutation testing.
- Examine feature stability and map microarray probes to gene annotations.
- Test the trained model on an independent external Parkinson's disease cohort.
- Investigate cross-platform differences and the reproducibility of gene expression effects across datasets.
- Critically assess limitations, potential confounding factors, and model generalisability.

## Datasets

Two publicly available PBMC gene expression datasets from the NCBI Gene Expression Omnibus (GEO) were used:

### Development Dataset — GSE22491

- **Samples:** 18 total — 10 Parkinson's disease and 8 controls
- **Sample type:** Peripheral blood mononuclear cells (PBMCs)
- **Platform:** Agilent whole-human-genome microarray
- **Purpose:** Model development, internal validation, feature analysis and gene annotation

### External Validation Dataset — GSE100054

- **Samples:** 19 total — 10 Parkinson's disease and 9 controls
- **Sample type:** Peripheral blood mononuclear cells (PBMCs)
- **Platform:** Affymetrix Human Clariom D Assay
- **Purpose:** Independent external validation

The use of different microarray platforms provides a challenging test of whether patterns learned from the development cohort generalise to an independent dataset.

## Methodology

The analysis followed an end-to-end computational workflow:

1. Downloaded and parsed publicly available GEO gene expression data.
2. Explored expression distributions and applied log2 transformation where appropriate.
3. Used PCA to examine major patterns of variation between samples.
4. Built a leakage-conscious machine learning pipeline using variance filtering, ANOVA-based feature selection, standardisation and logistic regression.
5. Evaluated internal performance using repeated stratified cross-validation and permutation testing.
6. Assessed feature-selection stability across training folds and mapped microarray probes to gene annotations.
7. Processed an independent external cohort and mapped both datasets to shared gene symbols.
8. Evaluated the trained model on the external cohort without using external labels for model fitting or feature selection.
9. Investigated cross-dataset expression shifts and the reproducibility of Parkinson's disease–control expression effects.

## Key Results

### Internal Validation

The development cohort showed very strong separation between Parkinson's disease and control samples.

- Repeated stratified cross-validation produced a balanced accuracy of **1.00**.
- The permutation test produced an empirical p-value of **0.0099** using 100 permutations.
- Performance remained stable across several feature-selection sizes.
- Feature-stability analysis identified a subset of repeatedly selected probes.

Because the development dataset contained only 18 samples and thousands of gene-expression features, these results were treated cautiously rather than as evidence of diagnostic performance.

### External Validation

Performance did not generalise to the independent GSE100054 cohort:

- **Accuracy:** 47.4%
- **Balanced accuracy:** 50.0%
- **Parkinson's disease F1-score:** 0.00
- **ROC-AUC:** 0.333
- The model classified all 19 external samples as controls.

Further analysis showed substantial cross-dataset expression shifts and weak agreement in Parkinson's disease–control expression effects among the selected genes. Of the 50 selected genes, 27 showed effects in the same direction across datasets, while the overall effect-size correlation was **-0.163**.

These findings highlight the importance of independent external validation in high-dimensional biomedical machine learning. Excellent internal cross-validation performance in a small cohort may reflect dataset-specific patterns that do not reproduce in an independent cohort.

## Limitations

Several limitations should be considered when interpreting the results:

- The development cohort was very small relative to the number of gene-expression features, creating a substantial risk of overfitting.
- The two cohorts were generated using different microarray platforms and preprocessing procedures, which may contribute to cross-dataset distribution shifts.
- Within-sample standardisation was used to support cross-platform comparison, but this does not eliminate platform-specific effects and changes the interpretation of expression values.
- Important potential confounders such as age, sex, BMI, medication and ancestry were not available in the GSE22491 series-matrix metadata used in this analysis.
- PBMC samples contain mixtures of immune cell types, and differences in cell-type composition could contribute to observed expression patterns.
- Probe-to-gene mapping and averaging multiple probes to gene-level values introduce additional uncertainty.
- The small cohorts and external validation results mean that the selected genes should not be interpreted as diagnostic biomarkers or evidence of causal involvement in Parkinson's disease.

This project is therefore intended as an exploratory computational analysis and a demonstration of machine learning, transcriptomic data processing and cross-cohort validation rather than a clinical diagnostic model.

## Technologies & Skills

- **Python**
- **Data analysis:** pandas, NumPy
- **Machine learning:** scikit-learn
- **Statistical analysis:** statsmodels, permutation testing, multiple-testing correction
- **Visualisation:** Matplotlib
- **Dimensionality reduction:** Principal Component Analysis (PCA)
- **Machine learning methods:** logistic regression, feature selection, repeated stratified cross-validation
- **Transcriptomic data processing:** GEO data parsing, microarray probe annotation and probe-to-gene mapping
- **Model evaluation:** internal cross-validation, feature-stability analysis and independent external validation
- **Reproducible research:** leakage-conscious pipelines and documented end-to-end analysis

## Repository Structure

```text
parkinsons-pbmc-machine-learning/
│
├── README.md
├── requirements.txt
├── .gitignore
│
├── notebooks/
│   └── 01_parkinsons_pbmc_analysis.ipynb
│
├── data/
│   └── README.md
│
└── figures/
```

The raw GEO datasets are not stored directly in this repository. The analysis notebook downloads the required public data from GEO, helping keep the repository lightweight and the workflow reproducible.

## How to Run

The analysis was developed and tested in Google Colab.

1. Clone or download this repository.
2. Open `notebooks/01_parkinsons_pbmc_analysis.ipynb` in Google Colab or Jupyter Notebook.
3. Install the required Python packages listed in `requirements.txt`.
4. Run the notebook from top to bottom.

The notebook downloads the required public GEO datasets and annotation data during execution, so the raw datasets do not need to be downloaded manually.

## Conclusion

This project demonstrates the importance of evaluating machine learning models beyond internal cross-validation, particularly when working with small, high-dimensional biomedical datasets.

Although the development cohort produced perfect internal classification performance, this result did not generalise to the independent external cohort. Cross-dataset expression shifts and weak reproducibility of disease-control effects suggest that the model learned patterns that were not sufficiently stable across cohorts and platforms.

Future work could investigate larger cohorts, improved control of clinical and technical confounders, cell-type-aware analysis, more robust cross-platform harmonisation, and single-cell transcriptomic approaches.
