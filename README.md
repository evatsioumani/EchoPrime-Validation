# EchoPrime External Validation

External validation of [EchoPrime](https://github.com/echonet/EchoPrime) on a Greek clinical echocardiography dataset from the **3rd Cardiology Department, Hippokration General Hospital, Thessaloniki, Greece**.

The project evaluates EchoPrime's zero-shot performance on data acquired with different clinical workflows and report language (Greek) than the original training set (Cedars-Sinai Medical Center, USA).



## Overview

EchoPrime is a foundation model for echocardiography interpretation. It was trained on over 1 million echocardiogram videos from Cedars-Sinai and evaluated across four distinct international healthcare sites. This repository provides the code for an external validation on a Greek hospital cohort, covering:

- **Ground truth extraction** from clinical echocardiography reports written in Greek, using regex-based NLP pipelines to extract both binary diagnostic labels and continuous measurements (EF, RVSP).
- **Model predictions** by running EchoPrime inference on the hospital's DICOM echocardiography studies.
- **Validation of binary classification tasks** (e.g., pacemaker, left ventricle systolic function) using AUROC, sensitivity, specificity, and bootstrap 95% confidence intervals.
- **Validation of regression tasks** (left ventricular ejection fraction, right ventricular systolic pressure) using MAE, R² and bootstrap 95% confidence intervals.



## Repository Structure

'''
EchoPrime-Validation/
│
├── Ground_truth_extraction_binary/      # NLP extraction of binary labels from Greek reports
├── Ground_truth_extraction_regression/  # NLP extraction of EF and RVSP values from Greek reports
├── Predictions/                         # EchoPrime inference pipeline on DICOM studies
├── Validation_for_binary_tasks/         # Evaluation: AUROC, sensitivity, specificity, CIs
├── Validation_for_regression_tasks/     # Evaluation: MAE, R², CIs
├── LICENSE
└── README.md
'''

### Ground Truth Extraction

Clinical echocardiography reports are stored as Compiled HTML Help (`.CHM`) files in Greek. The extraction pipeline uses 7-Zip for decompression, BeautifulSoup for HTML parsing, and regex patterns for structured information extraction.

**Binary labels** (`Ground_truth_extraction_binary/`): Extracts 17 binary diagnostic labels from section-specific text, including pacemaker presence, chamber dilation (LA, RA, RV), implanted devices (MitraClip, TAVR) etc.

**Regression targets** (`Ground_truth_extraction_regression/`): Extracts continuous values for left ventricular ejection fraction (LVEF) and right ventricular systolic pressure (RVSP) from report text.

### Predictions

Runs EchoPrime inference on the hospital's DICOM echocardiography studies using the pretrained model weights. Produces both binary classification outputs (17 pathology probabilities) and regression outputs (EF, RVSP) for each study.

### Validation

**Binary tasks** (`Validation_for_binary_tasks/`): Computes per-pathology AUROC, sensitivity, specificity, PPV, NPV, and balanced accuracy with 95% bootstrap confidence intervals (10,000 iterations).

**Regression tasks** (`Validation_for_regression_tasks/`): Computes MAE, RMSE, R² all with 95% bootstrap confidence intervals.



## Requirements

- Python 3.8+
- NVIDIA GPU with CUDA support (tested on RTX 3060 12GB)
- [EchoPrime](https://github.com/echonet/EchoPrime) (cloned separately)
- 7-Zip (for CHM report extraction on Windows)
