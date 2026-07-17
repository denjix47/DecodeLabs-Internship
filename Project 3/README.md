# Project 3: Unsupervised Learning (Customer Segmentation)

**Author:** Sameer Majid (25i-2018)  
**Institution:** FAST-NUCES, Islamabad  
**Program:** DecodeLabs Industrial Training Kit (Batch 2026)

---

## 📌 Project Overview
This repository contains the technical implementation for **Project 3: Unsupervised Learning**. The core objective of this project is to discover hidden mathematical groupings within unlabeled retail data and translate those clusters into actionable business personas. 

Instead of relying on basic demographic assumptions, this project utilizes a strict distance-based algorithmic pipeline to extract mathematical truths from consumer behavior, proving the optimal number of market segments through rigorous diagnostic evaluation.

## 🏗️ The Input-Process-Output (IPO) Architecture
This pipeline follows a strict four-phase integration framework to ensure zero mathematical bias and high business interpretability:

### 1. Scale (Standardization)
Distance-based algorithms like K-Means rely on Euclidean distance, meaning larger magnitude features (e.g., $100,000 Income) will inherently overpower smaller scale features (e.g., 10 Purchases). 
*   **Implementation:** Continuous features are normalized using `StandardScaler` to map them to a common geometric range ($Mean = 0, \sigma = 1$), ensuring equal mathematical voting power.

### 2. Compress (Principal Component Analysis)
High-dimensional datasets (20+ features) suffer from the *Curse of Dimensionality*, where data points become equidistant and distance metrics break down.
*   **Implementation:** `PCA` (Principal Component Analysis) is applied to project the high-dimensional data onto a lower-dimensional surface. A strict **95% Cumulative Explained Variance** threshold is enforced to drop low-variance noise while preserving core behavioral signals.

### 3. Cluster (K-Means Algorithm)
With a compressed, mathematically sound feature space, the data is partitioned.
*   **Implementation:** The K-Means algorithm is deployed to minimize the Within-Cluster Sum of Squares (WCSS).

### 4. Translate (Business Personas)
Abstract PCA coordinates (e.g., `[-2.14, 0.88]`) cannot drive business strategy. 
*   **Implementation:** Centroid coordinates are reverse-engineered through the inverse transforms of both PCA and the Standard Scaler. This reconstructs human-centric metrics (Age, Income, Spending Score) to build a **Strategic Persona Matrix**.

## ⚖️ Diagnostic Gatekeepers
Because K-Means cannot natively determine the optimal number of clusters ($K$), the architecture is mathematically proven using two diagnostic gatekeepers:

1.  **The Elbow Method:** Evaluates WCSS to find the point of maximum curvature (the inflection point of diminishing returns), preventing the artificial splitting of natural customer groups.
2.  **The Silhouette Score:** Measures cluster cohesion (internal tightness) versus separation (distance to neighbors). Scores approaching `+1.0` confirm optimal market isolation.

## 🚀 Getting Started

### Prerequisites
Ensure you have the following Python libraries installed:
```bash
pip install pandas numpy scikit-learn matplotlib seaborn openpyxl
```

### Execution
1. Place the `Dataset for Data Analytics.xlsx` file in the root directory.
2. Run the pipeline script:
```bash
python customer_segmentation.py
```
3. The script will output the diagnostic charts (Elbow Curve & Silhouette Scores).
4. After reviewing the charts, the script will automatically reverse-engineer the centroids and output the **Strategic Persona Matrix**.

## 📈 Strategic Output Example
The final output maps raw data into segmented business strategies, for example:
*   **Cluster 0 (The Affluent Conservatives):** High-touch support, exclusive perks.
*   **Cluster 1 (The High-Value Trendsetters):** Early access, experiential marketing.
*   **Cluster 2 (The Budget-Conscious Explorers):** Influencer campaigns, flash sales.
*   **Cluster 3 (The Conservative Minimizers):** Clear price value, basic utility.
