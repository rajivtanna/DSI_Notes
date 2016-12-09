# Decision Trees

* Non-parametric
* Hierarchical
* Classification & regression
* DAG: Directed Acyclic Graphs


## Classification trees

Classification trees are very similar to regression trees. Here is a quick comparison:

|regression trees|classification trees|
|---|---|
|predict a continuous response|predict a categorical response|
|predict using mean response of each leaf|predict using most commonly occuring class of each leaf|
|splits are chosen to minimize MSE|splits are chosen to minimize a different criterion (discussed below)|

Note that classification trees easily handle **more than two response classes**! (How have other classification models we've seen handled this scenario?)

Here's an **example of a classification tree**, which predicts whether or not a patient who presented with chest pain has heart disease:


```
Seeing the tree

from IPython.display import Image
from sklearn.tree import export_graphviz
from sklearn.externals.six import StringIO
import pydot
dot_data = StringIO()  
export_graphviz(clf_gs, out_file=dot_data,  
                feature_names=X.columns,  
                filled=True, rounded=True,  
                special_characters=True)  
graph = pydot.graph_from_dot_data(dot_data.getvalue())  
Image(graph[0].create_png())

```


## Use GridSearchCV to find the best Regression Tree

How do we know by pruning with max depth is the best model for us? Trees offer a variety of ways to pre-prune (that is, we tell a computer how to design the resulting tree with certain "gotchas").

Measure           | What it does
------------------|-------------
max_depth         | How many nodes deep can the decision tree go?
max_features      | Is there a cut off to the number of features to use?
max_leaf_nodes    | How many leaves can be generated per node?
min_samples_leaf  | How many samples need to be included at a leaf, at a minimum?  
min_samples_split | How many samples need to be included at a node, at a minimum?

Using grid search and visualising the tree

```
# Grid search
parameters = {'max_depth':range(1,4,1), 'max_features':range(2,5,1), 'min_samples_leaf':range(1,5,1)}
clf_gs = tree.DecisionTreeClassifier(random_state=1)
gs = GridSearchCV(clf_gs, parameters, verbose=True, cv=2, scoring='neg_mean_squared_error')
gs.fit(X, y)
mf = gs.best_params_['max_features']
md = gs.best_params_['max_depth']
msf = gs.best_params_['min_samples_leaf']
print gs.best_params_

# Fit a new model
clf_gs1 = tree.DecisionTreeClassifier(max_depth = md, max_features=mf, min_samples_leaf=msf)
clf_gs1 = clf_gs1.fit(X_train, y_train)
y_pred_gs1 = clf_gs1.predict(X_test)
r2_score(y_test, y_pred_gs1)

# Visualise the tree  (remember to import all the above)
dot_data = StringIO()  
export_graphviz(clf_gs1, out_file=dot_data,  
                feature_names=X.columns,  
                filled=True, rounded=True,  
                special_characters=True)  
graph = pydot.graph_from_dot_data(dot_data.getvalue())  
Image(graph.create_png())
