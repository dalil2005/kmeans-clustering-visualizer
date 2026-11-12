# K-Means Clustering: Explained and Simulated

An English explanation of the K-Means algorithm, plus an interactive web simulation that animates it **one iteration at a time** (you choose the number of points and k).

**Live demo:** enable GitHub Pages (see below) and open `https://<your-username>.github.io/<repo-name>/`

## Project structure

```
kmeans-project/
├── index.html   # Interactive simulation with built-in explanation (no dependencies)
├── README.md    # This document
├── LICENSE      # MIT
└── .gitignore
```

## 1. What is K-Means?

K-Means is an **unsupervised** machine learning algorithm. It takes data with no labels and splits it into **k groups (clusters)** so that points in the same group are similar to each other.

Each cluster is represented by its **centroid**, the average position of all its points.

Common uses: customer segmentation, image color compression, grouping documents, finding patterns in sensor data.

## 2. The algorithm

Input: a set of points and a number `k`.

1. **Initialize.** Pick `k` points at random as the starting centroids.
2. **Assign.** Give every point to the nearest centroid (usually by Euclidean distance).
3. **Update.** Move each centroid to the mean of the points assigned to it.
4. **Repeat** steps 2 and 3 until no point changes cluster (convergence), or until a maximum number of iterations is reached.

### Pseudocode

```text
choose k random points as centroids
repeat:
    for each point: assign it to the closest centroid
    for each cluster: centroid = mean of its points
until assignments stop changing
```

## 3. The math

K-Means minimizes the **within-cluster sum of squares (WCSS)**, also called inertia:

```
J = Σ_{j=1..k} Σ_{x ∈ C_j} ‖x − μ_j‖²
```

- `C_j` is cluster `j`, and `μ_j` is its centroid.
- The assign step picks `argmin_j ‖x − μ_j‖`, which cannot increase `J`.
- The update step sets `μ_j` to the mean of `C_j`, which is the point that minimizes squared distance to the cluster. This cannot increase `J` either.

Because `J` never goes up and there are finitely many ways to split the points, the algorithm always stops. It finds a **local** minimum, not necessarily the best one.

## 4. Worked example (1D)

Points: `1, 2, 3, 10, 11, 12`, with `k = 2` and starting centroids `1` and `2`.

| Iteration | Centroids | Clusters after assign |
|-----------|-----------|-----------------------|
| 1 assign  | 1, 2      | {1}, {2, 3, 10, 11, 12} |
| 1 update  | 1, 7.6    | |
| 2 assign  | 1, 7.6    | {1, 2, 3}, {10, 11, 12} |
| 2 update  | 2, 11     | |
| 3 assign  | 2, 11     | same clusters, so it **stops** |

## 5. Choosing k

- **Elbow method:** run K-Means for k = 1, 2, 3, ... and plot inertia. Pick the k where the curve bends (the "elbow").
- **Silhouette score:** measures how well each point fits its cluster compared to the next closest one. Higher is better.
- **Domain knowledge:** sometimes the business problem tells you k (for example, 3 pricing tiers).

## 6. Strengths and limits

| Strengths | Limits |
|-----------|--------|
| Simple and fast | You must choose k in advance |
| Scales to large data sets | Sensitive to the random start |
| Easy to interpret | Assumes round, similar-sized clusters |
| Works well on well-separated groups | Sensitive to outliers and to feature scale |

Tips: scale your features first, run several random starts and keep the lowest inertia, and use **k-means++** initialization (the default in scikit-learn) for better starting centroids.

## 7. The simulation

Open `index.html` in any browser. No install or build step is needed.

| Control | What it does |
|---------|--------------|
| Points (10 to 300) | Number of data points. Changing it generates a new data set |
| Clusters k (2 to 8) | Number of centroids |
| Data shape | Groups (blobs), uniform random, or two rings (a case where K-Means fails) |
| Starting centroids | Random points, or k-means++ for better starts |
| Play / Pause | Runs the steps automatically |
| Next step | Advances one stage (assign or update) |
| Restart | Same points, new starting centroids |
| New points | Generates a new data set |
| Speed | Changes animation speed |

What you see: colored points are assigned clusters, stars are centroids, thin lines link points to their centroid, and dashed lines trace each centroid's path. The panel under the animation shows the iteration number, the inertia (WCSS), and a small chart of the inertia after each assign step.



## 8. Courses and resources

| Resource | Provider | Level |
|----------|----------|-------|
| [Machine Learning Specialization](https://www.coursera.org/specializations/machine-learning-introduction) | Andrew Ng, Coursera | Beginner |
| [Clustering, ML Crash Course](https://developers.google.com/machine-learning/clustering) | Google for Developers | Beginner |
| [scikit-learn: k-means guide](https://scikit-learn.org/stable/modules/clustering.html#k-means) | scikit-learn docs | Intermediate |
| [StatQuest: K-means clustering (search)](https://www.youtube.com/results?search_query=StatQuest+K-means+clustering) | Josh Starmer, YouTube | Beginner |
| [Stanford CS229](https://cs229.stanford.edu/) | Stanford | Advanced |
| [Kaggle Learn](https://www.kaggle.com/learn) | Kaggle | Beginner |

Course pages change over time, so check that each link is still current.

### Papers

- Stuart Lloyd, "Least squares quantization in PCM" (the original algorithm)
- Arthur & Vassilvitskii, "k-means++: The Advantages of Careful Seeding"

## License

MIT. See `LICENSE`.
