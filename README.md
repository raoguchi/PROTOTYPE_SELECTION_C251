
# Prototype Selection for 1-NN Classification on MNIST

**Author:** Ryosuke Oguchi  
**Course:** CSE 251A — Project 1  
**Contact:** roguchi@ucsd.edu  

## 📚 Overview

This project investigates various methods of prototype selection for optimizing **1-Nearest Neighbor (1-NN)** classification using the **MNIST** dataset. The goal is to improve classification accuracy and time efficiency by intelligently reducing the dataset to a representative subset of prototypes.

---

## 📦 Dataset

- **MNIST** handwritten digits  
- 60,000 training images, 10,000 test images  
- Each image: 28×28 pixels grayscale

---

## 🧠 Prototype Selection Methods

### 1. Random Selection
Randomly samples \( M \) points from the training set.  
Baseline method — simple, but accuracy varies due to randomness.

### 2. K-Means Clustering
Clusters all data into \( M \) groups and uses centroids as prototypes.  
More structured than random selection but computationally expensive.

### 3. K-Means++ Clustering
Improves initial centroid choice using distance-based probabilistic seeding.  
Faster convergence and often better separation.

### 4. Random Sampling + K-Means
Reduces training set before K-Means using random sampling (\( j \gg M \)).  
Improves training time, risks representation loss.

---

## 🧪 Experiments & Results

### ✅ Evaluation Metric
- **Accuracy** of 1-NN predictions on the MNIST test set  
- **Confidence Intervals** (95% and 99%) for statistical comparisons

### 📊 Summary of Accuracy

| Method               | M (Prototypes) | Accuracy   |
|----------------------|----------------|------------|
| Random               | 10,000         | 94.69–95.04% (95%) |
|                      | 1,000          | 87.45–89.72% (95%) |
| K-Means              | 10,000         | 95.92%     |
|                      | 1,000          | 93.72%     |
| K-Means++            | 10,000         | 95.55%     |
|                      | 1,000          | 93.68%     |
| Random + K-Means     | 10,000 (from 45k) | 95.38–96.05% |
|                      | 1,000 (from 15k) | 91.25–93.02% |

> 📌 *K-Means and K-Means++ consistently outperform random selection, especially with fewer prototypes.*

---

## ⚙️ Implementation

- Prototypes used as references for 1-NN via **Euclidean distance**  
- Clustering with `sklearn.cluster.MiniBatchKMeans`  
- Random sampling via `numpy.random.choice`

### 🔧 Pseudocode

#### Euclidean Distance:
\[
d(x_q, x_i) = \sqrt{\sum_k (x_{q_k} - x_{i_k})^2}
\]

#### Prediction:
\[
\hat{y} = y_j \quad \text{where} \quad j = \arg\min_i d(x_q, x_i)
\]

---

## 🧩 File Structure

```
project/
├── code.ipynb              # Jupyter notebook with implementation
├── data/                   # Raw MNIST IDX files
├── CSE_251A_Project_1.pdf  # Final project report
```

---

## 🔍 Critical Insights

- K-Means++ performs slightly better in low-\( M \) scenarios but not drastically over K-Means.
- Random + K-Means is a promising compromise between accuracy and speed.
- High-dimensionality of MNIST (\( \mathbb{R}^{784} \)) may reduce benefits of clustering over full set.

---

## 🚀 Future Work

- Explore **Tangent Distance** to replace Euclidean, better capturing digit similarity
- Use data-driven sampling to pre-filter irrelevant training points
- Consider alternate clustering (e.g. GMM, hierarchical) or dimensionality reduction (e.g. PCA)

---

## 📚 References

- Arthur, D., & Vassilvitskii, S. (2007). *k-means++: The Advantages of Careful Seeding*  
- StackOverflow Discussions on K-Means Practicality and Initialization Methods

---

*This project was developed as part of UCSD CSE 251A coursework.*
