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
- **Method**: 
  - Hierarchical Clustering (DIANA)
  - Prototype Clustering (K-means)
  - Density Clustering (DBSCAN).

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


<style scoped>
h1 {
    margin-bottom: 0 !important; /* Remove bottom margin of h1 */
}

h2 {
    margin-top: 0 !important; /* Remove top margin of h2 */
}
</style>

# Hierarchical Clustering
## DIANA

- **Factor Analysis**:
  - KMO score: 0.8035.
  - 7 factors retained (80\% variance).
- **Clustering**:
  - Cut dendrogram at scale 10, thus 4 clusters.
  - Volkswagen products found in "Mid Price Comfortable" and "Mid Price Practical".



![bg right:35% fit 95%](image/slides/dendrograph.png)




---

<style scoped>
h1 {
    margin-bottom: 0 !important; /* Remove bottom margin of h1 */
}

h2 {
    margin-top: 0 !important; /* Remove top margin of h2 */
}
</style>


# Prototype Clustering
## K-means

- **PCA**:
  - Normlized data, weighted `price` and `carbody` by 1.25.
  - Retained 7 dimensions (80% variance).
- **Clustering**:
  - Chose `k=5` based on Elbow Plot.
  - Volkswagen products in "Mid Price Comfortable" and "Mid Price Practical".

![bg right:40% vertical fit](image/slides/elbow.png)
![bg fit](image/slides/kmeans.png)

---

<style scoped>
h1 {
    margin-bottom: 0 !important; /* Remove bottom margin of h1 */
}

h2 {
    margin-top: 0 !important; /* Remove top margin of h2 */
}
</style>


# Density Clustering
## DBSCAN

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

- **Best Method**: Factor Analysis + DIANA.

- **Competitors**: Filtered by price range (0.8, 1.2) times Volkswagen's mean price.
