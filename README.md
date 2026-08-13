# Anomaly Detection in Network Traffic

Detecting cyberattacks in network traffic using both **supervised** (Logistic Regression, Random Forest) and **unsupervised** (Isolation Forest, Autoencoder) machine learning approaches on the [CICIDS2017](https://www.unb.ca/cic/datasets/ids-2017.html) dataset.

> MSc coursework — Machine Learning (M430), National and Kapodistrian University of Athens, 2026.

---

## Overview

This project compares supervised and unsupervised approaches for **binary network intrusion detection** (benign vs. attack) on the CICIDS2017 dataset.

The supervised models are trained using both benign and attack labels, while the unsupervised models are trained only on benign traffic and identify attacks as deviations from normal behaviour.

The comparison highlights an important difference between the two settings: supervised models achieve very high performance when labelled attack examples are available during training, while unsupervised anomaly detection is more relevant to previously unseen attacks but proves substantially more difficult.

---

## Key Results

| Model | Accuracy | Precision | Recall | F1-Score |
|---|---:|---:|---:|---:|
| Logistic Regression | 0.918 | 0.717 | 0.961 | 0.821 |
| **Random Forest** | **0.999** | **0.996** | **0.998** | **0.997** |
| Isolation Forest | 0.858 | 0.725 | 0.448 | 0.554 |
| Autoencoder | 0.881 | 0.912 | 0.438 | 0.591 |

The supervised models clearly outperform the unsupervised approaches on the random train/test split. Random Forest reaches an F1-score of 0.997, while Logistic Regression reaches 0.821.

The unsupervised setting is considerably more challenging. Isolation Forest and the Autoencoder detect fewer than half of the attacks, with recall of 0.448 and 0.438 respectively. The Autoencoder achieves substantially higher precision (0.912), but still misses a large proportion of attacks.

### Interpreting the Random Forest result

The Random Forest F1-score of 0.997 should be interpreted with caution.

CICIDS2017 is a flow-based dataset, where a single attack event can generate many highly similar network-flow records. This experiment uses a random train/test split and does not remove duplicate records before splitting, so closely related flows may appear across both sets.

Five-fold cross-validation produced F1 scores of approximately:

`[0.99705, 0.99706, 0.99733, 0.99720, 0.99706]`

with a standard deviation of only `0.0001`.

The very small variation between folds, together with the flow-based structure of the dataset, suggests that the random-split evaluation may provide an optimistic estimate of performance on genuinely unseen network traffic.

A stronger evaluation would remove duplicate flows before splitting or use a temporal or attack-day-based split so that related traffic is kept together.

The main result of the comparison is therefore not simply the near-perfect Random Forest score. The larger challenge appears in the unsupervised setting: without using attack labels during training, both anomaly-detection models miss more than half of the attacks.

---
##  Results Visualization

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

##  Repository Structure
```
anomaly-detection-cicids2017/
│
├── anomaly_detection_pipeline.ipynb # Full ML pipeline
├── assets/ # Figures and plots
├── README.md
├── .gitignore
```

---

##  Dataset

**CICIDS2017 — Canadian Institute for Cybersecurity Intrusion Detection Dataset**

- ~2.8 million network flow instances
- 79 features, multiple attack types
- Highly imbalanced (~83% benign traffic)

>  Dataset is not included due to size (~500MB)  
> Download from: https://www.unb.ca/cic/datasets/ids-2017.html  

---

##  Methodology

### Preprocessing
- Merged multiple CSV files
- Sampled subset for efficiency
- Removed NaNs, infinities, constant features
- Converted multi-class labels → binary classification
- Feature scaling (Standardization)
- Train / Validation / Test split

### Models

**Supervised:**
- Logistic Regression (baseline, class-weighted, tuned C)
- Random Forest (ensemble, cross-validation)

**Unsupervised (trained on normal traffic only):**
- Isolation Forest (threshold tuning via validation set)
- Autoencoder (PyTorch, reconstruction error-based detection)

---

##  How to Run

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

##  Tech Stack
`Python` `scikit-learn` `PyTorch` `pandas` `numpy` `matplotlib`

##  Authors

- Anna Allagioti  
- Maria-Konstantina Karkoglou  
- Maria Plerou  
