## Introduction: DBSCAN

```
from sklearn.cluster import DBSCAN

db = DBSCAN(eps=0.3, min_samples=10)
db.fit(X)
```

So far you've learned about two types of clustering algorithms - k-means clustering and hierarchical clustering. Now, we're going to learn about one last clustering algorithm - density-based spatial clustering of applications with noise, or as we'll call it from now on, DBSCAN. DBSCAN is actually probably the most widely used and applicable clustering algorithm - given that it takes minimum predefined input and can discover clusters of any shape, not just the sphere-like clusters that k-means often computes. This way, we can discover less pre-defined patterns and glean some more useful insights. Remember you are generally clustering in an unsupervised way, and with difficulty visualising the data to get much insight into what sensible numbers of clusters really would be.

#### How does DBSCAN Work?

DBSCAN is a density based clustering algorithm, meaning that the algorithm finds clusters by seeking areas of the dataset that have a higher density of points than the rest of the dataset. Unlike in our previous examples, after you apply DBSCAN there may be datapoints that _aren't assigned to any cluster at all!_ This actually means that DBSCAN generally can give more sensible results than the other algorithms because it can ignore outliers. This can also be really useful for detecting outliers.

When we use DBSCAN, it requires two input parameters: **epsilon**, which is the maximum distance between two points for them to be considered a cluster (maximum cluster radius), and **minimum points**, the minimum number of points necessary to form a cluster (this prevents outliers or small outlier groups from becoming their own cluster). Hence we define a density - both the volume (defined by the distance as a radius) and the effective mass (defined by the number of points within the volume). You must choose these parameters by some trial-and-error, and domain knowledge. There is a variation of DBSCAN called OPTICS which runs a search across epsilon parameters, but this has not been implemented in sklearn - in fact, somewhat incredibly there is a pull request for this feature which is still actively being commented and tweaked three years after its initial submission (see [here](https://github.com/scikit-learn/scikit-learn/pull/1984)).

We might naively suppose that we can simply assign points to a cluster if they have at least the minimum number of points within their epsilon distance volume (which we refer to as their _neighbourhood_), and that all points that are reachable by directly moving from one epsilon volume to another without gaps in between are hence in the same cluster. However we must tweak this to include a concept of core and border points. Intuitively, the density of points on the edge of the cluster must be significantly lower than in the core - so we must treat these differently. Therefore the requirement is broadened such that for any point considered to be part of a cluster, there must be within its neighbourhood some point which has the minimum points within its own neighbourhood. The algorithm will work sequentially, taking an initial starting data point and considering whether it sits within a cluster based on passing the minimum points threshold within its neighbourhood. If so it moves on to all the points in the neighbourhood and performs an assessment on whether those points are also core points or are border points. We would not move on and assess points in a border point's neighbourhood for placement in the cluster. This carries on until all points directly reachable have been labelled as core or border. Once one cluster is finalised, the algorithm then moves to a new datapoint, and seeks to find related points to form yet another cluster; this will continue until DBSCAN simply runs out of data points. Hence in this case, we only consider our data points when considering distances (we never make reference to arbitrary points in space).

This [website](https://www.naftaliharris.com/blog/visualizing-dbscan-clustering/) does a good job of showing how the algorithm passes through points and develops the cluster.

[Here](./assets/papers/dbscan.pdf) is the original paper describing dbscan.


#### How does DBSCAN differ from K-Means and Hierarchical Clustering?

Whereas k-means can be thought of as a "general" clustering approach, DBSCAN performs especially well with unevenly distributed, non-linear clusters. The fundamental difference with DBSCAN lies in the fact that it is *density based* rather than k-means, which calculates clusters based on distance from a central point, or hierarchical clustering. When choosing epsilon in the minimum points in DBSCAN, a selection of < 2 will result in a linkage cluster - essentially the same result as if you were to perform a hierarchical clustering. To diversify the DBSCAN, we therefore must give it a significant amount of points to form a cluster.

DBSCAN can be particularly useful to us when we have a lot of dense data. If we used k-means on this data, the algorithm would effectively give us just one large cluster! However with DBSCAN, we can actually break down this cluster into smaller groups to see their attributes. Additionally DBSCAN performs well with outliers, which will not skew the clustering (since points can be assigned to no cluster).

**Check:** What does it do differently from k-means and hierarchical clustering?
