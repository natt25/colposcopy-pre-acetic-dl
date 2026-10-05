# colposcopy-pre-acetic-dl

Deep learning for cervical lesion detection using **only colposcopy images taken before acetic acid application**.

Code accompanying the paper *"Detección de lesiones cervicales precancerosas mediante aprendizaje profundo en imágenes de colposcopia sin ácido acético"*. Universidad Católica de Santa María, Arequipa, Perú.

> Research code only. Not a medical device and not for clinical use.

## Research question

Colposcopy relies on acetic acid to reveal the acetowhite sign. Most published models use images taken **after** the acid. This project measures how much diagnostic signal remains in images taken **before** it, using CNNs with transfer learning and patient-level validation.

## Dataset (not included)

The images are not in this repository. Download them from the original source and follow its terms of use:

- Mendeley Data: https://doi.org/10.17632/j5mhfhxrwj.1
- Article: A. O. Ogunlaja et al., "A multi-magnification digital colposcopy image datasets for cervical cancer screening in Nigeria," *Data in Brief*, vol. 65, 112633, 2026. https://doi.org/10.1016/j.dib.2026.112633

Place the data in `data/raw/` keeping the original folders (`01_Normal`, `02_Abnormal`). Note: the public files are 640 × 480 px, not 2048 × 1536 as the article states.

## Method summary

- **Subset:** pre-acid images at 7× magnification (files `p4*.jpg`).
- **Preprocessing:** the date, time and magnification burned into the upper-left corner are reconstructed with inpainting (OpenCV, Telea).
- **Validation:** 5-fold `StratifiedGroupKFold`, grouped by patient. Folds are disjoint by patient.
- **Models:** ResNet50, EfficientNetB0, MobileNetV2, DenseNet121 and InceptionV3, pretrained on ImageNet and fine-tuned.
- **Augmentation:** flips, rotation, brightness and contrast. Hue and saturation are not changed.
- **Metrics:** AUC-ROC, AUC-PR, sensitivity at fixed specificity (0.95, 0.90, 0.80, 0.70) and a 95 % bootstrap confidence interval that resamples whole patients (2,000 repetitions).
- **Interpretability:** class activation maps (CAM).
- **Extra analyses:** model ensembles, stacking with Papanicolaou results, and RadImageNet weights (PyTorch).

## Main result

Pre-acid 7× images, patient-level 5-fold cross-validation: AUC-ROC 0.626 (95 % CI 0.565–0.689). See the paper for details and limitations.

## Repository structure

```
colposcopy-pre-acetic-dl/
├── data/
│   └── raw/               # dataset (not tracked)
├── notebooks/             # Colab notebooks
├── src/                   # preprocessing, training, evaluation
├── results/               # metrics and figures
├── requirements.txt
└── README.md
```

## Setup

The experiments ran on Google Colab with an NVIDIA T4 GPU.

```bash
git clone https://github.com/YOUR-USERNAME/colposcopy-pre-acetic-dl.git
cd colposcopy-pre-acetic-dl
pip install -r requirements.txt
```

Main libraries: TensorFlow/Keras, scikit-learn, NumPy, pandas, OpenCV, Matplotlib. PyTorch and torchvision are used only for the RadImageNet experiment.

## Reproducibility

- Fixed random seeds for the fold split.
- Patient-level split to prevent data leakage between training and validation.
- Predictions from all folds are pooled before computing metrics and confidence intervals.

## Citation

```bibtex
@article{YOUR_KEY,
  title   = {Detecci{\'o}n de lesiones cervicales precancerosas mediante aprendizaje profundo en im{\'a}genes de colposcopia sin {\'a}cido ac{\'e}tico},
  author  = {Garz{\'o}n Mendoza, Ana Paula Viviana and Laureano Tupac Yupanqui, Natty Jimena},
  year    = {2026}
}
```

## License

Code: MIT License. The dataset has its own license and terms on Mendeley Data.

## Contact

Ana Paula Viviana Garzón Mendoza and Natty Jimena Laureano Tupac Yupanqui, Universidad Católica de Santa María.
