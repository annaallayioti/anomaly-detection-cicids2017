# Anomaly Detection in Network Traffic

Detecting cyberattacks in network traffic using both **supervised** (Logistic Regression, Random Forest) and **unsupervised** (Isolation Forest, Autoencoder) machine learning approaches on the [CICIDS2017](https://www.unb.ca/cic/datasets/ids-2017.html) dataset.

> MSc coursework — Machine Learning (M430), National and Kapodistrian University of Athens, 2026.

---

## Overview

This project compares supervised and unsupervised machine learning approaches for **binary anomaly detection** in network traffic (benign vs. attack), with a focus on class imbalance and the differences between models trained with labelled attack data and models trained only on benign traffic.

---

## Key Results

| Model | Accuracy | Precision | Recall | F1-Score |
|---|---:|---:|---:|---:|
| Logistic Regression | 0.918 | 0.717 | 0.961 | 0.821 |
| **Random Forest** | **0.999** | **0.996** | **0.998** | **0.997** |
| Isolation Forest | 0.858 | 0.725 | 0.448 | 0.554 |
| Autoencoder | 0.881 | 0.912 | 0.438 | 0.591 |

- **Random Forest** achieved the best overall performance with an F1-score of 0.997.
- **Autoencoder** achieved higher precision than Isolation Forest (0.912 vs. 0.725).
- Unsupervised models trained only on benign traffic detected fewer than half of the attacks (recall ≈ 0.44).

---

## Results Visualization

### Logistic Regression

![ROC Curve](assets/lr_roc.png)
![Confusion Matrix](assets/lr_confusion.png)
![Metrics](assets/lr_metrics.png)

---

### Random Forest

![Feature Importance](assets/rf_features.png)
![Confusion Matrix](assets/rf_confusion.png)
![Metrics](assets/rf_metrics.png)
![Cross Validation](assets/rf_cv.png)

---

### Isolation Forest

![Distribution](assets/iforest_distribution.png)
![Confusion Matrix](assets/iforest_confusion.png)
![Metrics](assets/iforest_metrics.png)

---

### Autoencoder

![Reconstruction Error Distribution](assets/autoencoder_distribution.png)
![Threshold Tuning](assets/autoencoder_threshold.png)
![Training History](assets/autoencoder_training.png)

---

### Model Comparison

![Performance Comparison](assets/model_comparison.png)
![Training Time](assets/training_time.png)

---

## Repository Structure

```text
anomaly-detection-cicids2017/
│
├── anomaly_detection_pipeline.ipynb  # Full ML pipeline
├── assets/                           # Figures and plots
├── README.md
├── .gitignore
└── LICENSE
```

---

## Dataset

**CICIDS2017 — Canadian Institute for Cybersecurity Intrusion Detection Dataset**

- ~2.8 million network flow instances
- 79 features and multiple attack types
- Highly imbalanced (~83% benign traffic)

> Dataset is not included due to size (~500MB).  
> Download from: https://www.unb.ca/cic/datasets/ids-2017.html

---

## Methodology

### Preprocessing

- Merged multiple CSV files
- Sampled a subset for computational efficiency
- Removed NaNs, infinities, and constant features
- Converted multi-class attack labels to binary labels
- Standardized numerical features
- Created train, validation, and test splits

### Models

**Supervised:**
- Logistic Regression — class-weighted baseline with tuned regularization
- Random Forest — ensemble model evaluated with cross-validation

**Unsupervised (trained on benign traffic only):**
- Isolation Forest — anomaly detection with threshold tuning
- Autoencoder — PyTorch model using reconstruction error for anomaly detection

---

## How to Run

### 1. Clone the repository

```bash
git clone https://github.com/annaallayioti/anomaly-detection-cicids2017.git
cd anomaly-detection-cicids2017
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Download the dataset

Download the CICIDS2017 dataset from:  
https://www.unb.ca/cic/datasets/ids-2017.html

Place the CSV files inside a `data/` folder in the project root.

### 4. Run the notebook

```bash
jupyter notebook anomaly_detection_pipeline.ipynb
```

---

## Tech Stack

`Python` `scikit-learn` `PyTorch` `pandas` `NumPy` `Matplotlib`

---

## Authors

- Anna Allagioti
- Maria-Konstantina Karkoglou
- Maria Plerou

---

## License

This project is licensed under the MIT License.
