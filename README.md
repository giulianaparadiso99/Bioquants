# Breast Cancer Gene Expression Analysis

Course project for **Bioquants** – MSc in Mathematical Engineering, Politecnico di Torino (A.Y. 2025/2026).

**Full report:** [Report.pdf](Report.pdf)

## Overview

End-to-end Python pipeline to identify genes that are differentially expressed between breast cancer and normal breast tissue, and to interpret them through functional enrichment analysis. The workflow covers data import, quality control, statistical testing with multiple-testing correction, Gene Ontology (GO) and KEGG enrichment, and visualisation of the results.

## Dataset

- **Source:** Gene Expression Omnibus (GEO), series [GSE15852](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE15852)
- **Platform:** Affymetrix Human Genome U133A Array ([GPL96](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GPL96))
- **Size:** 86 samples × 22,283 probes
- **Labels:** tumour vs. normal tissue, extracted from the `Sample_source_name_ch1` metadata field

The data are not included in this repository. Download the Series Matrix file from the GEO page above and the GPL96 annotation file from the platform page.

## Methods

1. **Data import:** the Series Matrix file is read with pandas and transposed so that rows are samples and columns are probes.
2. **Quality control:**
   - missing values checked with `isnull` (none found);
   - outlier samples screened with boxplots and per-sample z-scores, threshold |z| > 3 (none found, all samples retained).
3. **Differential expression:** Welch's two-sample t-test for each probe (tumour vs. normal), with p-values adjusted by the Benjamini–Hochberg false discovery rate (FDR) procedure. Significance threshold: adjusted p < 0.05.
4. **Probe-to-gene mapping:** probes mapped to gene symbols using the GPL96 annotation file.
5. **Functional enrichment:** GO Biological Process and KEGG pathway enrichment via Enrichr, with hypergeometric-test p-values corrected by Benjamini–Hochberg.
6. **Visualisation:** volcano plot, heatmap of the top 30 differentially expressed genes (row-wise z-scores), and bar plots of the top 10 enriched GO terms and KEGG pathways.

## Main results

| Analysis | Result |
|---|---|
| Differentially expressed probes (FDR < 0.05) | 1,667 |
| Unique gene symbols mapped | 1,398 |
| Enriched GO Biological Processes (FDR < 0.05) | 255 |
| Enriched KEGG pathways (FDR < 0.05) | 44 |

The top enriched GO processes involve regulation of apoptosis, cell proliferation, angiogenesis and extracellular matrix organisation. Among the KEGG pathways are PPAR signalling, proteoglycans in cancer, focal adhesion, ECM–receptor interaction and several metabolic pathways. Together these point to transcriptional changes consistent with known mechanisms of tumour progression. Full tables, figures and discussion are in the [report](Report.pdf).

## Repository structure

```
.
├── README.md
├── Report.pdf
├── requirements.txt
└── <notebook or script files>
```

## Requirements

Python 3 with:

```
pandas
numpy
scipy
statsmodels
matplotlib
seaborn
gseapy
```

Install with `pip install -r requirements.txt`.

## Author

Giuliana Paradiso – Politecnico di Torino
