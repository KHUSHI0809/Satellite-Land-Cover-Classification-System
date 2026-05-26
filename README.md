
## 📌 Overview

An end-to-end machine learning pipeline that classifies Paris regions as **Developed** (urban infrastructure, streets, buildings) vs **Undeveloped** (vegetation, parks, water bodies) using **8-band multispectral satellite imagery** from the SpaceNet 7 Challenge. This project addresses real-world needs in urban planning, rapid mapping, and global land monitoring by automating land-cover classification at scale , without requiring expensive deep-learning infrastructure.

---

## 🎯 Problem Statement

Government agencies, urban planners, and NGOs need fast, accurate, and up-to-date land cover maps for:
- **Urban Planning** — tracking city expansion and infrastructure development
- **Rapid Mapping** — resource allocation and policy-making support
- **Global Monitoring** — scalable land usage change detection across large regions

**Objective:** Binary classification of 162×162 pixel satellite image tiles into `Developed` or `Undeveloped` classes using classical ML on extracted spectral and texture features.

---

## 📊 Results

| Metric | Score |
|--------|-------|
| **Accuracy** | 0.89 |
| **Precision** | 0.89 |
| **Recall** | 0.90 |
| **F1 Score** | 0.89 |
| **ROC-AUC** | **0.977** |

> Evaluated on a 20% test split (144 tiles held out from 720 total samples)

**Class Distribution:**
- Developed: 381 tiles
- Undeveloped: 339 tiles

---

## 🏗️ Pipeline Architecture

```
Load Data → Preprocessing → Tiling → Feature Extraction → PCA → Model Training → Evaluation → Output Maps
```

| Step | Description |
|------|-------------|
| **1. Load Data** | Load 8-band multispectral TIFF images from SpaceNet 7 (20 images, Paris AOI) |
| **2. Preprocessing** | Normalize pixel intensity values; impute/remove corrupted spectral bands; extract RGB composites |
| **3. Tiling** | Slice each 162×162 image into 36 tiles (6×6 grid) → **720 total samples** |
| **4. Feature Extraction** | Calculate **81 numerical features** per tile: band statistics (mean, std), texture metrics, histogram-based indicators |
| **5. PCA** | Reduce 81 dimensions → **20 principal components** (100% variance retained) |
| **6. Model Training** | Train Random Forest Classifier (n_estimators=100, random_state=42) |
| **7. Evaluation** | Calculate F1, ROC-AUC, confusion matrix, feature importances |
| **8. Output** | Generate final land cover classification maps |

---

## 🗂️ Dataset

| Property | Details |
|----------|---------|
| **Source** | SpaceNet 7 Challenge (DigitalGlobe) |
| **Location** | Paris, France (AOI) |
| **Data Type** | 8-band Multispectral TIFF |
| **Resolution** | 162 × 162 pixels per chip |
| **Images Used** | 20 sample images |
| **Total Tiles** | 720 (20 images × 36 tiles each) |
| **Labeling Method** | Intensity & Texture Thresholds |

---

## 🔬 Feature Engineering

**81 statistical and spectral features extracted per tile:**
- Band statistics: mean, standard deviation per spectral band
- Texture metrics: entropy, contrast, homogeneity
- Histogram-based indicators
- Band ratios and spatial statistics

**PCA Dimensionality Reduction:**
- Input: 81 features
- Output: 20 principal components
- Variance retained: ~100%
- Top 3 PCs account for >40% of model decision-making power

**Key Feature Insights:**
- **PC1 (Overall Intensity)** — captures scene brightness/albedo, separating built structures (bright) from vegetation (dark)
- **PC2 (Spectral Contrast)** — correlates with Red vs NIR contrast
- **PC3 (Texture Entropy)** — critical for urban density distinction
- Lower-ranked components (PC15-20) contribute <2% combined, validating the PCA reduction strategy

---

## 🌲 Model: Random Forest Classifier

Random Forest was chosen over deep learning approaches for this dataset because:

- ✅ **Tabular Feature Expert** — handles 81 extracted statistical features efficiently without complex architectures
- ✅ **Small Dataset Friendly** — works well with 720 samples where CNNs would overfit
- ✅ **Robust & Interpretable** — resistant to noise; provides built-in feature importance for transparency
- ✅ **Computationally Efficient** — fast training and inference for rapid iteration

---

## 🛠️ Tech Stack

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white)
![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=flat&logo=scikit-learn&logoColor=white)
![NumPy](https://img.shields.io/badge/NumPy-013243?style=flat&logo=numpy&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557c?style=flat)

| Library | Purpose |
|---------|---------|
| `rasterio` / `tifffile` | Load and parse 8-band multispectral TIFF files |
| `numpy` | Feature computation and array operations |
| `scikit-learn` | PCA, Random Forest, cross-validation, metrics |
| `matplotlib` / `seaborn` | Visualizations, confusion matrix, ROC curve |


---

## 🚀 Getting Started

### Prerequisites
```bash
pip install -r requirements.txt
```

### Requirements
```
numpy
rasterio
scikit-learn
matplotlib
seaborn
tifffile
jupyter
```

### Run the Pipeline
```bash
# Clone the repo
git clone https://github.com/KHUSHI0809/satellite-land-cover-classification.git
cd satellite-land-cover-classification

# Launch notebook
jupyter notebook notebooks/satellite_classification.ipynb
```


## 🏷️ Topics

`satellite-imagery` `land-cover-classification` `geospatial-ml` `spacenet` `random-forest` `pca` `multispectral` `remote-sensing` `scikit-learn` `python` `feature-engineering` `explainable-ai`
