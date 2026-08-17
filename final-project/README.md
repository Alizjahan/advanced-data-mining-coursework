# Final Project — Advanced Data Mining

This final project consists of four deep learning mini-projects covering four architecture families:

- Recurrent Neural Networks
- Autoencoders
- Transformers
- Vision Transformers

Each mini-project includes a complete experimental pipeline, a stated hypothesis, an original contribution, quantitative evaluation, error analysis, and the required discussion questions from the course handbook.

## Mini-Projects

### 1. Multivariate Weather Forecasting — RNN

**Dataset:** Jena Climate  
**Task:** Multi-step temperature forecasting from multivariate weather observations.

**Models:**
- Persistence baseline
- Seasonal naive baseline
- LSTM
- GRU
- Seq2Seq
- GRU + Attention
- Compact Transformer baseline

**Original Contribution:**  
Change-Aware Sample-Weighted Training

The experiment investigates whether giving greater training emphasis to rapid temperature transitions can reduce forecasting error during difficult weather changes.

**Stretch:**  
Probabilistic forecasting with prediction intervals.

---

### 2. ECG Anomaly Detection — Autoencoder

**Dataset:** ECG5000  
**Task:** Detect abnormal heartbeats using reconstruction error from an Autoencoder trained only on normal samples.

**Model:**
- Dense Autoencoder

**Original Contribution:**  
Validation-Optimized Reconstruction-Error Threshold

A fixed 95th-percentile threshold is compared with a threshold selected using validation F1.

**Stretch:**  
Comparison with an additional anomaly-detection baseline.

---

### 3. Transformer Text Classification

**Dataset:** AG News  
**Task:** Classify news articles into four topic categories.

**Models:**
- TF-IDF + Logistic Regression
- DistilBERT

**Original Contribution:**  
Data-Efficiency Study

The experiment compares the classical baseline and pretrained Transformer across increasing amounts of labeled training data.

**Stretch:**  
Calibration analysis with temperature scaling.

---

### 4. Plant Disease Classification — Vision Transformer

**Dataset:** PlantVillage  
**Task:** Classify crop-leaf diseases using a Vision Transformer and a CNN baseline.

**Models:**
- ViT-B/16
- ResNet-50

**Original Contribution:**  
Layer-Unfreezing Strategy Comparison

Three Vision Transformer fine-tuning strategies are compared:

- Classifier head only
- Final two Transformer blocks + classifier head
- Full fine-tuning

The study evaluates the trade-off between predictive performance and the number of trainable parameters.

**Stretch:**  
Data-efficiency comparison between ViT and CNN.

---

## Project Structure

```text
final-project/
├── README.md
│
├── mini-project-01-rnn/
│   ├── README.md
│   ├── notebook.ipynb
│   └── results/
│
├── mini-project-02-autoencoder/
│   ├── README.md
│   ├── notebook.ipynb
│   └── results/
│
├── mini-project-03-transformer/
│   ├── README.md
│   ├── notebook.ipynb
│   └── results/
│
└── mini-project-04-vit/
    ├── README.md
    ├── notebook.ipynb
    └── results/
```

## Experimental Principles

Across the four mini-projects, the experiments follow a common structure:

- Fixed random seeds for reproducibility
- Explicit train/validation/test separation
- Appropriate baseline models
- A stated hypothesis before the main experiment
- Controlled comparison of the original contribution
- Task-appropriate evaluation metrics
- Error and failure analysis
- Honest interpretation of negative or mixed results
- Saved figures and result tables

## Covered Architecture Families

| Mini-Project | Architecture Family | Main Task |
|---|---|---|
| Weather Forecasting | RNN | Time-series forecasting |
| ECG Anomaly Detection | Autoencoder | Anomaly detection |
| AG News Classification | Transformer | Text classification |
| Plant Disease Classification | Vision Transformer | Image classification |

## Reproducibility

Each mini-project contains its own README with:

- Dataset information
- Experimental setup
- Original contribution
- Main results
- Instructions for running the notebook

The notebooks were designed for execution on Kaggle and save their main outputs to dedicated result and figure directories.

## Course Information

**Course:** Advanced Data Mining  
**University:** Shahid Beheshti University  
**Instructor:** Dr. Hadi Farahani  
**Head TA:** Mr. Mobin Nesari  
**Student:** Alireza Jahanbakhsh  
**Term:** Spring 2026