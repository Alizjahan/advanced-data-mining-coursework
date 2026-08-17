# Vision Transformer vs CNN for Plant Disease Classification

Plant disease classification on the **PlantVillage** dataset using a pretrained **Vision Transformer (ViT-B/16)** and a pretrained **ResNet-50**.

The main original contribution is a controlled **ViT layer-unfreezing study** that compares head-only fine-tuning, partial unfreezing, and full fine-tuning under the same data split and training budget.

## Project Summary

- **Dataset:** PlantVillage
- **Task:** 38-class plant disease classification
- **Vision model:** Pretrained ViT-B/16
- **CNN baseline:** Pretrained ResNet-50
- **Original contribution:** Layer-unfreezing strategy comparison
- **Primary metrics:** Accuracy and Macro-F1
- **Image size:** 224 × 224
- **Random seed:** 42
- **Framework:** PyTorch + torchvision + timm

## Dataset

The notebook uses the **PlantVillage** image dataset.

The executed notebook found:

```text
Dataset images: 43,444
Classes: 38
```

For computational practicality, the notebook samples a fixed maximum number of images per class before splitting. The resulting experiment therefore uses a controlled subset rather than preserving the original class prevalence exactly.

Observed subset and split:

```text
Selected images: 6,041
Training:        4,228
Validation:        906
Test:              907
```

The same split is used for every main model, and the test set remains untouched until final evaluation.

## Preprocessing

Both ViT and ResNet use:

```text
Image size: 224 × 224
Color mode: RGB
Normalization: ImageNet normalization
```

Training uses light horizontal flipping.

Validation and test transformations are deterministic.

## Common Training Protocol

The main ViT and ResNet experiments use the same:

- train/validation/test split
- image resolution
- batch size
- number of epochs
- optimizer family
- classification loss
- validation-selection metric

The best model configuration is selected by validation Macro-F1.

The project matches the main experimental settings, but it does **not** claim equal FLOPs or equal trainable-parameter counts between ViT and ResNet.

## CNN Baseline — ResNet-50

A pretrained ResNet-50 is fully fine-tuned after replacing its final classifier with a 38-class output layer.

Trainable parameters:

```text
23,585,894
```

Validation Macro-F1:

```text
Epoch 1: 0.9401
Epoch 2: 0.9631
Best:    0.9631
```

## Original Contribution — ViT Layer-Unfreezing Study

Three ViT-B/16 strategies are compared:

1. Classifier head only
2. Final two Transformer blocks + normalization + classifier head
3. Full fine-tuning

### Hypothesis

Partial unfreezing should substantially outperform head-only fine-tuning while remaining competitive with full fine-tuning and updating substantially fewer parameters.

## ViT Ablation Results

| Strategy | Validation Accuracy | Validation Macro-F1 | Trainable Parameters | Training Time (s) |
|---|---:|---:|---:|---:|
| Head only | 0.8775 | 0.8724 | 29,222 | 147.45 |
| Last 2 blocks | **0.9647** | **0.9649** | 14,206,502 | 182.98 |
| Full fine-tuning | 0.9272 | 0.9290 | 85,827,878 | 367.19 |

Selected strategy:

```text
Last two Transformer blocks + head
```

**Hypothesis decision: SUPPORTED**

The partial-unfreezing strategy achieved the highest validation Macro-F1 while using far fewer trainable parameters than full fine-tuning.

## Final Test Results

| Model | Accuracy | Macro-F1 | Trainable Parameters |
|---|---:|---:|---:|
| ViT-B/16 — last 2 blocks | **0.9713** | **0.9713** | 14,206,502 |
| ResNet-50 | 0.9691 | 0.9691 | 23,585,894 |

The raw test metrics slightly favor the selected ViT configuration.

## Paired Statistical Comparison

Because both models classify the same test images, the notebook performs a paired comparison.

Observed disagreement counts:

```text
ViT correct / ResNet wrong: 23
ResNet correct / ViT wrong: 21
```

Statistical results:

```text
Two-sided McNemar p-value:          0.8804
Δ Macro-F1 (ViT − ResNet):         +0.00213
Bootstrap 95% CI for Δ Macro-F1:   [-0.01242, 0.01619]
```

The point estimate slightly favors ViT, but the confidence interval includes zero and the McNemar result is not statistically significant.

## Per-Class Disagreement Analysis

The notebook identifies classes where the two models differ most in F1.

Examples of classes favoring ViT:

| Class | ViT F1 | ResNet F1 | Difference |
|---|---:|---:|---:|
| Tomato — Late blight | 0.980 | 0.846 | +0.133 |
| Tomato — Early blight | 0.958 | 0.844 | +0.114 |
| Corn — Northern Leaf Blight | 0.941 | 0.889 | +0.052 |
| Potato — Late blight | 0.958 | 0.909 | +0.049 |

The notebook also stores representative images where:

- ViT is correct and ResNet is wrong
- ResNet is correct and ViT is wrong

## ViT Attention vs ResNet Grad-CAM

The project directly compares:

- final-block ViT CLS-to-patch attention
- ResNet-50 Grad-CAM

on the **same test images**.

The comparison is qualitative because attention maps and Grad-CAM represent different quantities.

The generated figure is:

```text
05_attention_vs_gradcam.png
```

## Bonus — Data-Efficiency Sweep

A lightweight bonus experiment compares head-only ViT and ResNet models at different training-set fractions.

| Model | Training Fraction | Training Images | Validation Accuracy |
|---|---:|---:|---:|
| ResNet-50 head-only | 0.25 | 1,057 | 0.6645 |
| ViT-B/16 head-only | 0.25 | 1,057 | **0.8079** |
| ResNet-50 head-only | 0.50 | 2,114 | 0.8024 |
| ViT-B/16 head-only | 0.50 | 2,114 | **0.9084** |
| ResNet-50 head-only | 1.00 | 4,228 | 0.8510 |
| ViT-B/16 head-only | 1.00 | 4,228 | **0.9305** |

## Supplementary Experiment — CNN + Late Self-Attention

To address the required CNN/attention discussion empirically, the notebook compares:

- CNN control
- CNN + late self-attention

Both use the same frozen pretrained ResNet-50 convolutional backbone.

Results:

| Model | Validation Accuracy | Validation Macro-F1 | Trainable Parameters |
|---|---:|---:|---:|
| CNN control | **0.9536** | **0.9535** | 534,822 |
| CNN + late attention | 0.8996 | 0.8931 | 798,502 |

Observed Macro-F1 change:

```text
-0.0604
```

Mean learned attention distance on the final 7×7 feature map:

```text
3.726 cells
```

The late-attention block learned non-local spatial interactions, but it did not improve classification performance in this setup.

## Repository Structure

```text
vit-plant-disease-classification/
├── README.md
├── requirements.txt
├── REFERENCES.md
├── vit_plant_disease_classification.ipynb
├── figures/
└── results/
```

Trained model checkpoints are **not included in the repository because of their file size**.

The pretrained backbones can be downloaded automatically and the trained models can be reproduced by running the notebook end-to-end.

## Installation

The notebook metadata reports:

```text
Python 3.12.13
```

Install the dependencies:

```bash
pip install -r requirements.txt
```

## Running the Project

1. Install the dependencies.
2. Add PlantVillage as a Kaggle input.
3. Open `vit_plant_disease_classification.ipynb`.
4. Run the notebook from top to bottom.
5. The notebook searches recursively for an image-folder dataset containing the 38 PlantVillage classes.
6. Figures and result files are written to `figures/` and `results/`.

Internet access is required the first time to download pretrained ResNet-50 and ViT-B/16 weights if they are not already cached.

## Saved Outputs

The executed notebook generated these figures:

```text
01_class_distribution.png
02_example_images.png
03_vit_unfreezing_ablation.png
04_confusion_matrices.png
05_attention_vs_gradcam.png
06_bonus_data_efficiency.png
07_discussion_late_attention_ablation.png
```

Result files:

```text
bonus_data_efficiency.csv
class_names.json
discussion_cnn_late_attention.csv
environment_versions.json
final_model_summary.csv
hypothesis_decision.csv
paired_model_comparison.csv
per_class_disagreement.csv
representative_disagreements.csv
resnet_per_class_metrics.csv
vit_per_class_metrics.csv
vit_unfreezing_ablation.csv
```

## Reproducibility

The notebook uses:

```text
SEED = 42
```

and fixes seeds for:

- Python
- NumPy
- PyTorch
- CUDA, when available

It also enables deterministic CuDNN behavior when CUDA is available.

The executed notebook used:

```text
Device: CUDA
```

## Limitations

- PlantVillage contains relatively clean backgrounds compared with real field imagery.
- The experiment uses a capped per-class subset for computational practicality.
- The original PlantVillage class prevalence is therefore not preserved.
- The main training budget is only two epochs.
- Equal data, epochs, preprocessing, and optimizer family do not imply equal compute.
- FLOPs and trainable-parameter counts are not matched.
- The ViT-vs-ResNet test difference is small and not statistically significant.
- ViT attention and Grad-CAM are different explanation methods and are compared qualitatively.
- Results are specific to this dataset subset and training budget.

## References

See [`REFERENCES.md`](REFERENCES.md) for dataset, pretrained-model, and software sources.

## Course Context

**Advanced Data Mining — Deep Learning Project**

- Architecture family: Vision Transformer
- Project: Fine-Tuning a Vision Transformer with a CNN Baseline
- Project value: 30 points
