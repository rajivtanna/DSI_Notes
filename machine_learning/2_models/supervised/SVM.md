# SVM (Support Vector Machines)

```
# Basic Linear Model
from sklearn.svm import SVC

linear_model = SVC(kernel='linear')
linear_model.fit(X_train,y_train)
linear_model.score(X_test, y_test)

# Basic RBF Model
model = SVC(kernel='rbf')
model.fit(X_train,y_train)
model.score(X_test, y_test)

```


## Some Theory

### Maximum Margin Classifier

A way of classifying values into sets by creating a hyperplane (in 2d a line that separates points on a graph)

TBD
