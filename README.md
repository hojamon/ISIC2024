# ISIC2024
Summary of my solution

My solution is a rank-averaged ensemble of three notebooks.

notebook1 : using 2 image features with stacking ensemble
Private score 0.167 ,Public score 0.184
This is essentially the same as the public notebook, with a few minor changes.

notebook 2: using 4 image features with stacking ensemble
Private score 0.167, public score 0.184
This is the same as the public notebook.
https://www.kaggle.com/code/vyacheslavbolotin/isic-2024-skin-cancer-prediction

notebook 3: using LightGBM+image functions with stacking ensemble
Private score 0.165, Public score 0.184
Adding some noise and using early stopping
https://www.kaggle.com/code/richolson/isic-2024-lgbm-imagenet-v5a

The result of the rank-averaged ensemble
Private score 0.169, public score 0.186

My thoughts during the competition

・I thought that adding or removing a few features might not have enough impact on the score.
・The correlation between CV and publicLB was weak, and all notebooks had a high probability of target leakage.
・I thought all notebooks were overfitting and the variance was high (especially notebook 3).
・I thought that the rank-average ensemble might be effective as the pAUC is only affected by the relative size of the predicted values.
・I expected that the variance might be reduced by the ensemble of notebooks with high variance, but to be honest I don't know why it worked well.
