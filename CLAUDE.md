# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**SurfSat** classifies satellite images of beaches to identify potential surf spots. It uses transfer learning (ResNet-50) to detect surf-relevant geomorphological features. Live demo: [surfsat.streamlit.app](https://surfsat.streamlit.app)

## Development Environment

This project is **Jupyter notebook-based**, designed to run in **Google Colab** with GPU access. There is no local build/test/lint toolchain.

**Dependencies** (install in Colab or a local venv):
```
torch torchvision pandas numpy scikit-learn matplotlib seaborn requests geopy lxml
```

**To run locally:**
```bash
jupyter notebook code/SurfSat_Data_PreProcessing.ipynb
jupyter notebook code/SurfSat_Models_Creation.ipynb
```

Execute cells sequentially. GPU (CUDA) is strongly recommended for training.

## Data

- `data/Satalite_Surf_Final_dataset.csv` — 392 manually reviewed beach samples (13 columns)
- `data/Satalite Surfing Images.kml` — 450+ global beach coordinates
- Satellite images and trained `.pth` model weights are hosted on Google Drive (see `data/Google_Drive_Link.txt`)

## Architecture

### Pipeline (two notebooks)

**1. `SurfSat_Data_PreProcessing.ipynb`** — Data collection & feature engineering:
- Parse KML → extract lat/lon for each beach
- Call Google Maps Static API (1280×1280px) to fetch satellite images
- Manual feature annotation: `Bay`, `Rivermouth`, `Wave_Pattern` (binary)
- Geocoding via Nominatim → `Country`, `Terrain` (Tropical/Desert/Unknown)
- Stratified train/val/test splits (stratified by tagger + terrain)
- Output: cleaned CSV + organized image folders on Google Drive

**2. `SurfSat_Models_Creation.ipynb`** — Model training & evaluation:
- Loads images from Google Drive folders
- Trains **four separate binary classifiers** on ResNet-50:
  - Bay model (82 pos + 82 neg samples)
  - Rivermouth model (63+63)
  - Wave Pattern model (89+89)
  - "All Together" combined model (good vs. not-good surf)
- Saves model weights as `.pth` files to Google Drive

### Model Architecture
- **Base:** ResNet-50 pretrained on ImageNet
- **Head:** Replace final FC layer: 2048 → 2 (binary classification, no sigmoid)
- **Optimizer:** SGD (lr=0.0001, momentum=0.9)
- **Loss:** CrossEntropyLoss
- **Augmentation:** Random horizontal flip, random resized crop → 224×224
- **Normalization:** ImageNet stats `[0.485, 0.456, 0.406]` / `[0.229, 0.224, 0.225]`
- **Training:** 70–100 epochs; best ~70% precision & recall; slight overfitting after ~65 epochs

### Evaluation
- Confusion matrices (seaborn heatmap)
- `sklearn` classification reports
- Train/val accuracy curves with 5-epoch moving average smoothing
