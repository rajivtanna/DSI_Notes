# Feature Importance - Tree method

For Parametric Models we use the Coefficients

We can use trees not only to predict but also understand the drivers in our model.

We can use the following methods in tree models to help us with feature importance:
Measure of node purity
* Gini index approaches zero when the proportions of that classifiers at a node is very big of very small
* Cross Entropy - approaches zero when the proportions of the classifiers at a node is are very big of very small

## When using random forests (Or an Ensemble)
1 - We calculate the gains from each sub tree
2 - Calculate the average
