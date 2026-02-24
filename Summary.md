# IT Ticket Clustering – Process and Results Summary

## 1. Introduction
For this assignment, I worked on an open-source IT service ticket dataset from Kaggle. The objective was to preprocess the ticket data, remove configuration item (CI) related information, perform exploratory data analysis (EDA), and apply clustering techniques to group similar tickets.

---

## 2. Dataset Description
The dataset consists of IT service ticket descriptions along with a corresponding category (`Topic_group`). The `Document` column contains unstructured text describing the issue. The `Topic_group` column was used only for reference and not for training the clustering models.

---

## 3. Configuration Item (CI) Removal
A key requirement of the assignment was to remove configuration item (CI) or asset-related information from the ticket descriptions.

In real-world IT data, tickets often contain:
- Hostnames
- IP addresses
- Ticket IDs (INC, CHG, RITM, etc.)
- Asset tags and device identifiers

If these are not removed, the clustering model tends to group tickets based on device identifiers instead of the actual issue.

To address this, I applied regex-based cleaning to remove:
- IP addresses
- Hostnames and domain names
- Ticket IDs
- Alphanumeric asset identifiers
- CI-related keywords such as “configuration item” and “cmdb”

This ensured that the clustering focuses only on the issue description.

---

## 4. Data Preprocessing
The following preprocessing steps were applied:
- Converted all text to lowercase
- Removed special characters and extra spaces
- Created additional features such as:
  - Word length (`word_len`)
  - Character length (`char_len`)
- Filtered out:
  - Very short tickets (low information)
  - Very long tickets (noisy or multi-issue)

These steps helped improve data quality and consistency.

---

## 5. Exploratory Data Analysis (EDA)
EDA was performed to understand the structure and quality of the dataset.

### Key observations:
- The ticket lengths were reasonably distributed, indicating sufficient textual information.
- There were no major missing value issues in the text column.
- The label distribution (Topic_group) showed imbalance, which can make clustering more challenging.
- Frequent word analysis confirmed that the dataset contains relevant IT issue-related terms.

---

## 6. Feature Engineering
To convert text into numerical form, I used **TF-IDF vectorization**:
- Captures importance of words in each ticket
- Used uni-grams and bi-grams for better context
- Removed very rare and very frequent words using thresholds

Since TF-IDF produces high-dimensional data, I applied **Truncated SVD** to reduce dimensionality to 100 components. This helps:
- Reduce noise
- Improve clustering performance
- Speed up computation

---

## 7. Clustering Techniques
Multiple clustering algorithms were applied to ensure a robust solution:
- KMeans
- Gaussian Mixture Model (GMM)
- Agglomerative Clustering
- Birch
- DBSCAN

Each model was evaluated using:
- Silhouette Score (higher is better)
- Davies-Bouldin Index (lower is better)
- Calinski-Harabasz Score (higher is better)
- BIC/AIC (for GMM)

---

## 8. Results
The final clustering result produced 3 clusters with the following distribution:
- Cluster 1: 96.5%
- Cluster 2: 2.9%
- Cluster 3: 0.5%

### Interpretation:
This result indicates that the clustering is highly imbalanced. A large majority of the tickets were grouped into a single cluster, while the remaining clusters contained only a small fraction of the data.

This suggests that:
- The model was not able to effectively separate distinct issue categories
- Most tickets appear similar in the feature space after preprocessing
- Smaller clusters likely represent outliers or rare cases

Therefore, this clustering result is not ideal for meaningful segmentation.

---

## 9. Conclusion
The assignment successfully implemented:
- CI removal from ticket text
- Text preprocessing and EDA
- Multiple clustering techniques
- Model evaluation using standard metrics

However, the final clustering result showed strong imbalance, indicating that further improvements are needed for better separation of ticket categories.

---

## 10. Future Improvements
To improve clustering performance, the following steps can be taken:

1. **Use advanced text embeddings**
   - Replace TF-IDF with sentence embeddings (e.g., Sentence-BERT)
   - This captures semantic meaning better

2. **Improve preprocessing**
   - Ensure important keywords are not removed during cleaning
   - Add domain-specific stopwords

3. **Better clustering selection**
   - Use GMM or Agglomerative clustering with optimized number of clusters
   - Validate stability across different runs

4. **Parameter tuning**
   - Tune hyperparameters such as number of clusters (K), eps (for DBSCAN), etc.

5. **Cluster interpretation**
   - Extract top keywords per cluster
   - Validate clusters using sample ticket inspection

---

## 11. Final Remark
This exercise highlights the importance of proper preprocessing and model selection in text clustering. While initial results were not perfectly balanced, the approach provides a strong foundation for further improvements and real-world application.
