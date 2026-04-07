# Customer Segmentation — Preprocessing & Modeling Pipeline

## Overview

This document defines the final preprocessing pipeline used to prepare data for PCA and clustering. The goal is to convert cleaned features into a structured, scaled, and reduced representation suitable for unsupervised learning.

---

## Pipeline Steps

### 1. Feature Separation

#### Summary

Split features into categorical and numeric groups to apply appropriate transformations.

#### Approach

* Categorical features → One-hot encoding
* Numeric / ordinal features → Scaling

#### Example

```python
categorical_features = [...]
numeric_features = [...]
```

---

### 2. Categorical Encoding

#### Summary

Convert categorical variables into numeric format using one-hot encoding.

#### Approach

* Use `pd.get_dummies()`
* Drop first category to avoid redundancy (optional)

#### Example

```python
df_encoded = pd.get_dummies(df, columns=categorical_features, drop_first=True)
```

#### Notes

* Avoid one-hot encoding ordinal features
* Keep dimensionality under control

---

### 3. Missing Value Imputation

#### Summary

Handle missing values before scaling and PCA.

#### Approach

* Use mean imputation for numeric features
* Apply after encoding

#### Example

```python
from sklearn.impute import SimpleImputer

imputer = SimpleImputer(strategy="mean")
df_imputed = pd.DataFrame(imputer.fit_transform(df_encoded), columns=df_encoded.columns)
```

#### Notes

* Do not drop rows → dataset is large and sparse
* Preserve maximum information

---

### 4. Feature Scaling

#### Summary

Normalize features to ensure equal contribution to PCA.

#### Approach

* Use StandardScaler (mean = 0, std = 1)

#### Example

```python
from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()
df_scaled = scaler.fit_transform(df_imputed)
```

#### Notes

* Required for PCA
* Prevents dominance of large-scale features

---

### 5. PCA (Dimensionality Reduction)

#### Summary

Reduce feature space while preserving variance.

#### Approach

* Fit PCA on scaled data
* Analyze explained variance
* Select optimal number of components

#### Example

```python
from sklearn.decomposition import PCA

pca = PCA()
pca.fit(df_scaled)

explained_variance = pca.explained_variance_ratio_
```

---

### 6. Selecting Number of Components

#### Summary

Choose number of components based on cumulative variance.

#### Approach

* Plot cumulative variance
* Select threshold (e.g., 80–90%)

#### Example

```python
import numpy as np

cumulative_variance = np.cumsum(explained_variance)
```

---

### 7. Transform Data

#### Summary

Project data into reduced PCA space.

#### Example

```python
pca = PCA(n_components=30)  # example
df_pca = pca.fit_transform(df_scaled)
```

---

## Key Design Decisions

* One-hot encoding used for categorical variables
* Ordinal features kept numeric (no encoding)
* Mean imputation used for simplicity and consistency
* StandardScaler used for normalization
* PCA used to reduce dimensionality and noise

---

## Common Pitfalls Avoided

* Encoding ordinal features as categorical
* Dropping too many rows due to missing values
* Skipping scaling before PCA
* Keeping redundant features
* Overloading with high-cardinality categorical features

---

## Common Questions

* **Why scaling before PCA?**
  PCA is variance-based; unscaled features distort importance

* **Why not drop missing rows?**
  Too much data loss; imputation preserves structure

* **Why PCA?**
  Reduce noise, improve clustering performance

* **Why not use all features?**
  High dimensionality reduces cluster quality

* **Why one-hot encoding?**
  Required to convert categorical variables into numeric space

---

