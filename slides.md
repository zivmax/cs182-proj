---
marp: true
theme: default
header: Introduction to Machine Learning 2024 Fall
footer: 龙冠澎、虞果、唐林峥
paginate: true
math: katex
style: |
  section::after {
    content: attr(data-marpit-pagination) '/' attr(data-marpit-pagination-total);
  }
---

# Automotive Clustering

<div style="margin-left: 30%; margin-top: -3%">

### —— Identifying Competitors for Volkswagen Car

</div>

![bg opacity:0.1](image/slides/volkswagen.jpg)

---

# Introduction

- **Objective**: Cluster 205 automotive products to identify competitors of Volkswagen.
- **Key Features**: `carbody` and `price` are prioritized.
- **Algorithms**: K-means, DBSCAN, and DIANA (hierarchical clustering).


---

# Data Preprocessing

- **Feature Selection**:
  - Extracted `brand` from `CarName`.
  - Removed irrelevant `car_ID`.

![bg right:45% ](image/slides/datainfo.png)

---

# Data Preprocessing

- **Data Cleaning**:
  - Corrected spelling errors in `brand`.
  - Retained 24 outliers after Z-score analysis.

![bg right:50% fit](image/slides/z-socre-dist.png)

---

# Data Preprocessing

- **Feature Merging**:
  - Calculate the correlation matrix.
  - Reduced multicollinearity by merging highly correlated features into 19.

![bg right:50% fit 95%](image/slides/cor-matrix.png)


---

# Hierarchical Clustering (DIANA)

- **Factor Analysis**:
  - KMO score: 0.8035 (suitable for factor analysis).
  - 7 factors retained, explaining >80\% variance.
- **Clustering**:
  - Cut dendrogram at scale 10, resulting in 4 clusters.
  - Volkswagen products found in "Mid Price Comfortable Cars" and "Mid Price Practical Cars".


<div style="margin-left:20%">

![h:180](image/slides/dendrograph.png)

</div>


---

# Prototype Clustering (K-means)

- **PCA**:
  - Scaled data, weighted `price` and `carbody` by 1.25.
  - Retained 7 dimensions (80% variance).
- **Clustering**:
  - Chose `k=5` based on Elbow Plot.
  - Volkswagen products in "Mid Price Comfortable Cars" and "Mid Price Practical Cars".

![bg right:40% vertical fit](image/slides/elbow.png)
![bg fit](image/slides/kmeans.png)

---

# Density Clustering (DBSCAN)

- **PCA**:
  - Similar preprocessing as K-means.
- **Clustering**:
  - Optimized `eps=4.05` and `min_samples=3`.
  - Resulted in 2 clusters, with Volkswagen in "Mid Price MPVs".

![bg right:40% fit 180%](image/slides/dbscan.png)

---

# Results and Comparison

| **Method**         | **Silhouette Score** | **Calinski-Harabasz Score** | **Davies-Bouldin Score** |
|---------------------|----------------------|-----------------------------|--------------------------|
| Hierarchical        | 0.2835               | 87.8302                     | 1.2783                   |
| K-Means             | 0.1642               | 36.0311                     | 1.8688                   |
| DBSCAN              | 0.3590               | 9.0123                      | 0.9466                   |


<div style="font-size: 0.8em;">

- **Silhouette Score**: How similar an object is to its own cluster compared to other clusters.

- **Calinski-Harabasz Score**: The ratio of between-cluster dispersion to within-cluster dispersion. 

- **Davies-Bouldin Score**: The average similarity ratio of each cluster with the cluster that most similar to it.

</div>

---

# Conclusions

- **Best Method**: Factor Analysis + Hierarchical Clustering.

- **Competitors**: Filtered by price range (0.8, 1.2) times Volkswagen's mean price.