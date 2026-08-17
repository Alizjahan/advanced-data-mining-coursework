# References

## Dataset

### AG News

- **Dataset:** AG News
- **Task:** Four-class news-topic classification
- **Classes:** World, Sports, Business, Sci/Tech
- **Access used in the notebook:** Hugging Face `datasets`

Hugging Face dataset page:

https://huggingface.co/datasets/ag_news

Original AG News corpus:

https://www.di.unipi.it/~gulli/AG_corpus_of_news_articles.html

## Pretrained Transformer

### DistilBERT

The project fine-tunes:

```text
distilbert-base-uncased
```

Model page:

https://huggingface.co/distilbert/distilbert-base-uncased

DistilBERT paper:

**Sanh, V., Debut, L., Chaumond, J., & Wolf, T.**  
*DistilBERT, a distilled version of BERT: smaller, faster, cheaper and lighter.*  
2019.

https://arxiv.org/abs/1910.01108

## Software and Libraries

### Hugging Face Transformers

Used for:

- tokenization
- loading DistilBERT
- sequence-classification model construction
- pretrained model loading

https://huggingface.co/docs/transformers/

### Hugging Face Datasets

Used to download and load AG News.

https://huggingface.co/docs/datasets/

### PyTorch

Used for:

- model training
- custom Dataset / DataLoader classes
- neural-network layers
- Text-CNN
- self-attention classifier
- hybrid CNN + self-attention classifier

https://pytorch.org/

### scikit-learn

Used for:

- stratified train/validation splitting
- TF-IDF vectorization
- Logistic Regression
- accuracy and Macro-F1
- classification reports
- confusion matrices
- log loss

https://scikit-learn.org/

### SciPy

Used for statistical utilities.

https://scipy.org/

### NumPy

Used for numerical operations, random seeding, and bootstrap calculations.

https://numpy.org/

### pandas

Used for tabular data handling, summaries, and result export.

https://pandas.pydata.org/

### Matplotlib

Used for:

- class-distribution plots
- validation curves
- data-efficiency curves
- confusion matrices
- error-profile plots
- reliability diagrams
- temperature-scaling plots
- CNN/self-attention comparison plots

https://matplotlib.org/

## Statistical Methods

### McNemar Test

The project uses an exact paired McNemar-style comparison through a binomial test on model-disagreement counts.

General reference:

**McNemar, Q.**  
*Note on the sampling error of the difference between correlated proportions or percentages.*  
Psychometrika, 1947.

### Bootstrap Confidence Interval

A paired bootstrap is used to estimate uncertainty in the difference in Macro-F1 between DistilBERT and TF-IDF.

## Project Methodology

The original contribution is a controlled **data-efficiency study**.

Both the classical baseline and DistilBERT are evaluated on the same stratified labeled-data budgets:

```text
500
2,000
8,000
```

The project measures how the performance gap changes as the available labeled data increase.

Additional analyses include:

- per-class metrics
- representative model disagreements
- exact paired statistical comparison
- paired bootstrap confidence interval
- calibration analysis
- temperature scaling
- Text-CNN vs self-attention vs hybrid comparison

## Course Source

**Data Mining — Deep Learning Project Handbook**  
Instructor: Dr. Hadi Farahani  
Term: Spring 2026

The project follows the handbook's **TF-1 Fine-Tuning a Transformer for Text Classification** specification:

- AG News dataset
- DistilBERT / BERT-style pretrained encoder
- TF-IDF + linear classical baseline
- controlled original contribution
- data-efficiency curve
- per-class metrics
- representative error cases
- convolution vs self-attention discussion
- hybrid architecture discussion
- optional calibration analysis
