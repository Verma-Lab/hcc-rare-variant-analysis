# HCC Cancer-Predisposition Gene Analysis

Analysis code for the study:

> **Multi-ancestry sequencing analysis across 293,141 participants reveals cancer predisposition genes increase hepatocellular carcinoma risk**
> Garofalo AM\*, Chotiprasidhi P\*, Johnson JP, et al. Verma A†, Wangensteen KJ†.
> *\*Equal contribution; †Joint senior authors.* [Journal], [Year]. [DOI]

This repository contains the variant-processing, gene-burden, and downstream phenotype-analysis pipelines used in the study.


## Overview

Genetic testing for Lynch syndrome and *BRCA1/2*-associated hereditary cancer syndromes is recommended for patients with colorectal or pancreatic cancer, but the contribution of these DNA-repair genes to **hepatocellular carcinoma (HCC)** risk is largely unknown. We tested whether rare germline variants in DNA-repair genes are associated with HCC risk across ancestrally diverse cohorts.

Using whole-exome and whole-genome sequencing from four biobanks, we performed gene-level rare-variant burden testing on a pre-specified set of six DNA-repair genes and meta-analyzed the results across populations. Rare variants in **MSH6** and **BRCA2** were significantly associated with increased HCC risk (≈2.3–2.8-fold), implicating DNA-repair genes in HCC susceptibility across diverse populations.


## Study design

- **Participants:** 293,141 individuals — 2,594 HCC cases and 290,547 cancer-free controls.
- **Cohorts:** Penn Medicine BioBank (PMBB), All of Us Research Program (AOU), Mayo Clinic (including ESCALON and other internal/external biobanks), and the Million Veteran Program (MVP).
- **Genes tested (six DNA-repair genes):** `BRCA2`, `BRIP1`, `CHEK2`, `FANCA`, `MSH6`, `PMS2`.
- **Population groups:** participants were classified by genetic similarity into European (EUR), African (AFR), Admixed American (AMR), East Asian (EAS), South Asian (SAS), and Middle Eastern (MID). Burden analyses were run for **EUR**, **AFR**, and a combined all-population (**ALL**) analysis.

---

## Methods summary

- **Annotation:** variants annotated with Ensembl VEP against MANE transcripts; restricted to rare variants (MAF ≤ 1% or absent in gnomAD).
- **Variant tiers:** predicted loss-of-function (pLoF), damaging missense, and combined pLoF + damaging missense.
- **Burden testing:** SAIGE-GENE+, adjusted for age, sex, and genetic-ancestry principal components, at three maximum-MAF thresholds (1%, 0.1%, 0.01%).
- **Meta-analysis:** inverse-variance-weighted meta-analysis with the `metafor` R package (fixed-effects for EUR and AFR; random-effects for ALL).


## Repository contents

| File | Role in the analysis |
| --- | --- |
| `variant_mac_pipeline.py` | Computes per-variant minor allele counts (MAC) and applies the rare-variant MAF filtering used to define qualifying variants. |
| `variant_group_summary.py` | Assembles qualifying variants into per-gene burden groups (pLoF, damaging missense, and combined tiers) for SAIGE-GENE+ testing. |
| `variant_group_summary_sensitivity.py` | ClinVar-restricted sensitivity analysis of the variant groups (pathogenic/likely-pathogenic only). |
| `filter_variants_meeting_criteria_sanity_check.ipynb` | QC notebook confirming that retained variants meet the stated annotation/frequency inclusion criteria. |
| `plot_pca_ancestry_status.py` | Generates principal-component plots of genetic ancestry and case/control status used for population assignment. |
| `age_at_onset_pipeline.py` | Compares age at HCC diagnosis between variant carriers and non-carriers. |
| `carrier_comorbidity_pipeline.py` | Characterizes liver-disease comorbidities among rare-variant carriers (supports the comorbidity-adjusted burden analyses). |
| `cancer_cooccurence_pipeline.py` | Tests enrichment of co-occurring cancers among carriers (e.g., breast/ovarian for MSH6, prostate for FANCA). |


## Requirements

- **Python 3.9+** with the scientific stack: `pandas`, `numpy`, `scipy`, `matplotlib`, `seaborn`, and `jupyter` (for the QC notebook).
- **External tools used in the workflow** (run outside these scripts): Ensembl VEP (annotation), SAIGE-GENE+ (gene-burden association testing), and the `metafor` R package (meta-analysis).



## Usage

Each script is a standalone step; set the input/output paths and cohort identifiers at the top of each file before running. A representative order:

1. `variant_mac_pipeline.py` — compute MAC and apply MAF filters.
2. `variant_group_summary.py` — build per-gene variant-burden groups.
3. `filter_variants_meeting_criteria_sanity_check.ipynb` — QC the filtered variant set.
4. `variant_group_summary_sensitivity.py` — ClinVar-restricted sensitivity grouping.
5. (Run SAIGE-GENE+ burden tests and `metafor` meta-analysis on the resulting groups.)
6. `plot_pca_ancestry_status.py`, `age_at_onset_pipeline.py`, `carrier_comorbidity_pipeline.py`, `cancer_cooccurence_pipeline.py` — ancestry visualization and carrier-level phenotype analyses.


## Data availability

Data supporting the findings of this study are derived from four biobank resources, none of which are publicly available due to participant privacy and confidentiality protections:

- **All of Us Research Program** (Controlled Tier Dataset v8) — accessible to authorized researchers through the All of Us Researcher Workbench (https://allofus.nih.gov/article/opportunities-researchers).
- **Million Veteran Program (MVP)** — access restricted to VA investigators on approved federally funded projects; requests via the MVP Program Office.
- **Mayo Clinic Biobank** — available to authorized researchers in collaboration with a Mayo Clinic investigator (Biobank@mayo.edu).
- **Penn Medicine BioBank (PMBB)** — available to authorized researchers in collaboration with a Penn investigator (Contact: https://pmbb.med.upenn.edu/contact.php)


## Citation

If you use this code, please cite:

> Garofalo AM, Chotiprasidhi P, Johnson JP, et al. Multi-ancestry sequencing analysis across 293,141 participants reveals cancer predisposition genes increase hepatocellular carcinoma risk. [Journal]. [Year]. [DOI]



## Contact

 **Anurag Verma, PhD** — anurag.verma@pennmedicine.upenn.edu
 **Kirk J. Wangensteen, MD, PhD** (Mayo Clinic): Wangensteen.Kirk@mayo.edu


## License

[Specify a license (e.g., MIT) before public release.]
