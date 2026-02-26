# Wafer Level Anomaly Detection
This project evaluates **three anomaly detection methods** on two wafer datasets (**Wafer 1** and **Wafer 2**) under **realistic manufacturing constraints**.
The emphasis is **not headline accuracy**, but **model behavior, failure modes, and anomaly detection effectiveness** in highly imbalanced data.

## Repository Structure
├── DetectingAnomallies.ipynb # Main analysis notebook
├── DetectingAnomallies.html # Exported HTML version
└── README.md

## Dataset
The wafer fault detection dataset is sourced from:

**STMicroelectronics – ST-AWFD Dataset**  
https://github.com/STMicroelectronics/ST-AWFD/blob/main/README.md

The dataset reflects real-world semiconductor manufacturing conditions:
- Extreme class imbalance  
- Rare but critical anomalies  
- Subtle, process-level failures  

## Problem Reality Check
- Anomalies are rare  
- False negatives are very costly  
- High accuracy can occur even when anomalies are missed  
- Models are evaluated on **anomaly detection behavior**, not accuracy inflation  

**Primary metrics**
- Precision  
- Recall  
- F1-score  

Accuracy is reported only for reference.

## Models Evaluated

| Model | Type | Labels Required |
|-----|-----|----------------|
| XGBoost | Supervised | Yes |
| Isolation Forest | Unsupervised | No |
| Autoencoder | Unsupervised (Deep Learning) | No |

## Wafer 1 Results 

### 1. XGBoost (Supervised)
**Observed**
- Near-perfect anomaly precision and recall  

**Interpretation**
- Anomalies are well-separated in feature space  
- Decision boundaries are easy to learn with labels  

**Takeaway**
This reflects data separability, not inherent model superiority.  
Performance would degrade under anomaly drift.

### 2. Isolation Forest (Unsupervised)

**Observed**
- Very high accuracy  
- Extremely low anomaly recall  

**Interpretation**
- Model predicts the majority (normal) class  
- Accuracy is misleading  

**Takeaway**
Isolation Forest fails to detect rare defects and should not be trusted alone.

### 3. Autoencoder (Unsupervised, Deep Learning)

**Observed**
- Clear separation in reconstruction error  
- Effective threshold-based anomaly detection  

**Interpretation**
- Successfully learns normal wafer behavior  
- Anomalies consistently reconstruct poorly  

**Takeaway**
Captures multivariate, nonlinear structure missed by classical methods.

## Wafer 2 Results

Wafer 2 contains significant overlap between normal and anomalous samples, closely resembling real production conditions.

### 1. XGBoost (Supervised)

**Observed**
- Very high anomaly recall  
- Extremely low anomaly precision  
- Low overall accuracy  

**Interpretation**
- Aggressive anomaly flagging  
- Many false positives due to recall optimization  

**Takeaway**
In manufacturing, missing defects is often worse than over-inspection.

### 2. Isolation Forest (Unsupervised)

**Observed**
- Moderate accuracy  
- Near-zero anomaly recall  

**Interpretation**
- Struggles with overlapping distributions  
- Misses subtle process deviations  

**Takeaway**
Distance-based isolation is insufficient for complex wafer failures.

### 3. Autoencoder (Unsupervised, Deep Learning)

**Observed**
- Strong anomaly signal via reconstruction error  
- More balanced detection  

**Interpretation**
- Models normal process dynamics, not just point isolation  

**Takeaway**
Better suited for process drift and correlated sensor failures.

## Why Accuracy Is a Bad Metric?
**Example**
- 99% normal wafers  
- Model predicts everything as normal  
- Accuracy ≈ 99%  
- Anomaly recall = 0%  

A useless model with excellent accuracy.

## Cross-Method Comparison

| Method | Labels | Strength | Weakness |
|------|------|---------|---------|
| XGBoost | Required | Recall control | Label dependency |
| Isolation Forest | Not required | Fast baseline | Misses subtle faults |
| Autoencoder | Not required | Models complex behavior | Threshold tuning |

## Final Conclusions

- Wafer anomaly detection cannot rely on a single model  
- Isolation Forest alone is insufficient  
- Supervised models are powerful but fragile to drift  
- Autoencoders provide the best balance for unlabeled manufacturing data  
- Metric choice matters more than raw scores  

This project emphasizes **model behavior and failure modes**, not inflated performance numbers.
