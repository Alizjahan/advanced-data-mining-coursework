# Transformer Text Classification with DistilBERT

Four-class news-topic classification on the **AG News** dataset using a classical **TF-IDF + Logistic Regression** baseline and a fine-tuned **DistilBERT** Transformer.

The main original contribution is a controlled **data-efficiency study** that compares both approaches at multiple labeled-data budgets.

## Project Summary

- **Dataset:** AG News
- **Task:** Text classification
- **Classes:** World, Sports, Business, Sci/Tech
- **Classical baseline:** TF-IDF + Logistic Regression
- **Transformer:** DistilBERT (`distilbert-base-uncased`)
- **Original contribution:** Data-efficiency curve
- **Primary metrics:** Accuracy and Macro-F1
- **Random seed:** 42
- **Framework:** PyTorch + Hugging Face Transformers

## Dataset

The project uses **AG News** through the Hugging Face `datasets` library.

Observed dataset sizes in the executed notebook:

```text
Training split: 120,000
Test split:       7,600
```

The original training split is divided into:

```text
Training pool: 108,000
Validation:     12,000
Test:            7,600
```

The test set is kept untouched until final evaluation.

Class mapping:

```text
0: World
1: Sports
2: Business
3: Sci/Tech
```

## Controlled Training Budgets

Three stratified subsets are sampled from the training pool:

```text
500 samples
2,000 samples
8,000 samples
```

Each subset is exactly class-balanced.

This allows the labeled-data budget to be varied while keeping the task, labels, and evaluation set fixed.

## Classical Baseline

The classical baseline is:

```text
TF-IDF Vectorizer + Logistic Regression
```

Validation performance:

| Train Size | Accuracy | Macro-F1 |
|---:|---:|---:|
| 500 | 0.7839 | 0.7830 |
| 2,000 | 0.8654 | 0.8648 |
| 8,000 | 0.8962 | 0.8958 |

## Transformer Model

The Transformer baseline uses:

```text
distilbert-base-uncased
```

with a sequence-classification head.

The notebook fine-tunes DistilBERT separately at each labeled-data budget and selects the best checkpoint using validation Macro-F1.

Validation performance:

| Train Size | Accuracy | Macro-F1 |
|---:|---:|---:|
| 500 | 0.8748 | 0.8748 |
| 2,000 | 0.9046 | 0.9043 |
| 8,000 | 0.9126 | 0.9128 |

## Original Contribution — Data-Efficiency Study

### Hypothesis

The project tests whether pretrained DistilBERT provides a larger advantage over TF-IDF in low-data regimes and still maintains an advantage at the largest controlled training budget.

The comparison is performed on the same stratified training subsets:

```text
500
2,000
8,000
```

Validation Macro-F1 gaps:

```text
500 samples:   +0.0918
2,000 samples: +0.0395
8,000 samples: +0.0170
```

Mean low-data gap across 500 and 2,000 samples:

```text
0.0656
```

Final validation gap at 8,000 samples:

```text
0.0170
```

**Validation hypothesis decision: SUPPORTED**

The results show that DistilBERT has its largest advantage in the lower-data settings, while the gap narrows as more labeled data become available.

## Final Test Results

Both final models use the same 8,000-example labeled-data budget.

| Model | Accuracy | Macro-F1 |
|---|---:|---:|
| TF-IDF + Logistic Regression | 0.8913 | 0.8909 |
| DistilBERT | **0.9103** | **0.9105** |

DistilBERT outperformed the classical baseline on both final metrics.

## Per-Class Results

DistilBERT improved F1 for every AG News class.

| Class | TF-IDF F1 | DistilBERT F1 | Δ F1 |
|---|---:|---:|---:|
| World | 0.8977 | 0.9124 | +0.0147 |
| Sports | 0.9523 | 0.9621 | +0.0098 |
| Business | 0.8525 | 0.8737 | +0.0213 |
| Sci/Tech | 0.8612 | 0.8936 | +0.0324 |

The largest per-class gain was observed for **Sci/Tech**.

## Paired Statistical Comparison

Because both models predict the same test examples, the notebook performs an exact McNemar comparison.

Observed disagreements:

```text
DistilBERT correct / TF-IDF wrong: 424
TF-IDF correct / DistilBERT wrong: 280
```

The notebook also performs a paired bootstrap analysis for the Macro-F1 difference.

```text
Δ Macro-F1:          +0.01956
Bootstrap 95% CI:    [0.01277, 0.02637]
```

These results support a measurable final-test advantage for DistilBERT under the tested setup.

## Error Analysis

The notebook includes:

- Representative cases where DistilBERT is wrong and TF-IDF is correct
- Representative cases where TF-IDF is wrong and DistilBERT is correct
- Prediction confidence analysis
- Text-length analysis
- Confusion matrices
- Per-class performance comparison

Observed DistilBERT test summary:

```text
Correct predictions:   6,918
Incorrect predictions:   682
```

Mean prediction confidence:

```text
Correct cases:   0.957
Incorrect cases: 0.807
```

## Bonus — Calibration Analysis

Expected Calibration Error (ECE):

| Model | ECE |
|---|---:|
| TF-IDF + Logistic Regression | 0.1878 |
| DistilBERT | **0.0333** |

The notebook also evaluates post-hoc temperature scaling for DistilBERT.

Observed result:

```text
Temperature:          1.2
ECE before:           0.0333
ECE after:            0.0165
NLL before:           0.2747
NLL after:            0.2681
```

Temperature scaling improved both ECE and negative log-likelihood in this run.

## Convolution vs Self-Attention Experiment

To address the required discussion questions empirically, the notebook trains three lightweight text classifiers using the same 8,000-example training budget:

- Text-CNN
- Self-attention classifier
- CNN + self-attention hybrid

Validation results:

| Model | Accuracy | Macro-F1 |
|---|---:|---:|
| CNN + self-attention | **0.8097** | **0.8094** |
| Text-CNN | 0.8093 | 0.8091 |
| Self-attention | 0.8072 | 0.8058 |

The notebook also extracts concrete examples where the convolutional and attention-based classifiers disagree.

## Repository Structure

```text
transformer-text-classification/
├── README.md
├── requirements.txt
├── REFERENCES.md
├── transformer_text_classification.ipynb
├── figures/
└── results/
```

Trained model checkpoints are **not included in the repository because of file-size constraints**. DistilBERT can be downloaded from Hugging Face and fine-tuned again by running the notebook end-to-end.

## Installation

Install the dependencies:

```bash
pip install -r requirements.txt
```

## Running the Project

1. Install the dependencies.
2. Open `transformer_text_classification.ipynb`.
3. Run the notebook from top to bottom.
4. Internet access is required the first time to download AG News and `distilbert-base-uncased`.
5. Generated figures and CSV/JSON result files can be stored under `figures/` and `results/`.

## Saved Outputs

The executed notebook generated the following result files:

```text
calibration_summary.csv
discussion_cnn_attention_disagreements.csv
discussion_cnn_attention_hybrid.csv
error_profile_summary.csv
exact_mcnemar_test.csv
final_model_summary.csv
hypothesis_decision_validation.csv
label_map.json
per_class_model_comparison.csv
representative_error_cases.csv
statistical_comparison.csv
temperature_scaling_results.csv
validation_data_efficiency_results.csv
```

Figures generated by the notebook include:

```text
01_class_distribution.png
02_transformer_validation_curves.png
03_data_efficiency_curve.png
04_confusion_matrices.png
05_error_profile.png
06_reliability_diagram.png
07_temperature_scaling.png
discussion_cnn_attention_hybrid.png
```

## Reproducibility

The notebook uses:

```text
SEED = 42
```

and sets random seeds for:

- Python
- NumPy
- PyTorch
- CUDA, when available

The executed notebook used a CUDA device.

## Limitations

- AG News is a relatively clean four-class benchmark.
- DistilBERT benefits from large-scale language-model pretraining, whereas TF-IDF does not.
- The experiment controls labeled-data budget, not total compute.
- The data-efficiency curve uses only three labeled-data budgets.
- DistilBERT fine-tuning results may vary with hardware, package versions, and random state.
- Calibration results depend on the chosen ECE binning procedure.
- The CNN/self-attention experiment is a lightweight controlled comparison rather than a full architecture search.

## References

See [`REFERENCES.md`](REFERENCES.md) for dataset, model, and software sources.

## Course Context

**Advanced Data Mining — Deep Learning Project**

- Architecture family: Transformer
- Project: Fine-Tuning a Transformer for Text Classification
- Project value: 25 points
