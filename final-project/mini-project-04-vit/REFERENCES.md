# References

## Dataset

### PlantVillage

- **Dataset:** PlantVillage
- **Task used here:** 38-class crop-disease image classification
- **Notebook source:** Kaggle PlantVillage mirror

Dataset used in the executed notebook:

```text
/kaggle/input/datasets/mohitsingh1804/plantvillage/PlantVillage/train
```

Original PlantVillage paper:

**Hughes, D. P., & Salathé, M.**  
*An open access repository of images on plant health to enable the development of mobile disease diagnostics.*  
2015.

https://arxiv.org/abs/1511.08060

## Pretrained Models

### Vision Transformer

The project uses a pretrained ViT-B/16 implementation from `timm`.

`timm` documentation / repository:

https://github.com/huggingface/pytorch-image-models

Vision Transformer paper:

**Dosovitskiy, A., et al.**  
*An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale.*  
ICLR 2021.

https://arxiv.org/abs/2010.11929

### ResNet-50

The CNN baseline uses pretrained ResNet-50 weights from `torchvision`.

torchvision ResNet documentation:

https://pytorch.org/vision/stable/models/resnet.html

ResNet paper:

**He, K., Zhang, X., Ren, S., & Sun, J.**  
*Deep Residual Learning for Image Recognition.*  
CVPR 2016.

https://arxiv.org/abs/1512.03385

## Interpretability Methods

### Grad-CAM

The CNN explanation experiment uses Grad-CAM-style class activation visualization.

**Selvaraju, R. R., et al.**  
*Grad-CAM: Visual Explanations from Deep Networks via Gradient-based Localization.*  
ICCV 2017.

https://arxiv.org/abs/1610.02391

### ViT Attention Visualization

The notebook visualizes final-block CLS-to-patch attention from the selected Vision Transformer.

This is treated as a qualitative attention probe and not as a causal explanation.

## Software and Libraries

### PyTorch

Used for model training, tensor operations, custom modules, optimization, and evaluation.

https://pytorch.org/

### torchvision

Used for:

- ResNet-50
- pretrained ImageNet weights
- image transforms
- dataset utilities

https://pytorch.org/vision/

### timm

Used to load and fine-tune the pretrained Vision Transformer.

https://github.com/huggingface/pytorch-image-models

### scikit-learn

Used for:

- stratified train/validation/test splitting
- accuracy
- Macro-F1
- classification reports
- confusion matrices

https://scikit-learn.org/

### SciPy

Used for the exact binomial test underlying the paired McNemar comparison.

https://scipy.org/

### NumPy

Used for numerical operations, sampling, seed control, and bootstrap analysis.

https://numpy.org/

### pandas

Used for result tables and CSV exports.

https://pandas.pydata.org/

### Matplotlib

Used for image examples, ablation plots, confusion matrices, attention/Grad-CAM visualizations, and data-efficiency plots.

https://matplotlib.org/

### Pillow

Used for image loading and processing.

https://python-pillow.org/

## Project Methodology

The main original contribution is a controlled **ViT layer-unfreezing study** comparing:

1. classifier head only
2. final two Transformer blocks + classifier head
3. full fine-tuning

The selected ViT strategy is then compared with a pretrained ResNet-50 baseline on the same held-out test set.

Additional analyses include:

- paired McNemar comparison
- paired bootstrap confidence interval
- per-class disagreement analysis
- representative model disagreements
- ViT attention vs ResNet Grad-CAM
- data-efficiency sweep
- CNN + late self-attention ablation

## Course Source

**Data Mining — Deep Learning Project Handbook**  
Instructor: Dr. Hadi Farahani  
Term: Spring 2026

The project follows the handbook's **ViT-1 Fine-Tuning a Vision Transformer with a CNN Baseline** specification:

- PlantVillage dataset
- pretrained ViT-B/16
- comparable CNN baseline
- same data split and controlled training protocol
- original layer-unfreezing investigation
- stated hypothesis
- quantitative evidence
- per-class disagreement analysis
- attention-map / Grad-CAM comparison
- CNN/attention discussion
- optional data-efficiency sweep
