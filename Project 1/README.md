# Project 1: Advanced EDA & Feature Engineering

## Goal

Transform a raw e-commerce orders dataset into a clean, model-ready dataset by:
1. Handling missing values
2. Detecting and neutralizing outliers
3. Engineering new predictive features

## Dataset

`Dataset_for_Data_Analytics.xlsx` — 1,200 e-commerce order records with columns:
`OrderID, Date, CustomerID, Product, Quantity, UnitPrice, ShippingAddress,
PaymentMethod, OrderStatus, TrackingNumber, ItemsInCart, CouponCode,
ReferralSource, TotalPrice`.

## 1. Missing Value Handling

Only one column had missing values: **`CouponCode`** (309 / 1200 rows, ~25.75%).

**Decision: filled with `"NoCoupon"` instead of mean/median/KNN imputation.**

Mean, median, and KNN imputation are designed for *numeric* data, where a missing
value represents an unknown quantity that needs to be statistically estimated.
`CouponCode` is categorical, and a missing value here doesn't mean "unknown" — it
almost certainly means the customer simply didn't apply a coupon. Treating it as
its own category (`"NoCoupon"`) preserves that real signal instead of manufacturing
a fake one, and avoids dropping a quarter of the dataset.

## 2. Outlier Handling

Checked all four numeric columns (`Quantity`, `UnitPrice`, `ItemsInCart`,
`TotalPrice`) using the IQR method:

```
Lower bound = Q1 - 1.5 × IQR
Upper bound = Q3 + 1.5 × IQR
```

Only `TotalPrice` had outliers (8 rows out of 1200).

**Decision: capped (winsorized) the outliers with `.clip()` instead of deleting
the rows.**

Row deletion would shrink the dataset and risks losing otherwise valid information
in the *other* columns of those 8 rows (their `Quantity`, `PaymentMethod`, etc. are
still legitimate). Capping preserves row count and dataset structure while still
neutralizing the extreme values' influence on any downstream model.

See `boxplots_before_capping.png` for a visual of the spread before capping.

## 3. Feature Engineering

Four new features were created (3 minimum required):

| Feature | Formula | Why it's useful |
|---|---|---|
| `OrderMonth` | extracted from `Date` | Captures seasonal buying patterns |
| `AvgItemValue` | `TotalPrice / ItemsInCart` | Distinguishes "few expensive items" from "many cheap items" |
| `HasCoupon` | 1 if real coupon used, else 0 | Simple binary signal, more model-friendly than raw category |
| `CartFillRate` | `Quantity / ItemsInCart` | Measures how much of the cart was actually purchased vs. abandoned |

## 4. Encoding

Categorical columns (`PaymentMethod`, `OrderStatus`, `ReferralSource`, `Product`)
were one-hot encoded rather than label encoded. Label encoding would impose a
false numeric ordering between unrelated categories (e.g. implying "Online" is
mathematically "twice" "Cash"), which would mislead any downstream model.

## Files

- `Project_1.ipynb` — full notebook with code, comments, and outputs (open directly on GitHub to view)
- `cleaned_dataset.csv` — cleaned data with original readable columns
- `cleaned_dataset_encoded.csv` — fully numeric, one-hot encoded version ready for modeling
- `boxplots_before_capping.png` — outlier visualization

## How to Run

Open `Project_1.ipynb` in [Google Colab](https://colab.research.google.com) or
Jupyter, upload `Dataset_for_Data_Analytics.xlsx` when prompted, and run all
cells. Requires `pandas`, `numpy`, `matplotlib`, and `openpyxl`.
