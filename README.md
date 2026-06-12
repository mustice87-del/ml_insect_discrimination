# ml_insect_discrimination
Machine learning for insect discrimination
# Data & Code Package

---

## Contents

```
peer_review/
├── README.md                  ← this file
├── data/
│   ├── acoustic_features.csv  ← full feature matrix (human-readable, recommended)
│   ├── X_features.npy         ← feature matrix, numpy format (240 × 29)
│   ├── y_labels.npy           ← treatment labels (240,)
│   └── feat_names.npy         ← feature names (29,)
└── code/
    ├── 01_feature_extraction.py  ← extract 29 features from FLAC recordings
    ├── 02_classification.py      ← train & evaluate all classifiers
    ├── 03_figures.py             ← reproduce Figures 1–3
    └── requirements.txt          ← Python dependencies
```

---

## Data description

### `acoustic_features.csv`
The primary data file. Each row is one 5-second analysis window.

| Column | Description |
|---|---|
| `treatment` | `insect` = *S. granarius* in wheat; `worm` = *T. molitor* in powder; `powder` = powder substrate control; `wheat` = wheat substrate control |
| `rms` | Root mean square amplitude |
| `peak` | Peak amplitude |
| `crest_factor` | Peak / RMS |
| `zcr` | Zero-crossing rate |
| `skewness` | Amplitude distribution skewness |
| `kurtosis` | Amplitude distribution kurtosis |
| `spec_centroid` | Spectral centroid (Hz) |
| `spec_variance` | Spectral variance |
| `spec_skewness` | Spectral skewness |
| `spec_rolloff` | 85th-percentile spectral rolloff (Hz) |
| `spec_flatness` | Spectral flatness |
| `band_0_1k` | Fraction of energy in 0–1 kHz |
| `band_1_5k` | Fraction of energy in 1–5 kHz |
| `band_5_15k` | Fraction of energy in 5–15 kHz |
| `band_15_30k` | Fraction of energy in 15–30 kHz |
| `band_30_50k` | Fraction of energy in 30–50 kHz |
| `mfcc_1`–`mfcc_13` | Mel-frequency cepstral coefficients 1–13 |

**Sampling:** 60 windows per treatment class × 4 classes = 240 rows total.  
Windows were extracted at one-per-minute intervals from one-hour FLAC recordings  
(192 kHz, 24-bit, mono, piezoelectric contact transducer).

---

## Reproducing the analysis

### Requirements
Python ≥ 3.9. Install dependencies:
```bash
pip install -r code/requirements.txt
```

### Step 1 — Feature extraction (requires raw FLAC files)
```bash
python code/01_feature_extraction.py \
    --insect path/to/insect.flac \
    --worm   path/to/worm.flac   \
    --powder path/to/powder.flac \
    --wheat  path/to/wheat.flac  \
    --out    data/
```
Raw FLAC recordings are not included in this package due to file size  
(~330 MB each; total ~1.3 GB). They will be deposited in a public archive  
upon acceptance. The pre-extracted feature matrix (`acoustic_features.csv`)  
allows all downstream analyses to be reproduced without the raw audio.

### Step 2 — Classification & cross-validation (no raw audio needed)
```bash
python code/02_classification.py --data data/acoustic_features.csv
```
Prints Table 1 (feature statistics) and Table 2 (classifier performance)  
to stdout, and saves confusion matrices as PNG.

### Step 3 — Figures 1–3 (requires raw FLAC files for Figs 1 & 3)
```bash
python code/03_figures.py \
    --insect path/to/insect.flac \
    --worm   path/to/worm.flac   \
    --powder path/to/powder.flac \
    --wheat  path/to/wheat.flac  \
    --data   data/acoustic_features.csv \
    --out    figures/
```

---

## Anonymisation note
All author-identifying information has been removed from this package.  
File paths, comments, and metadata contain no institutional or personal details.
