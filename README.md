# ISIC 2024 - Skin Cancer Detection with 3D-TBP

Solution for the Kaggle competition [ISIC 2024 - Skin Cancer Detection with 3D-TBP](https://www.kaggle.com/competitions/isic-2024-challenge).

## Result

| | Private | Public |
|---|---|---|
| **Final submission (rank-averaged ensemble)** | **0.169** | **0.186** |

🥈 **Silver Medal**: Rank  40/ 2739 teams

## Solution Overview

The final submission is a **rank-averaged ensemble of three models**.
All three are built on public notebooks with small modifications.

| # | Approach | Private | Public | Output in notebook |
|---|---|---|---|---|
| 1 | GBDT soft voting (LightGBM / CatBoost / XGBoost) + 2 image-model features | 0.167 | 0.184 | `submission_2.csv` |
| 2 | GBDT soft voting (LightGBM / CatBoost / XGBoost) + 4 image-model features | 0.167 | 0.184 | `submission_3.csv` |
| 3 | LightGBM + ImageNet-model predictions, stacked with logistic regression | 0.165 | 0.184 | `submission_1.csv` |

### Model 1 & 2
- Based on [ISIC 2024 Skin Cancer Prediction](https://www.kaggle.com/code/vyacheslavbolotin/isic-2024-skin-cancer-prediction)
- Tabular features: raw metadata, engineered lesion features, patient-normalized features
- Image features: predictions from EfficientNet-B0 and a pseudo-label model (Model 1), plus EVA02 and EdgeNeXt (Model 2)
- Over/under-sampling to handle the extreme class imbalance

### Model 3
- Based on [ISIC 2024 LGBM ImageNet v5a](https://www.kaggle.com/code/richolson/isic-2024-lgbm-imagenet-v5a)
- Added Gaussian noise to the OOF image-model predictions so that LightGBM would not rely on them too heavily
- Early stopping based on pAUC
- "Ugly duckling" features (how much a lesion differs from the patient's other lesions at the same location)

### Ensemble
- Each model's predictions are converted to ranks, summed, and mapped back to the [0, 1] range.
- Because the metric (pAUC above 80% TPR) depends only on the order of predictions, rank averaging lets models with different output scales be combined directly.

## Notes / Reflections

- Adding or removing individual features had little impact on the score.
- CV and Public LB were only weakly correlated. I suspected possible target leakage in all of the base notebooks, especially through the OOF image predictions.
- All models seemed to overfit and showed high variance (Model 3 in particular).
- I expected that ensembling several high-variance models would reduce variance, and the ensemble did beat every single model on both Private and Public LB. However, I did not verify why it worked.

## Acknowledgements

This solution relies heavily on the following public notebooks. Thanks to the authors.
- [vyacheslavbolotin / ISIC 2024 Skin Cancer Prediction](https://www.kaggle.com/code/vyacheslavbolotin/isic-2024-skin-cancer-prediction)
- [richolson / ISIC 2024 LGBM ImageNet v5a](https://www.kaggle.com/code/richolson/isic-2024-lgbm-imagenet-v5a)
- [snnclsr / LGBM baseline with new features](https://www.kaggle.com/code/snnclsr/lgbm-baseline-with-new-features)
