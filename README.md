Airline Passenger Segmentation
🎯 Project Aim
The goal of this project is to analyze a dataset of airline passengers and segment them into distinct groups based on their frequent flyer program history and behavior. This segmentation allows the airline to understand its customer base better and apply targeted marketing strategies to different groups, such as loyal customers, at-risk customers, or new members.

🛠️ Tools Used
Python

Pandas: For data manipulation and analysis.

Scikit-learn: For implementing clustering algorithms (K-Means, Hierarchical, DBSCAN), scaling data (StandardScaler), and dimensionality reduction (PCA).

Matplotlib & Seaborn: For data visualization, including plotting the clusters.

SciPy: For generating the dendrogram in hierarchical clustering.

⚙️ Methodology / Working
The project was executed in the following steps:

Data Preprocessing: The raw data was cleaned, and the non-essential ID# column was removed. All features were then scaled using StandardScaler to ensure that distance-based algorithms perform accurately.

Model Implementation: Three clustering algorithms were applied to the scaled data:

K-Means: The optimal number of clusters (K=4) was determined using the Elbow Method.

Hierarchical Clustering: A dendrogram was used to identify the optimal cluster count (2 clusters).

DBSCAN: Parameters (epsilon and min_samples) were determined using a k-distance plot to group dense data points and identify outliers.

Visualization: Principal Component Analysis (PCA) was used to reduce the dataset's dimensions to two components, allowing the resulting clusters from each algorithm to be plotted and visually inspected.

🏁 Conclusion & Evaluation
The performance of the three models was compared using the Silhouette Score, which measures the quality of the clusters.

Clustering Algorithm	Silhouette Score
K-Means Clustering	0.242
Hierarchical Clustering	0.357
DBSCAN	0.312

Based on the evaluation, Hierarchical Clustering yielded the best Silhouette Score (0.357), indicating it created the most distinct and well-defined passenger segments for this particular dataset. The analysis of these clusters revealed key customer personas (e.g., frequent fliers, inactive members) that the airline can now use for targeted and effective marketing campaigns.

