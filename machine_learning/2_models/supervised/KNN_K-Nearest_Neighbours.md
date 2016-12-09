# KNN: K-Nearest Neighbours

Classifies the point base on the K nearest points - and then selecting what is the majority category for those points.

__Can also use this as a regression method__
* For K - Heuristic for 3 to 10 is good or square root of the number of features.

Pros
* Simple to implement

Cons
* Need to store training set
* Computational intensive - for many features can take a lot of time

[Implementing KNN in Python](http://machinelearningmastery.com/tutorial-to-implement-k-nearest-neighbors-in-python-from-scratch/)

__LIBRARY__
```
from sklearn.neighbors import KNeighborsClassifier
```

Coding example
```
# Import the Library
from sklearn.neighbors import KNeighborsClassifier

# Define the model
neigh = KNeighborsClassifier(n_neighbors=3)

# Fit the model
neigh.fit(X,y)

# Predict
print neigh.predict([[1.1]])

# Can also get the probability it belongs to one class of another
print neigh.predict_proba([[0.9]])

# Model Score


```
