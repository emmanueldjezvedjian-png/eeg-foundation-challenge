# EEG Foundation Challenge 1: predicting reaction time from EEG

IODAA course project. Goal: predict the reaction time (in seconds) of a subject in the
Contrast Change Detection task, from 2 s windows of EEG (129 channels, 100 Hz).
Data: HBN-EEG, mini releases R1 to R5 (99 subjects, 6,613 trials).

## Files

- `Projeteeg_propre.ipynb`: data preparation. Reads the BDF files, computes the reaction
  times, cuts the 2 s windows, and saves the arrays in `data/`.
- `Projeteeg_presentation_en.ipynb`: the analysis (references, Ridge, XGBoost, time slices,
  subject centring, PCA, SVR on windowed band power, checks).
- `data/y_5rel.npy`, `data/groupes_5rel.npy`, `data/releases_5rel.npy`: small saved arrays
  (reaction times, subject of each trial, release of each trial).

## How to run

1. Download the mini releases R1 to R5 (100 Hz, BDF) from the EEG Foundation Challenge
   website and put them in `data/R*_mini_L100_bdf/`.
2. Run `Projeteeg_propre.ipynb`. It writes `data/X_fen_5rel.npy` (651 MB, not versioned
   because it is too large for GitHub) and the three small arrays above.
3. Run `Projeteeg_presentation_en.ipynb`.

Python packages: numpy, pandas, scipy, scikit-learn, xgboost, matplotlib, mne.

## Main result

No classical model beats the mean baseline (RMSE 0.3431 s). Age alone gives 0.3355 s.
A weak signal appears when band power is computed in 4 chunks of 500 ms (about 1 to 2 %
better than the mean); the gain comes from temporal splitting, not from the choice of channels.