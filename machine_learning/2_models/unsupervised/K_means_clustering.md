# Clustering

Techniques to find sub groups in the data

A key issue is defining a "similarity" metrics:
* Euclidean
* Semantic (e.g. car like bus)
* syntactic (e.g. car =  bar)
* "correlation"

## K-Means Clustering

* We want to partition the space into K distinct, non-overlapping clusters
* A good cluster is one in which the within cluster variation in minimal and between cluster variation is max.

```
from sklearn.cluster import KMeans

cluster_model = KMeans(n_clusters = 5)
cluster_model.fit(X)

from sklearn.metrics import silhouette_score
silhouette_score(X, cluster_model.labels_, metric='euclidean')
```


__What is the algorithm?__

1) Randomly assign each observation to a different clusters
2) Iterate until assignment doesn't Change
  a) Compute the cluster centroids
  b) Reassign points to the cluster whose centroid is closest

K-means is unstable / sensitive to small changes in the data
  - We can run it multiple times and chose the "best one"

__Calculating distance between clusters:__
- Centroid - usually gives the worst results
- Single linkage - often gives the best results
- Complete linkage - comparing the longest pair distance between two points in different clusters
- Average

__Silhouette coefficient:__ It measures how similar an observation is to its own cluster (cohesion), compared to the closest cluster.
FORMULA: s.c. = b - a / max(b,a)
where b = average intra-cluster distance and a = average near-cluster distance

* Notes
  * Hard to define what is a good k. The best way is to plot how the silhouette coefficient changes with increasing k and looking for when there is an obvious change.
