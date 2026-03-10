<h1 align="center">✈️ Airline Passenger Segmentation</h1>

> East West Airlines wants to understand its customers better — who are the loyal high-fliers, who's barely active, and who's just accumulating miles through credit card offers? This project uses three clustering algorithms to segment passengers and answer exactly that.

---

# 📌 Problem

Clustering is used here to group airline passengers by their flying behaviour and reward usage — with no labels, no predefined categories. The goal is to discover natural customer segments that the airline can act on: target loyal customers with upgrades, re-engage dormant ones, or identify reward-heavy non-fliers.

---

# 📂 Dataset

| Property | Details |
|----------|---------|
| File | `EastWestAirlines.xlsx` |
| Source | East West Airlines frequent flyer data |
| Features | 11 numerical features (after dropping `ID#`) |
| Missing Values | None |

**Features:**

| Feature | Description |
|---------|-------------|
| `Balance` | Total miles eligible for award travel |
| `Qual_miles` | Miles counting toward Topflight status |
| `cc1_miles` | Miles earned via frequent flyer credit card (past 12 months) |
| `cc2_miles` | Miles earned via Rewards credit card (past 12 months) |
| `cc3_miles` | Miles earned via Small Business credit card (past 12 months) |
| `Bonus_miles` | Miles from non-flight bonus transactions (past 12 months) |
| `Bonus_trans` | Number of non-flight bonus transactions |
| `Flight_miles_12mo` | Actual flight miles flown (past 12 months) |
| `Flight_trans_12` | Number of flight transactions (past 12 months) |
| `Days_since_enroll` | Days since enrollment date |
| `Award?` | Whether last award was redeemed (1/0) |

> `cc1_miles`, `cc2_miles`, `cc3_miles` are binned: 1 = under 5K · 2 = 5K–10K · 3 = 10K–25K · 4 = 25K–50K · 5 = over 50K

---

# 🔍 Approach

1. **Dropped `ID#`** — unique identifier with no analytical value
2. **StandardScaler** — applied before clustering; all three algorithms are distance-based and scale-sensitive
3. **EDA** — histograms and boxplots across all 11 features to understand distributions and outliers
4. **K-Means** — elbow curve over k=1–10 to find optimal clusters
5. **Hierarchical (Agglomerative)** — dendrogram with Ward linkage to determine natural cluster count
6. **DBSCAN** — K-nearest neighbours distance plot to find optimal epsilon; `min_samples = 2 × features = 22`
7. **Silhouette Score** — evaluated all three algorithms for cluster separation quality
8. **PCA (2 components)** — used only for visualizing clusters in 2D, not for training
9. **Cluster Profiling** — `groupby().mean()` on each algorithm's labels to interpret what each cluster represents

---

# 🤖 Model / Algorithm

| Algorithm | Config | Clusters Found |
|-----------|--------|---------------|
| K-Means | `k=4`, `random_state=42` | 4 |
| Agglomerative Clustering | `linkage='ward'`, Ward dendrogram | 2 |
| DBSCAN | `eps=1.8`, `min_samples=22` | 2 (+ noise points labeled `-1`) |

**Why StandardScaler?**
All three algorithms compute distances — without scaling, `Balance` (miles in the thousands) would completely dominate over `Award?` (0 or 1), making the clustering meaningless.

**Why min_samples=22 for DBSCAN?**
A common rule of thumb: `min_samples = 2 × number of features`. With 11 features, that gives 22.

---

# 📊 Results

**Silhouette Scores:**

| Algorithm | Silhouette Score | Interpretation |
|-----------|-----------------|----------------|
| K-Means (k=4) | 0.276 | Clusters overlap — acceptable given data complexity |
| DBSCAN (eps=1.8) | 0.310 | Slightly better separation |
| **Hierarchical (k=2)** | **Best** | Cleanest separation among the three |

> Hierarchical clustering produced the best-separated clusters in this dataset.

**K-Means Cluster Profiles (4 clusters):**

| Cluster | Segment |
|---------|---------|
| 0 | Frequent fliers — loyal, high balance, large award miles |
| 1 | Credit card miles collectors — miles from shopping/offers, not flights |
| 2 | Long-enrolled but inactive — signed up long ago, now disengaged |
| 3 | New joiners — recently enrolled, still building up rewards |

**Hierarchical Cluster Profiles (2 clusters):**

| Cluster | Segment |
|---------|---------|
| 0 | Active & engaged — flying regularly, rewards to redeem |
| 1 | Inactive — low miles, low flight activity |

**DBSCAN Cluster Profiles:**

| Cluster | Segment |
|---------|---------|
| 0 | Low engagement — less flying, few award redemptions |
| 1 | Premium customers — high award redemptions, frequent fliers |

---

# ▶️ How to Run

```bash
git clone https://github.com/Chaithanya449/Airline-Passenger-Segmentation.git
cd Airline-Passenger-Segmentation
pip install -r requirements.txt
python clustering.py
```

> The script loads `Airlines(1).csv` — make sure the file is in the same directory. The uploaded file is `EastWestAirlines.xlsx`; convert or rename as needed.

---

# 🛠️ Tech Stack

![Python](https://img.shields.io/badge/Python-3.11-blue?logo=python)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Wrangling-lightgrey?logo=pandas)
![Scikit-learn](https://img.shields.io/badge/Scikit--learn-Clustering%20%7C%20PCA-orange?logo=scikit-learn)
![SciPy](https://img.shields.io/badge/SciPy-Dendrogram-blue)
![Seaborn](https://img.shields.io/badge/Seaborn-Visualization-9cf)
![Matplotlib](https://img.shields.io/badge/Matplotlib-Plots-yellow)

---

# 📁 Project Structure

```
Airline-Passenger-Segmentation/
├── clustering.py           # Main script
├── EastWestAirlines.xlsx   # Dataset
└── README.md
```

---

# 🔮 Next Steps

The silhouette scores across all three algorithms are on the lower side — that's worth digging into. Want to try different epsilon values for DBSCAN and see if cleaner clusters emerge, because 1.8 was picked from the elbow of the distance plot which is somewhat subjective.

For K-Means specifically, k=4 was selected from the elbow curve but the silhouette score at k=10 was 0.45 — noticeably better. The tradeoff is interpretability vs separation. Would like to find the middle ground, maybe k=6 or k=7, and check if the clusters still make business sense.

Also want to try clustering on a reduced PCA feature space instead of the full 11 features — high-dimensional data tends to hurt distance-based methods and that might explain the low silhouette scores here.

Finally, cluster profiling with visualizations like radar/spider charts would make the segment differences much easier to communicate to a non-technical audience.

---

# 👤 Author

**Chaithanya Krishna** · [LinkedIn](https://www.linkedin.com/in/chaitanyakrishna-profile) · [GitHub](https://github.com/Chaithanya449)
