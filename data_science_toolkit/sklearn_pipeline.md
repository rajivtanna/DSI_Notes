# Pipelines

Many organizations rely on data engineering teams to encode these common tasks into pipelines. Data pipelines are a series of automated data transformations that ensure the validity of your work for routine data maintenance tasks

Data Pipelines - The term Pipeline is used to indicate a series of concatenated data transformations. Each stage of the pipeline feeds from the previous stage, i.e. the output of a stage is plugged into the input of the next stage and data flows through the pipeline from beginning to end as water flows through... a pipeline.

__Pipelines in scikit-learn__ - One way to improve coding and model management is to use pipelines in scikit-learn. These tie together all the steps that you may need to prepare your dataset and make your predictions. Because you will need to perform all of the same transformations on your evaluation data, putting this all together makes sense.


__Really useful for using a grid search over various parts of a model pipeline__

Scikit-learn pipelines can also be built using the `make_pipeline` command, which has a simpler syntax where you don't need to pass a name (item gets given a standard name).

Code Example
```
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression

pipe1 = make_pipeline(StandardScaler(), LogisticRegression())    

pipe2 = Pipeline(steps=[('standard_scal',StandardScaler()), ('logistic_regr',LogisticRegression())])

pipe1

pipe2
```

__The preprocessing module comes loaded with many very useful pre-processing classes.__

Data Manipulators
- Binarizer - sets the feature values to a zero or a one depending on a threshold
- KernelCenterer - ??
- MaxAbsScaler - sets the max of each feature to 1 and they keeps the same distribution of data for all other points
- MinMaxScaler - standerdise the features to any range (can be set) using the min and max
- Normalizer - scales features into normal
- PolynomialFeatures - creates a set of polynomial features based on the original data
- RobustScaler - similar to below but uses the inter quartile range (25% to 75%)
- StandardScaler -
- VarianceThreshold - removes features that don't hit a certain variance thereshold

Data Imputation
- Imputer

Function Transformer
- FunctionTransformer

Label Manipulators
- LabelBinarizer
- LabelEncoder
- MultiLabelBinarizer

### Other tools that can be used for preprocessing (ETL)

While scikit-learn pipelines help with managing the transformation from raw data, there may be many steps before this takes place in your pipeline. These pipelines are often referred to as _ETL pipelines_ for "Extract, Transform, Load." In an _ETL pipeline_, the data is pulled or extracted from some source (like a database), transformed or manipulated, and then "loaded" into whatever system or analysis requires them. We will see this in the lab. Other ways you can manage such ETL pipelines besides the sklearn tools include:

- [Luigi](https://github.com/spotify/luigi), developed by Spotify
- [Airflow](https://github.com/airbnb/airflow), developed by AirBnB.


NOTE - You can implement custom transformers. This is probably something that would need to be done for production code. __REALLY USEFUL__
```
from sklearn.base import BaseEstimator, TransformerMixin
import numpy as np

# the inheritance of BaseEstimator and TransformerMixin allows use of methods such as fit_transform()
# which are not explicitly defined here (you can just always import them for this purpose)
class FeatureMultiplier(BaseEstimator, TransformerMixin):

    # This initialises the object, any argument we want to define has to appear here
    def __init__(self, factor):
        self.factor = factor

    # This is where the actual transformation that we want occurs
    def transform(self, X, *_):
        return X * self.factor

    # This needs to be defined but we don't want it to do anything
    def fit(self, *_):
        return self

fm = FeatureMultiplier(2)

test = np.diag((1,2,3,4))
print test

fm.transform(test)
```

Once you have all the pipelines you can create a union of them:

```
from sklearn.pipeline import make_union

pipe_union = make_union(pipe1,pipe2,pipe3,pipe4)
```

Finally, you can save the pipeline object to the drive in a process called pickling
```
import dill
import gzip

with gzip.open('union.dill.gz', 'w') as fout:
    dill.dump(pipe_union, fout)
```
