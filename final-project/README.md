# Final Project — Advanced Data Mining

A multi-part deep learning project covering four architecture families through practical experiments in time series, anomaly detection, NLP, and computer vision.

Each mini-project includes a baseline, a stated hypothesis, an original contribution, quantitative evaluation, error analysis, and reproducible experiments.

---

## Project Overview

| # | Mini-Project | Architecture | Dataset | Main Task |
|---|---|---|---|---|
| 01 | Weather Forecasting | RNN | Jena Climate | Multi-step time-series forecasting |
| 02 | ECG Anomaly Detection | Autoencoder | ECG5000 | Reconstruction-based anomaly detection |
| 03 | News Classification | Transformer | AG News | Text classification |
| 04 | Plant Disease Classification | Vision Transformer | PlantVillage | Image classification |

---

## Mini-Projects

### 01 — Multivariate Weather Forecasting

**Architecture:** RNN  
**Dataset:** Jena Climate

Multi-step temperature forecasting using recurrent neural networks and several forecasting baselines.

**Models**
- Persistence
- Seasonal Naive
- LSTM
- GRU
- Seq2Seq
- GRU + Attention
- Compact Transformer baseline

**Original Contribution**

Change-Aware Sample-Weighted Training, designed to give greater training emphasis to windows containing rapid temperature transitions.

**Additional Experiment**

Probabilistic forecasting with empirical prediction intervals.

[View Mini-Project 01](./mini-project-01-rnn/)

---

### 02 — ECG Anomaly Detection

**Architecture:** Autoencoder  
**Dataset:** ECG5000

An Autoencoder is trained only on normal heartbeats and anomalies are detected using reconstruction error.

**Model**
- Dense Autoencoder

**Original Contribution**

A validation-optimized reconstruction-error threshold is compared with a fixed percentile-based threshold.

**Additional Experiment**

Comparison with a classical anomaly-detection baseline.

[View Mini-Project 02](./mini-project-02-autoencoder/)

---

### 03 — Transformer Text Classification

**Architecture:** Transformer  
**Dataset:** AG News

News articles are classified into four topic categories using a classical NLP baseline and a pretrained Transformer.

**Models**
- TF-IDF + Logistic Regression
- DistilBERT

**Original Contribution**

A data-efficiency study comparing TF-IDF and DistilBERT across increasing labeled-data budgets.

**Additional Experiment**

Calibration analysis using temperature scaling.

[View Mini-Project 03](./mini-project-03-transformer/)

---

### 04 — Plant Disease Classification

**Architecture:** Vision Transformer  
**Dataset:** PlantVillage

A pretrained Vision Transformer and a ResNet-50 CNN are compared under a matched experimental setup.

**Models**
- ViT-B/16
- ResNet-50

**Original Contribution**

A controlled layer-unfreezing study comparing:

- classifier head only
- final two Transformer blocks + classifier head
- full fine-tuning

The experiment studies the trade-off between predictive performance and the number of trainable parameters.

**Additional Experiment**

Data-efficiency comparison between ViT and CNN.

[View Mini-Project 04](./mini-project-04-vit/)

---

## Repository Structure

```text
final-project/
│
├── README.md
│
├── mini-project-01-rnn/
│   ├── README.md
│   ├── report.pdf
│   ├── requirements.txt
│   ├── REFERENCES.md
│   ├── rnn_weather_forecasting.ipynb
│   └── results/
│
├── mini-project-02-autoencoder/
│   ├── README.md
│   ├── report.pdf
│   ├── requirements.txt
│   ├── REFERENCES.md
│   ├── ecg_anomaly_detection_autoencoder.ipynb
│   └── results/
│
├── mini-project-03-transformer/
│   ├── README.md
│   ├── report.pdf
│   ├── requirements.txt
│   ├── REFERENCES.md
│   ├── transformer_text_classification.ipynb
│   └── results/
│
└── mini-project-04-vit/
    ├── README.md
    ├── report.pdf
    ├── requirements.txt
    ├── REFERENCES.md
    ├── vit_plant_disease_classification.ipynb
    └── results/
```

---

## Experimental Design

The four mini-projects follow a consistent experimental workflow:

- fixed random seeds
- explicit train / validation / test separation
- appropriate baseline models
- hypothesis-driven original contributions
- controlled ablation or comparison
- task-specific evaluation metrics
- statistical analysis where appropriate
- error and failure analysis
- reproducible saved outputs

---

## Architecture Coverage

```text
Time Series        → RNN
Anomaly Detection  → Autoencoder
Natural Language   → Transformer
Computer Vision    → Vision Transformer
```

This project therefore spans four distinct deep learning architecture families.

---

## Reproducibility

Each mini-project contains its own README with details on:

- dataset preparation
- model configuration
- experimental setup
- original contribution
- evaluation metrics
- main results
- execution instructions

The notebooks are designed primarily for execution on Kaggle and save relevant figures, result tables, and model outputs to dedicated directories.

---

## Course Information

**Course:** Advanced Data Mining  
**University:** Shahid Beheshti University  
**Instructor:** Dr. Hadi Farahani  
**Head TA:** Mr. Mobin Nesari  
**Student:** Alireza Jahanbakhsh  
**Term:** Spring 2026
