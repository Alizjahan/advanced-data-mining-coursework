# References

## Dataset

### Jena Climate Dataset

- **Dataset:** Jena Climate
- **Institution:** Max Planck Institute for Biogeochemistry
- **Description:** Multivariate weather measurements recorded at 10-minute intervals.
- **Dataset copy used by the notebook:** Kaggle Jena Climate mirror when available.
- **TensorFlow/Keras fallback dataset:**  
  https://storage.googleapis.com/tensorflow/tf-keras-datasets/jena_climate_2009_2016.csv.zip

The notebook automatically searches `/kaggle/input` for a Jena Climate CSV and uses the TensorFlow/Keras-hosted dataset as a fallback.

## Software and Libraries

### TensorFlow / Keras

Used for:

- LSTM model
- GRU model
- Seq2Seq model
- GRU with additive attention
- Compact Transformer
- Training, early stopping, prediction, and model construction

- TensorFlow: https://www.tensorflow.org/
- Keras: https://keras.io/

### NumPy

Used for numerical array operations, window construction, random seeding, and metric calculations.

- https://numpy.org/

### pandas

Used for loading, preprocessing, summarizing, and exporting tabular data.

- https://pandas.pydata.org/

### SciPy

Used for statistical inference, including rank-based tests and correlation analysis.

- https://scipy.org/

### Matplotlib

Used for plots and result visualizations.

- https://matplotlib.org/

## Project Methodology

This project implements the forecasting models and experiments directly in the accompanying notebook.

The original contribution is a hypothesis-driven **change-aware training** experiment. It evaluates whether giving greater training emphasis to rapid-temperature-change windows improves forecasting performance in those regimes.

The repository does not claim ownership of the Jena Climate dataset, TensorFlow/Keras, NumPy, pandas, SciPy, or Matplotlib.

## Course Source

Project specification:

**Data Mining — Deep Learning Project Handbook**  
Instructor: Dr. Hadi Farahani  
Term: Spring 2026

The project follows the handbook's RNN multivariate weather forecasting specification, including:

- Jena Climate data
- LSTM, GRU, and Seq2Seq comparison
- Increasing forecast horizons
- MAE/RMSE evaluation
- An original experimental contribution
- Required discussion and observation analysis
