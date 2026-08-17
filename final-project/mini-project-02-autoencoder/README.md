# ECG Anomaly Detection with an Autoencoder

Anomaly detection on the **ECG5000** dataset using a reconstruction-based Autoencoder trained only on normal heartbeats.

The project compares a standard reconstruction-error threshold with a validation-optimized threshold, analyzes failure cases across ECG anomaly classes, and includes supplementary experiments on bottleneck capacity, self-attention, multi-seed robustness, and Isolation Forest.

## Project Summary

- **Dataset:** ECG5000
- **Task:** Binary anomaly detection
- **Normal class:** ECG5000 class 1
- **Anomaly classes:** ECG5000 classes 2–5
- **Sequence length:** 140
- **Main model:** Dense Autoencoder
- **Anomaly score:** Reconstruction MSE
- **Original contribution:** Validation-optimized threshold selection
- **Framework:** TensorFlow / Keras
- **Random seed:** 42

## Dataset

The project uses the **ECG5000** time-series classification dataset.

The executed notebook loaded:

```text
Original TRAIN split: 500 samples
Original TEST split:  4,500 samples
Total:                5,000 samples
Sequence length:      140
```

The original TEST split is preserved as the final test set.

The original TRAIN split is divided into:

```text
Training:   350 samples
Validation: 150 samples
```

The Autoencoder itself is trained only on the normal beats from the training partition:

```text
Normal training beats: 204
```

## Preprocessing

- ECG labels are converted to binary normal/anomalous labels.
- Class 1 is treated as normal.
- Classes 2–5 are treated as anomalous.
- Scaling statistics are fitted **only on normal training beats**.
- The same fitted scaler is then applied to validation and test data.

This avoids using anomaly or test information during model fitting.

## Autoencoder Architecture

The main dense Autoencoder uses:

```text
140 → 64 → 32 → 8 → 32 → 64 → 140
```

Total trainable parameters:

```text
22,868
```

Training setup:

- Loss: Mean Squared Error
- Optimizer: Adam
- Maximum epochs: 200
- Batch size: 128
- Validation data: normal validation beats
- Fixed random seed: 42

The executed run reached 200 epochs.

## Baseline Anomaly Detection

The baseline threshold is the **95th percentile of reconstruction errors on normal validation beats**.

Observed threshold:

```text
0.700382
```

Median test reconstruction errors:

```text
Normal:    0.128479
Anomalous: 1.825507
```

This shows strong separation between normal and anomalous reconstruction errors.

## Original Contribution — Validation-Optimized Threshold

The proposed intervention keeps the same trained Autoencoder and reconstruction errors, but changes the threshold-selection rule.

### Baseline

```text
95th percentile of normal validation reconstruction errors
```

### Proposed Method

The threshold is selected from the validation precision-recall curve to maximize validation F1.

Observed optimized threshold:

```text
1.350430
```

Validation F1:

```text
Baseline threshold:  0.9531
Optimized threshold: 0.9756
```

## Final Test Results

| Method | Precision | Recall | F1 | Accuracy |
|---|---:|---:|---:|---:|
| 95th-percentile baseline | 0.9439 | **0.9797** | **0.9615** | **0.9673** |
| Validation-optimized threshold | **0.9742** | 0.8655 | 0.9166 | 0.9344 |

Observed change:

```text
Δ Precision: +0.0302
Δ Recall:    -0.1143
Δ F1:        -0.0449
```

The optimized threshold improved precision but reduced recall and overall F1 on the final test set.

**Conclusion:** the expected F1/recall improvement was not consistently supported.

The negative result is retained as part of the project rather than being hidden or retuned on the test set.

## Statistical Analysis

### Reconstruction-Error Separation

A one-sided Mann-Whitney U test evaluates whether anomalous beats have larger reconstruction errors than normal beats.

Observed common-language effect size:

```text
P(error_anomalous > error_normal) = 0.986153
```

The notebook reports that the p-value is below floating-point resolution.

### Paired Bootstrap for Threshold Intervention

A 2,000-resample paired bootstrap estimates uncertainty in the threshold comparison.

| Metric Change | Observed | 95% CI Low | 95% CI High |
|---|---:|---:|---:|
| Δ Precision | +0.0302 | +0.0227 | +0.0384 |
| Δ Recall | -0.1143 | -0.1289 | -0.0996 |
| Δ F1 | -0.0449 | -0.0539 | -0.0359 |

## Error Analysis

Using the validation-optimized threshold:

```text
False positives: 43
False negatives: 252
```

Per-class anomaly miss rates:

| ECG5000 Class | Test Count | False Negatives | Miss Rate |
|---|---:|---:|---:|
| 2 | 1590 | 153 | 0.0962 |
| 3 | 86 | 15 | 0.1744 |
| 4 | 175 | 76 | **0.4343** |
| 5 | 22 | 8 | 0.3636 |

Class 4 is the largest identified failure mode in the final test set.

The notebook also visualizes representative false positives and false negatives.

## Bottleneck Capacity Study

To investigate the reconstruction paradox, the notebook compares bottleneck sizes 2, 8, and 32.

| Bottleneck | Error Contrast Ratio | Precision | Recall | F1 |
|---:|---:|---:|---:|---:|
| 2 | **11.1294** | 0.9685 | 0.9525 | **0.9604** |
| 8 | 8.8785 | 0.9742 | 0.8655 | 0.9166 |
| 32 | 7.2171 | 0.9323 | **0.9851** | 0.9579 |

The smallest bottleneck produced the strongest anomaly/normal error contrast and the best F1 in this experiment.

## Temporal Reconstruction-Error Analysis

The notebook measures reconstruction error across time positions and identifies the largest abnormal-vs-normal gaps around:

```text
[5, 4, 16, 6, 136, 18, 15, 133]
```

This provides an empirical view of which parts of the heartbeat waveform most strongly separate anomalous and normal examples.

## Self-Attention Ablation

A controlled sequence-Autoencoder experiment compares:

- Convolutional Autoencoder without self-attention
- Convolutional Autoencoder with self-attention

Results:

| Model | Precision | Recall | F1 | Error Contrast |
|---|---:|---:|---:|---:|
| No attention | 0.7726 | 0.8398 | 0.8048 | 2.2669 |
| Self-attention | **0.9030** | **0.9744** | **0.9373** | **9.6741** |

Observed F1 improvement:

```text
+0.1325
```

The highest average attention positions were:

```text
[5, 6, 7, 4, 123, 124, 122, 125, 126, 121]
```

## Multi-Seed Robustness

The threshold comparison is repeated across five seeds:

```text
7, 21, 42, 77, 123
```

Mean change across seeds:

```text
Δ Precision: +0.0123 ± 0.0221
Δ Recall:    -0.0466 ± 0.0689
Δ F1:        -0.0181 ± 0.0244
```

The optimized threshold did not produce a consistent F1 improvement across seeds.

## Bonus — Isolation Forest

An Isolation Forest trained only on normal training beats is included as a stretch baseline.

| Method | Precision | Recall | F1 | Accuracy |
|---|---:|---:|---:|---:|
| Autoencoder baseline | **0.9439** | **0.9797** | **0.9615** | **0.9673** |
| Validation-optimized AE | 0.9742 | 0.8655 | 0.9166 | 0.9344 |
| Isolation Forest | 0.7246 | 0.1335 | 0.2254 | 0.6182 |

Isolation Forest average precision:

```text
0.8274
```

## Repository Structure

```text
ecg-anomaly-detection-autoencoder/
├── README.md
├── report.pdf
├── requirements.txt
├── REFERENCES.md
├── ecg_anomaly_detection_autoencoder.ipynb
└── results/         
```

Trained model checkpoints are **not included in the repository because of file-size constraints**. The model can be reproduced by running the notebook end-to-end.

## Installation

The executed notebook metadata reports:

```text
Python 3.12.13
TensorFlow 2.20.0
```

Install the project dependencies:

```bash
pip install -r requirements.txt
```

## Running the Project

1. Install the dependencies.
2. Provide the ECG5000 TRAIN and TEST files in `.txt` or `.arff` format.
3. On Kaggle, attach the ECG5000 dataset under Kaggle Input.
4. Open `ecg_anomaly_detection_autoencoder.ipynb`.
5. Run the notebook from top to bottom.

The loader searches recursively for:

```text
ECG5000_TRAIN
ECG5000_TEST
```

## Saved Outputs

The executed notebook generated the following result files:

```text
bonus_isolation_forest_comparison.csv
bottleneck_comparison.csv
final_metrics.csv
mannwhitney_test.csv
multi_seed_summary.csv
multi_seed_threshold_results.csv
per_class_miss_rates.csv
reconstruction_error_summary.csv
score_metrics.csv
self_attention_ablation.csv
threshold_bootstrap_ci.csv
threshold_comparison.csv
threshold_effect.csv
```

It also generated 12 figures covering:

- example signals
- training curves
- reconstruction-error distributions
- threshold sweep
- confusion matrices
- metric comparison
- precision-recall curve
- failure cases
- bottleneck comparison
- temporal error profile
- self-attention ablation
- self-attention profile

## Reproducibility

The notebook uses:

```text
SEED = 42
```

and attempts to enable deterministic TensorFlow operations.

Python, NumPy, and TensorFlow random seeds are fixed before training.

## Limitations

- ECG5000 is a relatively small benchmark dataset.
- The Autoencoder is trained from only 204 normal training beats.
- The validation-optimized threshold is supervised and requires labeled validation anomalies.
- Optimizing F1 on a small validation set can overfit the threshold.
- The dense baseline does not explicitly encode temporal locality.
- The threshold intervention did not improve test F1 in the main run.
- Multi-seed experiments show that the intervention effect is not stable across seeds.
- Failure analysis by original ECG class is descriptive and is not used to retune the final test result.

## References

See [`REFERENCES.md`](REFERENCES.md) for dataset, framework, and library sources.

## Course Context

**Advanced Data Mining — Deep Learning Project**

- Architecture family: Autoencoder
- Project: ECG Anomaly Detection with an Autoencoder
- Project value: 20 points
