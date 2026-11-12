# EEG Foundation Challenge: predicting reaction time from 2 s of EEG

Class project (CentraleSupélec / AgroParisTech, defended 3 Nov 2026).
Task: predict a subject's reaction time (seconds) from a 2 s EEG window,
Challenge 1 of the EEG Foundation Challenge (Contrast Change Detection task).

## Data
- HBN-EEG, mini releases R1-R5 (20 subjects each), 100 Hz, 128 channels (+ 1 flat reference channel, dropped)
- 6,613 trials from 99 subjects after filtering (RT < 0.5 s removed: 137 trials; one subject with a flat recording removed: 63 windows)
- Each trial = one window of 128 x 200 samples, starting 0.5 s after the stimulus

## Validation
- **Subject-wise GroupKFold (5 folds)**: no subject appears in both train and validation, so the model cannot recognise the person. A trial-wise split would leak subject identity (the subject explains ~20% of RT variance).
- Metric: RMSE in seconds. Noise between folds is about +/-0.015.

## Features
- Welch band power (delta, theta, alpha, beta, gamma), log10: 128 x 5 = 640 features
- Mean signal per 200 ms window: 1,280 features
- Per-subject centring

## Results (RMSE, lower is better)
| Model | RMSE |
|---|---|
| Predict the mean (baseline) | 0.3431 |
| Ridge (all features, small alpha) | up to 0.3755 (overfits) |
| Ridge + per-band PCA (k = 10, alpha = 1e4) | 0.3411 (within noise) |
| XGBoost | ~0.345 |
| SVR, band power in 4 x 500 ms windows | ~1-2% better than the mean |
| EEGNeX (single fold) | 0.34-0.38 |

**Takeaway:** no model clearly beats the mean baseline. Inter-subject variability dominates the reaction-time signal in 2 s of EEG, and with 1,920 features for 99 subjects, models memorise subjects. PCA per band fixes the overfitting but does not create signal. The only consistent gain comes from splitting band power in time.

## Next steps
PLS (supervised PCA), more subjects (R6-R11), event-locked features (ERPs).

## Files
- `Projeteeg_propre.ipynb`: dataset construction
- `Projeteeg_presentation_en.ipynb`: analysis and results
- Starter kit files come from the official challenge starter kit: not my work.
