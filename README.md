# Does Fusion Stage Matter? Comparing Feature-Level and Decision-Level Multimodal Learning for Lung Cancer Subtyping

**Women in Machine Learning (WiML) Workshop at NeurIPS 2026 | Atlanta, USA**

**Researcher:** Gloria Moraa Riechi  
**Affiliation:** Department of Computational Data Science and Engineering, North Carolina A&T State University  
**Contact:** gmriechi@aggies.ncat.edu

**[View Accepted Abstract on OpenReview](https://openreview.net/forum?id=7Rp07uEd5s)**

## Overview

Multimodal machine learning integrates heterogeneous biological and clinical information to improve disease classification. However, the benefits of combining additional modalities may depend on both the informativeness of individual data sources and the stage at which they are integrated.

This research investigates whether **feature-level early fusion or decision-level late fusion** provides better classification performance for lung adenocarcinoma (LUAD) and lung squamous cell carcinoma (LUSC).

Using a matched cohort of 823 patients from The Cancer Genome Atlas (TCGA), we compare unimodal and multimodal classification across four data modalities and multiple machine-learning algorithms.

The study evaluates whether adding more modalities consistently improves prediction and whether the choice of fusion strategy influences classification performance.

## Research Objectives

1. Compare feature-level early fusion and decision-level late fusion for LUAD–LUSC classification.
2. Evaluate the predictive contribution of RNA-Seq, DNA methylation, histopathology, and clinical data.
3. Assess fusion strategies under an equal-feature comparison.
4. Examine modality contributions using ablation analysis.

## Dataset

**Source:** The Cancer Genome Atlas (TCGA)

**Classification task:** LUAD versus LUSC

**Matched cohort:** 823 patients

| Modality | Data representation |
|---|---|
| RNA-Seq | Gene expression features |
| DNA methylation | Epigenetic features |
| Histopathology | Whole-slide image (WSI) embeddings |
| Clinical | Patient clinical variables |

The primary equal-feature analysis retains 40 features per modality.

Raw patient-level data are not redistributed in this repository. Researchers should obtain the applicable datasets from authorized TCGA data sources and follow the relevant data-access requirements.

## Methodology

### 1. Data Preparation

Patients with matched multimodal information were selected to maintain consistent cohort composition across experiments.

Feature preprocessing and dimensionality reduction were performed using training data to avoid information leakage.

Histopathology representations were derived using attention-based multiple instance learning (ABMIL), followed by training-only dimensionality reduction.

### 2. Feature-Level Early Fusion

Early fusion combines selected features from different modalities into a single representation before classification.

The combined representation is supplied to a machine-learning classifier.

### 3. Decision-Level Late Fusion

Late fusion trains modality-specific classifiers independently and combines their predictions to generate a final classification.

This approach allows each modality to contribute through its own predictive model rather than requiring direct feature concatenation.

### 4. Model Evaluation

Six classical machine-learning classifiers were evaluated across unimodal and multimodal configurations.

Evaluation metrics included:

- Classification accuracy
- Macro-F1 score
- Area under the ROC curve (AUC-ROC)
- Sensitivity
- Specificity

Out-of-fold predictions from the training data were used for model and fusion-rule selection, with a held-out test set reserved for final evaluation.

## Experimental Results

| Model configuration | Strategy | Accuracy | AUC-ROC |
|---|---|---:|---:|
| RNA-Seq only | Unimodal | 97.55% | 0.9992 |
| Omics | Early fusion | 97.14% | 0.9987 |
| Omics | Late fusion | 97.14% | 0.9975 |
| Clinical + Omics | Early fusion | 96.73% | 0.9985 |
| Clinical + Omics | Late fusion | 97.14% | 0.9977 |
| Four modalities | Early fusion | 96.33% | 0.9928 |
| Four modalities | Late fusion | **97.96%** | 0.9935 |

### Key Findings

**RNA-Seq is highly predictive.** RNA-Seq alone achieved 97.55% accuracy and the highest reported AUC-ROC of 0.9992.

**Late fusion achieved the highest observed accuracy.** With all four modalities, late fusion achieved 97.96% accuracy compared with 96.33% for early fusion.

**Additional modalities did not consistently improve performance.** Results demonstrate that multimodal integration does not necessarily outperform an informative unimodal baseline.

### Ablation Analysis

Removing RNA-Seq from the four-modality late-fusion model reduced accuracy from 97.96% to 95.10%, representing a decline of 2.86 percentage points.

These findings suggest that RNA-Seq contributed substantially to classification performance.

The reported performance differences are descriptive and should not be interpreted as statistically significant without additional inferential analysis.

## Reproducibility

This repository is intended to provide the implementation of the study's preprocessing, feature extraction, fusion strategies, model evaluation, and result visualization.

Reproduction requires access to the relevant TCGA datasets and the study's preprocessing and model configurations.

Installation instructions, execution commands, dependencies, and the final repository directory structure will be documented alongside the released implementation.

## Citation

If you use this work, please cite the accepted WiML submission:

Riechi, G. M. (2026). *Does Fusion Stage Matter? Comparing Feature-Level and Decision-Level Multimodal Learning for Lung Cancer Subtyping*. Women in Machine Learning Workshop, NeurIPS 2026.

https://openreview.net/forum?id=7Rp07uEd5s

## Contact

**Gloria Moraa Riechi**  
Department of Computational Data Science and Engineering  
North Carolina A&T State University  
gmriechi@aggies.ncat.edu
