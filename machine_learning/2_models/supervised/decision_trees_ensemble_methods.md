# Decision Trees - Ensembles & Bagging

### Ensembles
Ensemble techniques are supervised learning methods to improve predictive accuracy by combining several base models in order to enlarge the space of possible hypotheses to represent our data. Ensembles are often much more accurate than the base classifiers that compose them.

**Check:** Short discussion:
- when this might be useful?
- can you think of a business case where we may want to get a very very accurate model (despite it being more complex)

Two families of ensemble methods are usually distinguished:

- In averaging methods, the driving principle is to build several estimators independently and then to average their predictions. On average, the combined estimator will often then perform better than any of the single base estimators.

Examples of this family include Bagging methods and Random Forests.

- The other family of ensemble methods are boosting methods, where base estimators are built sequentially. The motivation is to combine several weaker models to produce a more powerful ensemble. We will discuss these in a future lecture.

Examples of this family include AdaBoost and Gradient Tree Boosting.

### Characteristics of Ensemble methods

In order for an ensemble classifier to outperform a single base classifier, the following conditions must be met:

- **accuracy**: base classifiers outperform random guessing
- **diversity**: misclassifications must occur on different training examples

__Bagging can be used with other models but works particularly well for Decision Trees__


Ensembles are a way of averaging different models so that we can come out with the best predictions
  * In the case of a regression we average the predictions
  * In the case of classification we need to come up with a method to work out the final prediction based on the results from the individual models

## Bagging - A method to create additional sample data for a small data set

* The method copies a data point into a new set randomly. Importantly compared to cross validation data can be bagged twice.
* In addition we can actually create many bags (not just 5 or 10) to allow us to fit various models on different bags.

In practice we run the same model on the various 'bags' to train the each model and then take the ensemble to fit the results.
To make sure that we don't over report the score we always use bagging on the training set and still do the final score on the test set. This is also a good way to make sure the model does not over fit on the training set.


## Introduction: Bagging

_Bootstrapping_ is a general process by which we take a subset of our data _with replacement_, so rather than something we have seen before with methods like cross validation where we clearly divide our data set into the train
and the cross validation and we do not mix the two - which is subsetting the data _without replacement_ - in the case
of bootstrapping we reach into the pool of possible datapoints and take one out, then when we reach in again to pull
out the next data point we could potentially pick the same data point as we already pulled out. This is a random
process whether a data point is repeated or not, but given the probability of being chosen we expect around 2/3 of the data to be chosen in each bag and 1/3 are likely to be repeated picks if we take a sample which is the same size as the original dataset. We can then
compare variation in the statistics such as mean and variance amongst the different bootstrap samples as a way of drawing conclusions around our dataset and so gain more statistical insight with a smaller number of datapoints than simply our total mean and variance as our only two statistics.

_Bagging (Bootstrap AGGregatING)_ is a method that involves fitting multiple classifiers/regressions by resampling the dataset using bootstrapping. We learn $k$ base classifiers on $k$ different samples of training data.  These samples are independently created by resampling the training data using uniform weights (eg, a uniform sampling distribution). In other words each model in the ensemble votes with equal weight (unlike other methods such as boosting we look at tomorrow). In order to promote model variance, bagging trains each model in the ensemble using a randomly drawn subset of the training set.

|Original|1|2|3|4|5|6|7|8|
|----|----|----|----|----|----|----|----|----|
|Training set 1|2|7|8|3|7|6|3|1|
|Training set 2|7|8|5|6|4|2|7|1|
|Training set 3|3|6|2|7|5|6|2|2|
|Training set 4|4|5|1|4|6|4|3|8|

Given a standard training set $D$ of size $n$, bagging generates $m$ new training sets $D_i$, each of size $n′$ (which can be equal to $n$), by sampling from $D$ uniformly. By sampling with replacement, some observations will be repeated in each $D_i$. The $m$ models are fitted using the above $m$ samples and combined by averaging the output (for regression) or voting (for classification).

**Check:** Can you propose another sample to add to those above? Call out the numbers you would include.

Bagging reduces the variance in our generalisation error by aggregating multiple base classifiers together (provided they satisfy our earlier requirements). Since each sample of training data is equally likely, bagging is not very susceptible to overfitting with noisy data. As they provide a way to reduce overfitting, bagging methods work best with strong and complex models (e.g., fully developed decision trees), in contrast with boosting methods which usually work best with weak models (e.g., shallow decision trees). Boosting is discussed tomorrow and involves refitting the model with a bag that is weighted towards misclassified data points in the prior pass, and then refitting and passing over again and again. Random Forests, certainly the most popular ensemble decision tree method, is then another iteration on this with random subselecting of predictors (I am deliberately not going into this too much since it is discussed tomorrow, just providing some context in which to place the bagging approach).

When it comes to reporting error rates with the bagging method, we do not have clear cut training and test sets. For this then various algorithms have been developed to come up with reasonable estimates of the generalisable test error by keeping track of the points collected in the bag and the points out of the bag, and through various averaging arriving at a reasonable estimate of the expected test error that is tolerable for most applications but obviously not as clear cut as the cross validation approach. I've provided a copy of a 1986 review paper which covers such approaches for those who are interested [here](./assets/papers/bootstrap_error_estimates.pdf). I've also provided a 1996 paper which describes bagging predictors as implemented in sklearn [here](./assets/papers/bagging_predictors.pdf) (i.e. the sklearn implementation was based on this paper).  Alternatively, we can simply calculate score based on our own cross validation / train test split and so we know we will not be using points that might be present in the bagged sets (this is the approach taken below), and this removes this element of concern though it means, of course, fewer points from which to select for the training bags.

Bear in mind that bootstrapping and fitting multiple models based on a bagging approach is just one way to try to get as much from a limited dataset as possible. Another would be to assume the distribution is normal and randomly generate new data from a distribution with the same mean and standard deviation as the sample (such as the Monte Carlo simulation approach we saw last week). You could then fit multiple models on such datasets and average them in a similar way. Bootstrapping does not make assumptions about the underlying distribution of the data because it simply treats every point as equally likely to be selected (even if that point has been selected before). Typically we might fit several hundred models in the ensemble.

__Example Code__
```
from sklearn.tree import DecisionTreeClassifier

trees=DecisionTreeClassifier()
bagging2=BaggingClassifier(trees, n_estimators=100, random_state=42)

print("Trees Score:\t", cross_val_score(trees, X, y, cv=10).mean())
print("Bagging Score:\t", cross_val_score(bagging2, X, y, cv=10).mean())
```

## APPENDIX - ADDITIONAL INFORMATION

### Cross Validation, test sets, and bootstrapping clarification

We have something of a proliferation of sampling methods so let's go through them. We know that `train_test_split` allows us to split our data and perform model fitting on train data and report a score based on applying that fit to the test data, which more realistically tells us how our model is likely to perform against new data than testing against the training data. Hence we don't necessarily always apply such splits, it depends what we are reporting. In the case of reporting the error, it is a good idea because the error on the train set will be unrealistically low. In order to use more of the data points for fitting, we can do a cross validation method which splits our data into folds for us, performs a fit many times and reports the error on each fold (which was not used for the fitting), but still allows us to overall use all of the data points for fitting. Hence when we use `cross_val_score`, we report our best estimate of the accuracy score by using cross validation rather than using eg a train-test split. That's because we use all of our available data points for the fitting (overall) and we use all our available data points for the testing (overall). It also means we can use fewer lines of code since sklearn implements this for us. So there's a double win there by using that as our default method of reporting accuracy, rather than a `train_test_split`.

When we perform a `GridSearchCV`, this is also using k folds cross validation (using the `cv` parameter this time, rather than `k` because it can actually take different arguments than just an integer if you wish to set it up this way - if this parameter is None it will default to 3 folds, if it is an integer it behaves the same way as `k`). That's required for this method (there is no grid search without cross validation implemented) so we report a realistic score for each element of the grid by fitting and testing across each fold for each parameter. Then the hyperparameter that reported to best averaged score across the folds of cross validation is returned as the optimal. In this case putting a cross validated algorithm such as `LogisticRegressionCV()` within the `GridSearchCV` would not serve an advantage as there would be an overproliferation of splits of the data in a redundant manner.

One case we saw last week where there is an advantage of putting a cross validating operater with a `GridSearchCV` is the `RFECV`. That's because the `RFECV` is a case which actually has a different functional purpose to `RFE`, in that it returns an optimal number of features to keep - whereas for `RFE` there is no way to calculate this so the user must input the number of features to keep as a parameter. In `RFE` one does not report a score, since the aim is simply to rank features based on coefficient and retain the most important ones down to a user-specificied number. For `RFECV` we use the score from cross validation to determine how the iteratively formed models with different numbers of features actually perform and then return the number of features that returned the best score. So since this serves a different functional purpose to `RFE`, it does in that case make sense to use it within a `GridSearchCV`.

For the bootstrap, note that this is simply a different method of sampling the data. We could, for example, take different folds of the data and fit our classification trees to each of those folds and average them. By bootstrapping we can more straightforwardly increase the number of bags we use to the hundreds, or even thousands, without limiting our model fits by having too few data points in each. Since we are not using this approach to directly report an error, it is not a problem that we re-use data points. In the end we are simply sampling the underlying population in differing ways. Bootstrapping would not make much sense for `train_test_split` since we would end up reporting a score for the test that included points that were in the training set, but for the bagging approach it allows us to have a greater number of splits of our data and indeed model performance often improves by this method (though we see modest improvement here, it can be dramatic).


### The Hypothesis space

In any supervised learning task, our goal is to make predictions of the true classification function $f$ by learning the classifier $h$. In other words we are searching in a certain hypothesis space for the most appropriate function to describe the relationship between our features and the target.

**Check:** Can you give an example of how this search is performed using one of the classifiers you know?

**Check:** What reasons could be preventing our hypothesis reaching perfect score?

There could be several reasons why a base classifier doesn't perform terribly well in trying to approximate the true classification function. We can call the three types:
- statistical problem
- computational problem
- representational problem

#### The statistical problem
If the amount of training data available is small, the base classifier will have difficulty converging to $h$.

An ensemble classifier can mitigate this problem by "averaging out" base classifier predictions to improve convergence. This can be pictorially represented as a search in a space where multiple partial perspectives are combined to obtain a better picture of the goal.

![Statistical Problem](./assets/images/statistical.png)

(source: http://www.cs.iastate.edu/~jtian/cs573/Papers/Dietterich-ensemble-00.pdf)

The true function $f$ is best approximated as an average of the base classifiers.

**Check:** How would you explain the statistical problem?

#### The computational Problem
Even with sufficient training data, it may still be computationally difficult to find the best classifier $h$.

For example, if our base classifier is a decision tree, an exhaustive search of the hypothesis space of all possible classifiers is extremely complex.

In fact this is why we use a heuristic algorithm (greedy search).

An ensemble composed of several _Base Classifiers_ with different starting points can provide a better approximation to $f$ than any individual _Base Classifiers_.

![Computational Problem](./assets/images/computational.png)

The true function $f$ is often best approximated by using several starting points to explore the hypothesis space.

**Check:** How would you explain the computational problem?

#### The representational Problem
Sometimes $f$ cannot be expressed in terms of our hypothesis at all. To illustrate this, suppose we use a decision tree as our base classifier. A decision tree works by forming a rectilinear partition of the feature space, i.e it always cuts at a fixed value along a feature.

![Decision Tree boundary](./assets/images/dtcut.png)

But what if $f$ is a diagonal line?

Then it cannot be represented by finite rectilinear segments, so the 'true' decision boundary cannot be obtained by a decision tree classifier.

However, it may be still be possible to approximate $f$ or even to expand the space of representable functions using ensemble methods.

![Representational Problem](./assets/images/representational.png)

### ADDITIONAL RESOURCES

- [Ensemble models on wikipedia](https://en.wikipedia.org/wiki/Ensemble_learning)
- [Bagging on wikipedia](https://en.wikipedia.org/wiki/Bootstrap_aggregating)
- [Ensemble methods on Scikit Learn](http://scikit-learn.org/stable/modules/ensemble.html)
- [Bagging Classifier documentation](http://scikit-learn.org/stable/modules/generated/sklearn.ensemble.BaggingClassifier.html)
- [Bias Varias Decomposition Scikit Learn Example](http://scikit-learn.org/stable/auto_examples/ensemble/plot_bias_variance.html#example-ensemble-plot-bias-variance-py)
