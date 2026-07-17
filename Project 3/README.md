# Project 3: Unsupervised Learning (Customer Segmentation)

## Goal

Transform unlabeled retail data into actionable business personas by:
1. Standardizing features and applying Principal Component Analysis (PCA) to compress dimensionality.
2. Using the "Elbow Method" and "Silhouette Score" to mathematically prove the optimal number of K-Means clusters.
3. Translating the resulting abstract mathematical clusters into human-centric business strategies.

## Dataset

`Dataset for Data Analytics.xlsx` — 1,200 e-commerce order records with columns:
`OrderID, Date, CustomerID, Product, Quantity, UnitPrice, ShippingAddress, PaymentMethod, OrderStatus, TrackingNumber, ItemsInCart, CouponCode, ReferralSource, TotalPrice`.

## 1. Scale & Compress (Standardization & PCA)

All continuous numeric features were first scaled using `StandardScaler`. 

**Decision: Applied Standardization before distance-based clustering.**
Euclidean distance treats all spatial directions equally, meaning a feature with a massive scale (like `TotalPrice`) will completely swallow smaller scale features (like `ItemsInCart`), distorting spatial proximity. StandardScaler maps all features to a common geometric range (Mean = 0), giving them equal mathematical voting power and preventing scale-induced bias.

**Decision: Applied PCA with a 95% cumulative explained variance threshold.**
Enterprise datasets suffer from the "Curse of Dimensionality" where volume grows exponentially relative to distance, causing standard metrics to fail entirely. PCA compresses the feature space by identifying orthogonal axes of maximum variance. Setting the threshold to 95% discards low-variance noise while keeping the core behavioral signals intact.

## 2. Clustering & Diagnostics (K-Means)

Applied the K-Means algorithm to minimize the Within-Cluster Sum of Squares (WCSS). Because K-Means forces data into whatever 'K' value it is given, two diagnostic gatekeepers were used to mathematically prove the architecture:

*   **The Elbow Method:** Plotted WCSS against K to find the point of maximum curvature (the inflection point of diminishing returns), ensuring we stop artificially splitting natural customer groups.
*   **The Silhouette Score:** Measured cluster cohesion (internal tightness) versus separation. A score near +1.0 mathematically confirms excellent separation and ensures customers belong entirely to their assigned group rather than overlapping.

## 3. Business Translation

The K-Means clustering was executed within a reduced, abstract PCA-space.

**Decision: Reverse-engineered centroids using inverse transforms.**
Synthetic PCA coordinates (e.g., `[-2.14, 0.88]`) mean nothing to a marketing team. The final centroid coordinates were mapped back through the inverse transforms of both PCA and the `StandardScaler` to reconstruct interpretable, human-centric physical dimensions. This allowed for the creation of a Strategic Persona Matrix mapping raw data to targeted actions (e.g., separating "High-Value Trendsetters" from "Budget-Conscious Explorers").

## Files

- `Project_3.ipynb` — full notebook with code, comments, and outputs (open directly on GitHub to view)
- `Segmented_Customers.csv` — original dataset appended with an `Assigned_Persona` cluster column
- `diagnostic_charts.png` — visualizations of the Elbow Method and Silhouette Scores
- `README.md` — project overview and architecture methodology

## How to Run

Open `Project_3.ipynb` in [Google Colab](https://colab.research.google.com) or Jupyter, upload `Dataset for Data Analytics.xlsx` when prompted, and run all cells. Requires `pandas`, `numpy`, `matplotlib`, `seaborn`, `scikit-learn`, and `openpyxl`.
