# LUAD–LUSC Multimodal Fusion

### Does Fusion Stage Matter? Comparing Feature-Level and Decision-Level Multimodal Learning for Lung Cancer Subtyping

**Women in Machine Learning (WiML) Workshop @ NeurIPS 2026**

**Author:** Gloria Moraa Riechi  
**Affiliation:** Department of Computational Data Science and Engineering, North Carolina A&T State University, Greensboro, NC, USA

📄 **[Accepted Research Abstract (PDF)](https://openreview.net/pdf?id=7Rp07uEd5s)**

---

## 1. Overview

Multimodal machine learning enables the integration of heterogeneous molecular, histopathological, and clinical information for cancer classification. However, incorporating additional data modalities does not necessarily improve predictive performance, particularly when individual modalities already provide highly discriminative information.

This study investigates how the **stage of multimodal integration** influences the classification of lung adenocarcinoma (LUAD) and lung squamous cell carcinoma (LUSC).

Using a matched cohort of **823 patients from The Cancer Genome Atlas (TCGA)**, we systematically compare two multimodal integration strategies:

- **Feature-level early fusion:** Combines features from multiple modalities before classification.
- **Decision-level late fusion:** Combines predictions from independently trained modality-specific classifiers.

The study evaluates multiple modality combinations, six classical machine-learning classifiers, and a controlled equal-feature setting to investigate whether fusion strategy and modality informativeness influence classification performance.

### Research Questions

1. Does decision-level late fusion outperform feature-level early fusion for LUAD–LUSC classification?
2. Does incorporating additional modalities consistently improve performance over an RNA-Seq-only baseline?
3. How does the contribution of individual modalities influence multimodal classification outcomes?

## 2. Dataset and Study Design

### Data Source

Data were obtained from **The Cancer Genome Atlas (TCGA)**, specifically the lung adenocarcinoma (TCGA-LUAD) and lung squamous cell carcinoma (TCGA-LUSC) projects.

| Characteristic | Description |
|---|---|
| Data source | TCGA-LUAD and TCGA-LUSC |
| Classification task | Binary LUAD vs. LUSC |
| Matched cohort | 823 patients |
| Training set | 578 patients |
| Held-out test set | 245 patients |
| Modalities | RNA-Seq, DNA methylation, histopathology, clinical data |
| Primary feature setting | 40 features per modality |

### Data Modalities

The study integrates four complementary sources of information:

| Modality | Representation |
|---|---|
| RNA-Seq | Transcriptomic gene expression features |
| DNA methylation | Epigenetic methylation features |
| Histopathology | Whole-slide image (WSI) representations |
| Clinical data | Patient-level clinical variables |

All experiments use the same matched patient cohort to support controlled comparisons across modality combinations and fusion strategies.

## 3. Methodology

### 3.1 Data Preprocessing and Feature Extraction

Each modality underwent preprocessing and feature preparation before model training.

The workflow included:

1. Matching patients across TCGA molecular, histopathological, and clinical datasets.
2. Establishing consistent training and held-out test partitions.
3. Preparing modality-specific feature representations.
4. Extracting histopathology features using attention-based multiple instance learning (ABMIL).
5. Applying dimensionality reduction, including training-only principal component analysis (PCA), where appropriate.
6. Constructing equal-feature representations containing 40 features per modality.

The equal-feature setting was used for the primary controlled comparison, while original modality-specific representations were retained for sensitivity analysis.

Preprocessing operations requiring parameter estimation were restricted to training data to reduce information leakage.

### 3.2 Feature-Level Early Fusion

Early fusion integrates modality-specific features before classification.

Given feature vectors from \(M\) modalities, the fused representation is constructed as:

\[
\mathbf{x}_{\mathrm{early}}
=
[\mathbf{x}^{(1)} \Vert \mathbf{x}^{(2)} \Vert \cdots \Vert \mathbf{x}^{(M)}]
\]

where \(\Vert\) represents feature concatenation.

The combined representation is then used to train a single classification model.

Under the equal-feature setting, the full four-modality representation contains **160 features**, comprising 40 features from each modality.

### 3.3 Decision-Level Late Fusion

Late fusion trains independent classifiers for individual modalities and combines their predictions to obtain the final classification.

For modality \(m\):

\[
\hat{\mathbf{p}}^{(m)}=f_m(\mathbf{x}^{(m)})
\]

where \(f_m\) is the modality-specific classifier and \(\hat{\mathbf{p}}^{(m)}\) denotes its predicted class probabilities.

The combined prediction is obtained using:

\[
\hat{y}
=
g(\hat{\mathbf{p}}^{(1)},\hat{\mathbf{p}}^{(2)},\ldots,\hat{\mathbf{p}}^{(M)})
\]

where \(g\) represents the selected decision-aggregation rule.

This approach allows modality-specific models to learn independently while integrating their predictions during final decision-making.

### 3.4 Experimental Configurations

The study evaluated progressively larger modality combinations:

| Configuration | Included modalities |
|---|---|
| RNA-Seq baseline | RNA-Seq |
| Omics | RNA-Seq + DNA methylation |
| Clinical + Omics | RNA-Seq + DNA methylation + clinical data |
| Four-modality fusion | RNA-Seq + DNA methylation + clinical data + histopathology WSI |

Both early and late fusion were evaluated for multimodal configurations.

### 3.5 Model Training and Evaluation

Six classical machine-learning classifiers were evaluated under a consistent experimental protocol.

Model and fusion-rule selection used out-of-fold predictions from the training data. The held-out test set was reserved for final performance evaluation.

The following metrics were considered:

- Accuracy
- Macro-averaged F1 score
- Area under the receiver operating characteristic curve (AUC-ROC)
- Sensitivity
- Specificity

This controlled experimental design was intended to reduce differences attributable to cohort composition, feature dimensionality, and evaluation procedures.

## 4. Experimental Results

### 4.1 Classification Performance

The primary equal-feature analysis produced the following results:

| Model Configuration | Fusion Strategy | Accuracy (%) | AUC-ROC |
|---|---|---:|---:|
| RNA-Seq only | Unimodal | 97.55 | **0.9992** |
| Omics | Early | 97.14 | 0.9987 |
| Omics | Late | 97.14 | 0.9975 |
| Clinical + Omics | Early | 96.73 | 0.9985 |
| Clinical + Omics | Late | 97.14 | 0.9977 |
| Four modalities | Early | 96.33 | 0.9928 |
| Four modalities | Late | **97.96** | 0.9935 |

### 4.2 Key Findings

**RNA-Seq provides strong standalone discrimination.**

RNA-Seq alone achieved 97.55% classification accuracy and the highest reported AUC-ROC of 0.9992. This indicates that transcriptomic information contains substantial predictive signal for distinguishing LUAD from LUSC.

**Multimodal integration does not consistently improve performance.**

Adding DNA methylation, clinical information, and histopathology did not uniformly outperform the RNA-Seq-only baseline.

**Late fusion achieves the highest observed classification accuracy.**

In the full four-modality setting, decision-level late fusion achieved 97.96% accuracy, compared with 96.33% for feature-level early fusion—a difference of 1.63 percentage points.

**Accuracy and AUC-ROC identify different leading configurations.**

Although late fusion achieved the highest observed accuracy, RNA-Seq alone achieved the highest AUC-ROC. This distinction highlights the importance of evaluating multiple classification metrics.

### 4.3 Ablation Analysis

Modality-removal experiments were performed to assess the contribution of individual information sources.

Removing RNA-Seq from the full late-fusion model reduced accuracy from **97.96% to 95.10%**, representing a decline of **2.86 percentage points**.

This was the largest reported decline following removal of an individual modality, indicating that RNA-Seq contributed substantially to multimodal classification performance.

## 5. Discussion

The findings demonstrate that multimodal learning performance depends not only on the number and type of modalities included but also on their individual predictive contributions and the strategy used to integrate them.

RNA-Seq provided sufficiently strong discrimination that additional modalities offered limited and inconsistent improvements. Nevertheless, late fusion achieved higher observed accuracy than early fusion when all four heterogeneous modalities were combined.

These observations suggest that decision-level integration can be beneficial when combining substantially different molecular, clinical, and histopathological representations.

However, the performance differences are descriptive. Statistical significance has not been established, and the findings should not be interpreted as evidence of consistent superiority across independent cohorts.

## 6. Reproducibility

The repository is intended to provide the implementation supporting the reported experiments, including:

- Multimodal cohort preparation and preprocessing
- Feature extraction and dimensionality reduction
- Early-fusion and late-fusion model implementations
- Model training and fusion-rule selection
- Performance evaluation
- Modality ablation experiments
- Result visualization

The experimental workflow follows:

**TCGA data → Cohort matching → Feature extraction → Fusion strategies → Model training → Held-out evaluation → Results**

Execution instructions and software dependencies should be documented using the final released source files and configurations.

## 7. Data Availability

Data originate from [The Cancer Genome Atlas (TCGA)](https://portal.gdc.cancer.gov/).

Raw patient-level datasets and restricted data are not redistributed through this repository. Researchers should obtain the relevant datasets through authorized TCGA access channels.

## 8. Citation

If you use this work, please cite the accepted WiML @ NeurIPS 2026 submission:

```bibtex
@inproceedings{
riechi2026does,
title={Does Fusion Stage Matter? Comparing Feature-Level and Decision-Level Multimodal Learning for Lung Cancer Subtyping},
author={Gloria Moraa Riechi},
booktitle={Women in Machine Learning Workshop @ NeurIPS 2026},
year={2026},
url={https://openreview.net/forum?id=7Rp07uEd5s}
}
```

**[Read the Accepted Abstract (PDF)](https://openreview.net/pdf?id=7Rp07uEd5s)**

## 9. Contact

**Gloria Moraa Riechi**  
Department of Computational Data Science and Engineering  
North Carolina A&T State University

**Email:** gmriechi@aggies.ncat.edu
