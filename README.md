# Tumor microenvironment of HPV-independent vulvar squamous cell carcinoma

Analysis code and anonymized data accompanying the article:

> Peñuelas N, Sisuashvili L, Marimón L, Díez-Ahijado L, Carreras-Dieguez N, Gonzalez-Serra C, Darecka K, Muñoz-Hurtado J, Saco A, del Pino M, Torné A, Valls-Losada S, Escoda-Suarez A, Sánchez-Hoyo B, Gaba L, Ordi J, Albero R and Rakislova N (2026). **Plasmacytoid dendritic cells and M2 macrophages are associated with clinical outcomes in human papillomavirus–independent vulvar squamous cell carcinoma.** *Front. Immunol.* 17:1922286. doi: [10.3389/fimmu.2026.1922286](https://doi.org/10.3389/fimmu.2026.1922286)

Part of this dataset will also be analysed in a separate manuscript (in preparation) focused on the transcriptomic analysis. This README will be updated with the reference once it is published.

---

## 1. Overview

This repository contains the R scripts used to:

1. Estimate immune and stromal cell populations from targeted transcriptomic data (HTG EdgeSeq) using four deconvolution algorithms: **xCell** (main analysis), **EPIC**, **quanTIseq** and **MCP-counter** (supporting analyses).
2. Compare the tumor microenvironment (TME) between HPV-associated (HPV-A, n=18) and HPV-independent (HPV-I, n=52) vulvar squamous cell carcinoma (VSCC), and within HPV-I tumors according to p53 immunohistochemistry (IHC) and PD-L1 status.
3. Evaluate the association of deconvolution-derived cell populations with recurrence-free survival (RFS) and disease-specific survival (DSS) in HPV-I VSCC.
4. Evaluate the main findings at the protein level by IHC for **CD123** (n=43) and **CD163** (n=50), including an exploratory analysis combining CD163 and PD-L1.
5. Explore a 12-chemokine tertiary lymphoid structure (TLS) gene signature.

All analyses use the same 70 cases (18 HPV-A, 52 HPV-I).

---

## 2. Repository structure

```
.
├── README.md
├── LICENSE
├── data/
│   ├── counts_matrix_anonymized.csv
│   ├── clinical_data_anonymized.csv
│   ├── clinical_data_dictionary.csv
│   └── LICENSE.md
└── scripts/
    ├── 1_Loading data.Rmd
    ├── 2_Descriptive tables.Rmd
    ├── 3_Comparison HPV status_xCell.Rmd
    ├── 4_Comparison HPV status_EPIC.Rmd
    ├── 5_Comparison HPV status_MCPcounter.Rmd
    ├── 6_Comparison HPV status_QTI.Rmd
    ├── 7_HPV-I comparison by p53 status_xCell.Rmd
    ├── 8_HPV-I comparison by p53 status_EPIC.Rmd
    ├── 9_HPV-I comparison by p53 status_MCPcounter.Rmd
    ├── 10_HPV-I comparison by p53 status_QTI.Rmd
    ├── 11_HPV-I comparison by PD-L1 status_xCell.Rmd
    ├── 12_HPV-I comparison by PD-L1 status_EPIC.Rmd
    ├── 13_HPV-I comparison by PD-L1 status_MCP.Rmd
    ├── 14_HPV-I comparison by PD-L1 status_QTI.Rmd
    ├── 15_HPV-I survival analysis.Rmd
    ├── 16_HPV-I survival analysis_EPIC.Rmd
    ├── 17_HPV-I survival analysis_MCP.Rmd
    ├── 18_HPV-I survival analysis_QTI.Rmd
    ├── 19_IHC CD123 analysis.Rmd
    ├── 20_IHC CD163 analysis.Rmd
    ├── 21_Comparison full cohort vs IHC cohorts.Rmd
    ├── 22_CD123 CD163 PDL1 correlation.Rmd
    └── 23_TLS signature analysis.Rmd
```

"QTI" in script names refers to quanTIseq.

---

## 3. Data

All files in `data/` are anonymized and contain only the 70 cases analysed in the study.

| File | Content |
|---|---|
| `counts_matrix_anonymized.csv` | Filtered HTG EdgeSeq count matrix (10,513 genes × 70 cases). First column `gene` (gene symbol); remaining columns are cases. This is the matrix used as input for all deconvolution analyses. |
| `clinical_data_anonymized.csv` | One row per case: HPV status, clinicopathological variables used in the analyses, PD-L1, follow-up (RFS and DSS, truncated at 60 months) and CD123/CD163 IHC percentages. |
| `clinical_data_dictionary.csv` | Description of every variable in the clinical file. |

**Case identifiers.** Cases are identified as `VSCC-xxx`. Identifiers are identical in the count matrix and the clinical file. VSCC-006, VSCC-018 and VSCC-037 do not appear in either file because they were excluded from the study (see Methods of the article).

**IHC data.** CD123 and CD163 percentages are available only for HPV-I cases with assessable tissue (CD123: n=43; CD163: n=50) and are `NA` otherwise. Values are the percentage of positive cells in three compartments: overall tumor area, tumor-associated stroma and perivascular areas. The interobserver re-evaluation (CD163: n=43; CD123: n=38; Supplementary Table 10) is not included in the public files.

### Privacy and anonymization

To minimize the risk of re-identification:

- Hospital accession numbers, dates (diagnosis, surgery, follow-up) and any other direct identifiers have been removed.
- Only variables used in the analyses are included.
- Survival times are expressed in months from surgery and rounded to one decimal.
- Age at diagnosis and some descriptive variables (histologic type, lymphovascular invasion, surgery type, adjuvant radiotherapy) are **not** included in the public file.

The correspondence between study identifiers and hospital records is kept by the authors and is not shared.

---

## 4. Requirements

The analyses were performed with **R 4.4.0 on Windows 11**. Main packages and versions:

| Package | Version | Source |
|---|---|---|
| immunedeconv | 2.1.0 | GitHub (omnideconv/immunedeconv) |
| edgeR | 4.2.2 | Bioconductor |
| limma | 3.60.6 | Bioconductor |
| ComplexHeatmap | 2.20.0 | Bioconductor |
| GSVA | 1.52.3 | Bioconductor |
| coxphf | 1.13.4 | CRAN |
| maxstat | 0.7-26 | CRAN |
| survival | 3.8-6 | CRAN |
| survminer | 0.5.2 | CRAN |
| gtsummary | 2.5.0 | CRAN |
| dplyr | 1.2.0 | CRAN |
| tidyr | 1.3.2 | CRAN |
| ggplot2 | 4.0.2 | CRAN |
| broom | 1.0.12 | CRAN |
| tibble | 3.2.1 | CRAN |
| ggpubr | 0.6.3 | CRAN |
| rstatix | 0.7.3 | CRAN |
| openxlsx | 4.2.8.1 | CRAN |

Installation (example):

```r
install.packages(c("coxphf", "maxstat", "survival", "survminer", "gtsummary",
                   "dplyr", "tidyr", "ggplot2", "broom", "tibble", "ggpubr",
                   "rstatix", "openxlsx", "readr", "readxl", "ggrepel", "remotes"))

if (!requireNamespace("BiocManager", quietly = TRUE)) install.packages("BiocManager")
BiocManager::install(c("edgeR", "limma", "ComplexHeatmap", "GSVA"))

remotes::install_github("omnideconv/immunedeconv")
```

The complete `sessionInfo()` of the environment used for the article is provided at the end of `1_Loading data.Rmd`.

---

## 5. How to run

The scripts are provided to document the analyses exactly as performed. They were written for the internal data objects used during the study and read the original (non-public) input files; to run them with the public data in `data/`, the loading steps need to be adapted. The scripts are designed to be run in numerical order within the same R session.

The public files correspond to the objects used in the scripts as follows:

| Object in the scripts | Public file | Notes |
|---|---|---|
| `counts_filtered_filtered` (and HPV subsets, e.g. `counts_HPV_Independent`) | `counts_matrix_anonymized.csv` | Genes as row names (first column `gene`), cases as columns |
| `TP53status_clinical` (and HPV subsets `TP53status_clinical_HPVas` / `_HPVind`) | `clinical_data_anonymized.csv` | Case identifier: `id` in the scripts, `case_id` in the public file. In the scripts, p53 IHC status is coded `Wt`/`Mutant` (public file: `Normal`/`Abnormal`) and FIGO stage as `0 (Initial)`/`1 (Advanced)`. Variable names are described in `clinical_data_dictionary.csv` |
| `CD123_5`, `CD163` (read from `Validation_CD123.xlsx` / `Validation_CD163.xlsx`) | `clinical_data_anonymized.csv` (columns `cd123_*` and `cd163_*`) | In the scripts, IHC columns are named `Positive_cells_tumoral_area`, `Positive_cells_tumor_stroma` and `Positive_cells_perivascular_areas`, with the case identifier `Case_ID` |

---

## 6. Scripts and correspondence with the article

| Script | Analysis | Output in the article |
|---|---|---|
| `1_Loading data` | Loads the data, creates HPV subsets, reports package versions and `sessionInfo()` | — |
| `2_Descriptive tables` | Clinicopathological characteristics by HPV status and, within HPV-I, by p53 status | Table 1; Supplementary Table 2 (require the complete clinical dataset; see Section 9) |
| `3_Comparison HPV status_xCell` | xCell deconvolution of the 70 cases, filtering of cell types, PCA, HPV-A vs HPV-I comparisons (Wilcoxon, BH-FDR), heatmap | Figure 1; Supplementary Figure 1; Supplementary Tables 3–5 |
| `4–6_Comparison HPV status_*` | Same analyses with EPIC, MCP-counter and quanTIseq | Supplementary Figure 2; Supplementary Table 5 |
| `7–10_HPV-I comparison by p53 status_*` | HPV-I tumors: p53-normal vs p53-abnormal | Supplementary Figure 3; Supplementary Tables 6–7 |
| `11–14_HPV-I comparison by PD-L1 status_*` | HPV-I tumors: PD-L1⁺ vs PD-L1⁻ | Figure 4 |
| `15_HPV-I survival analysis` | Univariable and multivariable Firth Cox models (maxstat, median, continuous), proportional-hazards tests, bootstrap optimism correction, Kaplan–Meier curves (xCell) | Figure 2; Supplementary Data 1–3 |
| `16–18_HPV-I survival analysis_*` | Same survival workflow for EPIC, MCP-counter and quanTIseq | Supplementary Data 4 |
| `19_IHC CD123 analysis` | CD123 IHC: p53 comparison, survival models, PH tests, bootstrap | Figure 3; Supplementary Data 5–7 |
| `20_IHC CD163 analysis` | CD163 IHC: PD-L1 comparison, survival models, PH tests, bootstrap; exploratory PD-L1 × CD163 analysis (four-group, additive and interaction models, penalized likelihood-ratio test) | Figure 5; Supplementary Data 8–10; Supplementary Table 9 |
| `21_Comparison full cohort vs IHC cohorts` | Included vs excluded cases in the IHC cohorts | Supplementary Table 8 (see Section 9) |
| `22_CD123 CD163 PDL1 correlation` | Spearman correlations among CD123, CD163 and PD-L1 | Results (correlation analysis) |
| `23_TLS signature analysis` | Exploratory 12-chemokine TLS signature (GSVA): group comparisons, correlations with xCell populations and CD123/CD163 IHC, survival analyses | Supplementary Figure 6; Supplementary Tables 11–13 |

### Supplementary Data files

The Supplementary Data files are numbered in the order in which they are cited in the article.

| File | Content | Script | Source in the script |
|---|---|---|---|
| Supplementary Data 1 | Univariable and multivariable Firth Cox models for DSS and RFS, xCell features | 15 | written to `Supplementary_Data_1_Survival_xCell.xlsx` |
| Supplementary Data 2 | Proportional-hazards tests (Schoenfeld residuals), xCell survival models | 15 | written to `Supplementary_Data_2_PH_xCell.xlsx` (objects `ph_DDS_all`, `ph_RFS_all`) |
| Supplementary Data 3 | Bootstrap optimism-corrected estimates, xCell models | 15 | objects `boot_DDS_all`, `boot_RFS_all` |
| Supplementary Data 4 | Univariable and multivariable survival models, EPIC, quanTIseq and MCP-counter | 16–18 | `EPIC_survival_with_cutoffs.xlsx`, `QTI_survival_with_cutoffs.xlsx`, `MCP_survival_with_cutoffs.xlsx` |
| Supplementary Data 5 | Univariable and multivariable survival models, CD123⁺ cell infiltration | 19 | `CD123_survival_with_cutoffs.xlsx` |
| Supplementary Data 6 | Bootstrap optimism-corrected estimates, CD123 models | 19 | objects `boot_DSS_ihc_all`, `boot_RFS_ihc_all` |
| Supplementary Data 7 | Proportional-hazards tests, CD123 models | 19 | objects `ph_DSS_ihc_all`, `ph_RFS_ihc_all` |
| Supplementary Data 8 | Univariable and multivariable survival models, CD163⁺ cell infiltration | 20 | `CD163_survival_with_cutoffs.xlsx` |
| Supplementary Data 9 | Bootstrap optimism-corrected estimates, CD163 models | 20 | `CD163_bootstrap.xlsx` |
| Supplementary Data 10 | Proportional-hazards tests, CD163 models | 20 | objects `ph_DSS_ihc_all`, `ph_RFS_ihc_all` |

Supplementary Table 9 (PD-L1 × CD163 models) is written by script 20 to `Supplementary_Table_9_PDL1_CD163.xlsx`.

---

## 7. Methodological notes

**Deconvolution.** xCell was applied to raw counts; EPIC, quanTIseq and MCP-counter were applied to TMM-normalized CPM values (edgeR). Cell types with an estimate >0.01 in fewer than 10% of samples were excluded. For xCell, progenitor populations were also excluded and the three aggregate scores (immune, stroma, microenvironment) were analysed separately, leaving 25 cell types. For HPV-I analyses, xCell was re-run on the 52 HPV-I samples, because xCell scores are scaled relative to the samples analysed together; the same 25 cell types retained in the full cohort were used.

**Survival analysis.** Cox regression with Firth's penalized likelihood (`coxphf`), with confidence intervals and p-values from penalized profile likelihood. Each marker was analysed as:

- dichotomized by the maximally selected log-rank statistic (`maxstat`, groups constrained to 20–80%);
- dichotomized at the median;
- continuous.

"High" is defined as a value **above** the cut-off. Cut-points were estimated separately for RFS and DSS and are reported, with the number of patients and events per group, in the Supplementary Data. P-values were adjusted with the Benjamini–Hochberg method within each approach. Markers with BH-adjusted p<0.10 in any univariable approach were carried forward to multivariable models adjusted for FIGO stage and p53 IHC status (CD163 models were additionally adjusted for PD-L1). Follow-up was truncated at 60 months.

**Proportional hazards** were assessed with Schoenfeld residuals (`cox.zph`) on the corresponding unpenalized Cox models. When one group has no events (complete separation), the test returns χ² = 0 and p ≈ 1; this reflects the absence of events, not evidence that the assumption holds.

**Bootstrap optimism correction** (1,000 resamples; the seed is set to 1 separately for each marker) was applied to multivariable models whose enrichment term reached BH-adjusted p<0.05, re-estimating the cut-point in each resample. Bootstrap replicates were fitted with 500 iterations and a maximum step size of 0.5 per iteration (`coxphf` arguments `maxit` and `maxstep`); apparent models used 100,000 iterations. Resamples in which either group contained no events (in the bootstrap sample, or after applying the bootstrap cut-point to the original data) were excluded, and the proportion of valid resamples is reported. For models with complete separation in the original data (e.g., no events in the high-infiltration group), a substantial fraction of resamples is excluded, and corrected estimates should be interpreted with caution.

**PD-L1 × CD163 interaction.** The interaction was assessed with the Firth-penalized interaction coefficient and with a penalized likelihood-ratio test (`coxphftest`) comparing models with and without the interaction term. The test is fitted with numeric 0/1 indicators (CD163-high, PD-L1-positive and their product), which gives a model identical to `CD163 * PDL1`. With factor variables, `coxphftest(..., test = ~ CD163:PDL1)` silently tests 0 parameters and returns p = 1.

**TLS signature.** The 12-chemokine TLS signature (CCL2, CCL3, CCL4, CCL5, CCL8, CCL18, CCL19, CCL21, CXCL9, CXCL10, CXCL11, CXCL13) was scored by GSVA (Gaussian kernel) on TMM-normalized log2-CPM values. Survival analyses followed the same two-step strategy; given its exploratory nature, bootstrap optimism correction and proportional-hazards testing were not performed for this signature.

**Combined IHC analyses.** Analyses combining IHC markers (correlation, PD-L1/CD163 stratification) use the overall tumor area, with high infiltration defined as values above the maxstat-derived DSS cut-off (>30% CD123⁺ cells; >15% CD163⁺ cells).

---

## 8. Reproducibility notes

- **Survival times.** Survival times in the public file are rounded to one decimal to limit re-identification risk. RFS analyses are reproduced exactly. DSS estimates may differ slightly from those reported (typically in the second or third decimal), without changing any conclusion.
- **Near-zero xCell estimates.** For several cell types, most samples have an xCell estimate of essentially zero, which xCell returns as floating-point values of the order of 1e-17 rather than exact zeros. Rank-based tests (Wilcoxon) and maxstat cut-points for these cell types can therefore vary between package versions and platforms. Results for cell types with non-zero estimates, and all aggregate scores, are stable.
- **Package versions and platform.** Deconvolution estimates and bootstrap results can differ slightly between package versions (e.g., R, maxstat, survival) and operating systems. The results in the article were obtained with the versions listed in Section 4 (R 4.4.0, Windows 11). In independent re-runs with other versions, all point estimates reported in the article were reproduced, and bootstrap estimates differed only in the decimals, without changing any conclusion.
- **MCP-counter.** In immunedeconv, the MCP-counter "monocytic lineage" signature is reported under two names ("Monocyte" and "Macrophage/Monocyte") with identical values. Both were retained, as in the article; they should be interpreted as a single population.

---

## 9. Analyses that require the complete clinical dataset

Table 1 and Supplementary Table 2 (`2_Descriptive tables`) and Supplementary Table 8 (`21_Comparison full cohort vs IHC cohorts`) include variables that are not in the public file (age, histologic type, lymphovascular invasion, surgery type, radiotherapy) and were generated from the complete clinical dataset. With the public data, script `21` reproduces all remaining rows. The PCA regression models (`3`–`10`) include age as a covariate; to run them with the public data, remove `age` from the model formula (estimates for the remaining variables may differ slightly from those reported). The interobserver analysis (Supplementary Table 10) is based on the second pathologist's evaluation, which is not included in the public files.

---

## 10. Change log

- **November 2026 (proof stage).** Script `15`: added the export of Supplementary Data 2 (`Supplementary_Data_2_PH_xCell.xlsx`) directly from R, which also corrects decimal separators lost when the table was previously transferred to Excel manually. Script `20`: replaced the penalized likelihood-ratio test for the CD163 × PD-L1 interaction with a version based on numeric indicators (see Section 7) and added the export of Supplementary Table 9. Supplementary Data files renumbered 1–10 in order of citation. No reported result changed.

---

## 11. Ethics

The study was approved by the Institutional Ethics Committee of Hospital Clínic of Barcelona (HCB/2020/1198). Data are shared in anonymized form in accordance with the approval and applicable data-protection regulations. Please do not attempt to re-identify participants.

---

## 12. Citation

If you use this code or data, please cite the article:

> Peñuelas N, Sisuashvili L, Marimón L, Díez-Ahijado L, Carreras-Dieguez N, Gonzalez-Serra C, Darecka K, Muñoz-Hurtado J, Saco A, del Pino M, Torné A, Valls-Losada S, Escoda-Suarez A, Sánchez-Hoyo B, Gaba L, Ordi J, Albero R and Rakislova N (2026). Plasmacytoid dendritic cells and M2 macrophages are associated with clinical outcomes in human papillomavirus–independent vulvar squamous cell carcinoma. *Front. Immunol.* 17:1922286. doi: 10.3389/fimmu.2026.1922286

## 13. License

Code: MIT License (see `LICENSE`). Data in `data/`: CC BY 4.0 (see `data/LICENSE.md`).

## 14. Contact

Natalia Rakislova — natalia.rakislova@isglobal.org
