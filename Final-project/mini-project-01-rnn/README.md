# Multivariate Weather Forecasting with RNNs

Multi-step temperature forecasting on the **Jena Climate** dataset using recurrent neural networks.

This project compares **LSTM, GRU, and Seq2Seq** models over a 24-hour forecasting horizon and includes an original **change-aware training** experiment designed to improve predictions during rapid temperature transitions.

## Project Summary

- **Dataset:** Jena Climate
- **Task:** Multivariate time-series forecasting
- **Input window:** Previous 72 hours
- **Forecast horizon:** Next 24 hours
- **Target:** Temperature (`T (degC)`)
- **Main models:** LSTM, GRU, Seq2Seq
- **Additional baselines:** Persistence, Seasonal Naive
- **Original contribution:** Change-Aware Training
- **Framework:** TensorFlow / Keras
- **Random seed:** 42

## Dataset

The project uses the **Jena Climate** dataset from the Max Planck Institute for Biogeochemistry.

The executed notebook loaded:

```text
420,551 observations
15 CSV columns
14 weather variables used as model inputs
```

The original measurements are recorded every 10 minutes. The notebook converts the data to hourly resolution before building forecasting windows.

If a Jena Climate CSV is available under `/kaggle/input`, the notebook uses it automatically. Otherwise, it downloads the public TensorFlow/Keras copy.

## Forecasting Setup

Each sample contains:

```text
Input:  72 hours × 14 weather features
Output: 24 future hourly temperature values
```

The data are split chronologically into training, validation, and test partitions.

Observed window counts in the executed notebook:

```text
Training windows:   48,968
Validation windows: 10,418
Test windows:       10,420
```

Normalization statistics are estimated from the training data only.

## Models

The following forecasting methods are evaluated:

1. Persistence baseline
2. Seasonal-naive baseline
3. LSTM
4. GRU
5. Seq2Seq

A GRU with additive temporal attention and a compact Transformer are also evaluated in supplementary experiments.

## Main Test Results

| Model          |  Mean MAE | Mean RMSE | MAE at h=1 | MAE at h=24 |
| -------------- | --------: | --------: | ---------: | ----------: |
| GRU            | **1.783** | **2.277** |      0.554 |       2.288 |
| LSTM           |     1.821 |     2.320 |      0.636 |       2.280 |
| Seq2Seq        |     1.865 |     2.367 |      0.781 |       2.355 |
| Seasonal Naive |     2.507 |     3.258 |      2.507 |       2.507 |
| Persistence    |     3.157 |     4.098 |      0.677 |       2.507 |

Under the common experimental setup, **GRU achieved the best overall recurrent-model performance**.

## Original Contribution — Change-Aware Training

The project first investigates whether rapid temperature changes are associated with higher forecasting error.

The proposed intervention gives greater training emphasis to windows containing rapid temperature transitions.

### Hypothesis

> Change-aware weighting will reduce MAE on rapid-change windows compared with standard GRU training, while keeping stable-regime MAE degradation below 5%.

Three schemes are compared:

- Uniform weighting
- Mild change-aware weighting
- Strong change-aware weighting

The weighting strength is selected using validation performance only.

### Result

The selected change-aware model produced a small improvement on rapid-change windows, while remaining within the predefined stable-regime degradation limit. However, the improvement was not statistically significant.

```text
Uniform rapid-window MAE:        2.08245
Selected change-aware MAE:       2.06000
Relative improvement:            ≈ 1.08%
One-sided Wilcoxon p-value:       0.36309
Bootstrap 95% CI for ΔMAE:       [-0.19092, 0.14271]
Stable-regime MAE degradation:   +1.42%
```

**Hypothesis decision: NOT SUPPORTED**

The negative result is retained because the experiment was hypothesis-driven and evaluated without retuning on the test set.

## Additional Experiments

### GRU with Temporal Attention

A GRU with additive attention over recurrent states is trained under the same general forecasting setup. Attention weights are inspected to analyze which historical timesteps receive greater emphasis.

### GRU vs Transformer

A compact Transformer is compared with the GRU.

Final test comparison:

| Model       |   Mean MAE |  Mean RMSE |
| ----------- | ---------: | ---------: |
| GRU         | **1.7832** | **2.2772** |
| Transformer |     2.6733 |     3.4306 |

The notebook also studies both architectures across:

- 25% and 100% training-data budgets
- 24, 72, and 168-hour input histories

### Prediction Intervals

A supplementary probabilistic experiment constructs empirical 90% prediction intervals using validation residual quantiles and evaluates interval coverage and width on the test set.

## Statistical Analysis

The notebook includes:

- Friedman test for baseline-family comparison
- Holm-adjusted paired Wilcoxon post-hoc tests
- Spearman correlation for failure-mode diagnosis
- One-sided paired Wilcoxon test for the original contribution
- Paired bootstrap confidence intervals
- Multi-seed robustness analysis

Non-overlapping forecast windows are used for inferential tests to reduce dependence from overlapping sliding windows.

## Repository Structure

```text
rnn-weather-forecasting/
├── README.md
├── report.pdf
├── requirements.txt
├── REFERENCES.md
├── rnn_weather_forecasting.ipynb
└── results/
```

Trained model checkpoints are **not included in the repository because of their file size**. They can be reproduced by running the notebook end-to-end.

## Installation

Python version used by the notebook metadata:

```text
Python 3.12.13
```

Install the required packages:

```bash
pip install -r requirements.txt
```

## Running the Project

1. Install the dependencies.
2. Open `rnn_weather_forecasting.ipynb`.
3. Run the notebook from top to bottom.
4. The notebook loads the Jena Climate dataset from Kaggle when available, or downloads the public TensorFlow/Keras copy.
5. Generated plots and result tables can be saved under `figures/` and `results/`.

## Reproducibility

The notebook uses a fixed seed:

```text
SEED = 42
```

The executed notebook reports:

```text
TensorFlow 2.20.0
GPU available: True
```

The main result can be reproduced by rerunning the full notebook; pretrained or saved model checkpoints are not required.

## Limitations

- The original 10-minute measurements are resampled to hourly resolution.
- The study focuses on one weather station and temperature as the forecast target.
- The model configurations are intentionally compact.
- The change-aware intervention showed only a small and statistically non-significant test improvement.
- The Transformer comparison covers only the tested data-size and sequence-length regimes.
- Prediction intervals are empirical residual-based intervals rather than a full probabilistic forecasting model.

## References

See [`REFERENCES.md`](REFERENCES.md) for dataset, framework, and library sources.

## Course Context

**Advanced Data Mining — Deep Learning Project**

- Architecture family: Recurrent Neural Networks
- Project: Multivariate Weather Forecasting
- Project value: 25 points
