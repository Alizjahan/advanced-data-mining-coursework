# References

## Dataset

### ECG5000

- **Dataset:** ECG5000
- **Source family:** UCR / UEA Time Series Classification Archive
- **Samples:** 5,000 heartbeat sequences
- **Sequence length:** 140
- **Classes:** 5 original ECG classes
- **Project labeling:** class 1 = normal; classes 2–5 = anomalous

Official archive:

https://www.timeseriesclassification.com/

The project uses the standard ECG5000 TRAIN and TEST files in `.txt` or `.arff` form.

## Software and Libraries

### TensorFlow / Keras

Used to build and train:

- Dense Autoencoder baseline
- Bottleneck-capacity variants
- Convolutional sequence Autoencoder
- Self-attention Autoencoder

Official documentation:

https://www.tensorflow.org/

https://keras.io/

### scikit-learn

Used for:

- train/validation splitting
- StandardScaler
- precision, recall, F1, accuracy
- precision-recall curve
- confusion matrices
- Isolation Forest bonus baseline

Official documentation:

https://scikit-learn.org/

### SciPy

Used for:

- Mann-Whitney U statistical testing
- ARFF dataset loading

Official documentation:

https://scipy.org/

### NumPy

Used for:

- numerical array operations
- reconstruction-error calculations
- bootstrap resampling
- random seeding

Official documentation:

https://numpy.org/

### pandas

Used for:

- tabular summaries
- result tables
- CSV result export

Official documentation:

https://pandas.pydata.org/

### Matplotlib

Used for:

- ECG waveform plots
- training curves
- reconstruction-error distributions
- confusion matrices
- precision-recall plots
- error analysis
- attention visualizations

Official documentation:

https://matplotlib.org/

## Project Methodology

The implementation and experiments are contained in the accompanying notebook.

The main original contribution is a controlled threshold-selection experiment:

1. Train one Autoencoder only on normal ECG beats.
2. Compute reconstruction-error anomaly scores.
3. Compare a fixed 95th-percentile normal-validation threshold with a labeled validation-F1 optimized threshold.
4. Evaluate both threshold rules on the same untouched test set.
5. Report precision, recall, F1, bootstrap uncertainty, and failure cases.

Supplementary experiments include:

- bottleneck-capacity study
- temporal reconstruction-error analysis
- controlled self-attention ablation
- multi-seed robustness
- Isolation Forest bonus baseline

## Course Source

**Data Mining — Deep Learning Project Handbook**  
Instructor: Dr. Hadi Farahani  
Term: Spring 2026

The project follows the handbook's **AE-1 ECG Anomaly Detection with an Autoencoder** specification:

- ECG5000 dataset
- Autoencoder trained only on normal beats
- reconstruction-error anomaly detection
- threshold selection
- precision / recall / F1 evaluation
- original threshold or architecture contribution
- error-distribution analysis
- failure-case analysis
- discussion of reconstruction capacity
- discussion / experiment involving self-attention
- optional Isolation Forest comparison
