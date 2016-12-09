# Introduction: Feature Selection

## Feature Selection techniques in skLearn
feature_selection.GenericUnivariateSelect([...])
feature_selection.SelectPercentile([...])
feature_selection.SelectKBest([score_func, k])
feature_selection.SelectFpr([score_func, alpha])
feature_selection.SelectFdr([score_func, alpha])
feature_selection.SelectFromModel(estimator)
feature_selection.SelectFwe([score_func, alpha])

Code Example

### Select KBest

```
from sklearn.feature_selection import SelectKBest, f_classif

kbest_selection = SelectKBest(f_classif, k=5).fit(Xt, y)
selected_cols_kbest = kbest_selection.get_support()
kbest_columns = Xt.loc[:,selected_cols_kbest]
kbest_columns.head()

# Check the documentation for SelectKBest, note that the f_classif
# is passed as an argument itself (i.e. not as a string)
```

### Recursive Feature Selection
```
from sklearn.feature_selection import RFE
from sklearn.linear_model import LogisticRegression

lm = LogisticRegression()
selector = RFE(lm,5,step=1)
REF_selection = selector.fit(Xt, y)
selected_cols_REF = REF_selection.get_support()
REF_columns = Xt.loc[:,selected_cols_REF]
REF_columns.head()

```


In machine learning we usually start from a vector of features that encapsulate different aspects of our data and we use these to train a model that predicts a target variable.
Many of the datasets we have encountered so far were in tabular format, and features were well defined already. This is often not the case at all!

**Check** Can you give a couple of examples of data where features must be extracted?


For example, in the case of text data, a data-point usually corresponds to a document or a sentence, and we have to use feature extraction techniques to map each document to a vector of numbers. Similarly, to classify images, we must first represent them as a vector of numbers.
As we have seen with the `CountVectorizer` for text data, feature extraction can result in a very large number of features, some of which may have more predictive power than others.

|Text|the|cat|is|on|table|blue|sky|...|
|----|
|The cat is on the table|2|1|1|1|1|0|0|...|
|The table is blue|1|0|1|0|1|1|0|...|
|The sky is blue|1|0|1|0|0|0|1|...|
|...|...|...|...|...|...|...|...|...|

In the above example the word `the` probably has zero predictive power.

In a different scenario, the tabular data may be a result of a database dump where irrelevant columns were not removed.


|CustomerID |CompanyName |ContactName | ContactTitle |Address|City | Region | PostalCode | Country |Phone | Fax|
|---|
|ALFKI| Alfreds Futterkiste| Maria Anders | Sales Representative | Obere Str. 57 | Berlin|| 12209| Germany | 030-0074321| 030-0076545|
|ANATR| Ana Trujillo Emparedados y helados | Ana Trujillo | Owner| Avda. de la Constitución 2222 | México D.F. || 05021| Mexico| (5) 555-4729 | (5) 555-3745|
|ANTON| Antonio Moreno Taquería| Antonio Moreno | Owner| Mataderos2312 | México D.F. || 05023| Mexico| (5) 555-3932 ||
|...|...|...|...|...|...|...|...|...|...|...|

Feature Selection is a way to reduce the number of features to simplify the model while retaining its predictive power.



### Bottom up feature selection / forward stepwise selection

One way to select features is to start with no predictors, so for example a logistic regression against a constant as the sole feature. We then add in each of the p possible features and find the single feature that gives the highest score metric we are testing, and then iteratively add the other features one by one, each time checking how much the score improves and adding specifically the one feature that improves the score the most each time. Score improvement will hence be smaller and smaller the more features we add; adding complexity to the model will yield diminishing returns.

One can then set a criteria to only retain the first N features or the first features that achieve e.g. 90% as good a score as the model with all p features (bear in mind this could be _a lot_ fewer features than p).

You might notice that we do not test every possible model this way, what about all the other combinations of features that are possible? This process is computationally more feasible than an approach taking all possible combinations and likely to yield similar results. Fitting all possible feature combinations is known as the **best subset selection**.

For 20 possible features, there are 211 models to be tested in forward stepwise selection. For best subset selection, there are 1,048,576 models to be tested. How long do you have?


### Top Down feature selection / backward stepwise selection, and hybrid approaches

Obviously, we can do this process in reverse and start with the full model with all features and remove the least predictive one at each step. This would not necessarily end with the same features as the forward stepwise approach. More interestingly, a hybrid approach could be taken with, e.g. a forward stepwise approach that then performans a backward analysis to check if a different feature might be removed each time a new one has been added. This more closely approximates a full best subset fit whilst still involving a smaller number of total model fits.


### Regularisation

Regularisation imposes a global constraint on the values of the parameters that define the model. The regularized model is found solving a new minimisation problem where two terms are present: the term defining the model and the term defining the regularisation.

In general, a regularisation term $R(f)$ is introduced to a general loss function:

$$\min_f \sum_{i=1}^{n} V(f(\hat x_i), \hat y_i) + \lambda R(f)$$

for a loss function $V$ that describes the cost of predicting $f(x)$ when the label is $y$, such as the square loss or hinge loss, and for the term $\lambda$ which controls the importance of the regularization term. $R(f)$ is typically a penalty on the complexity of $f$, such as restrictions for smoothness or bounds on the vector space norm.

#### l1 and l2 regularisation

To reiterate, for the case where our model $f$ is parametric (e.g. like in linear models), we can express the regularisation term as a norm on the parameters. Depending on the kind of norm chosen for the parameters regularisation can be:

l1 or Lasso regularisation:
$$ R(f) \propto \sum{|\beta_i|}$$

or

l2 or Ridge regularisation:
$$ R(f) \propto \sum{\beta_i^2}$$

where $\beta_i$ are the parameters of the model. Critically, l1 regularisation can set parameter coefficients to zero and thus act as a feature selection method.

### Feature importance

Some models offer an intrinsic way of evaluating feature importance. For example, as we will see next week, _Decision Trees_ have a way to assess how much each feature is contributing to the overall prediction.

Logistic Regression also offers an intrinsic way of assessing feature importance. In fact, since the Logistic Regression model is linear in the features, the size of each coefficient represents the impact a unit change in that feature has on the overall model. In order to interpret this as feature importance, we need to make sure that features are normalized to have the same scale, or a unit change (i.e. the coefficient) would imply varying change in the odds outcome.

# Recursive Feature Elimination

I will explicitly discuss recursive feature elimination (RFE) in sklearn since we use this in the upcoming lab and it might be a bit unclear. This involves fitting models several times. You fit the model once with all the features, then you drop one (or more, you can set this parameter) and then refit, then drop one and refit (until you get to the number of features you input also as a parameter at the start). How do you decide which of the features you want to drop? In the case of logistic regression it is based on the coefficient size with some extra corrections. Hence this is a version of top down (or backwards) stepwise selection, with attempts to be more intelligent than only comparing coefficient size.

```
from sklearn.feature_selection import RFE
```

Can also use RFECV to incorporate cross validation.

# Statistical Testing for Feature Selection

Some of the methods we will see in the lab use some statistical tests to determine if features are significant. We have talked about statistical testing in the context of t-tests. In that case, we were looking to see if
two statistics (such as a mean value for the heights of a population) were significantly different from each other by assuming either a normal (for large numbers of values, where we can approximate a normal distribution) or a t (for smaller numbers of values i.e. below 30) distribution. For the normal case this is called a z-test, for the t a t-test. The distinction in the normal and t distributions was in the kurtosis, such that a t distribution would have a greater tolerance for more extreme values and judge these to be less significant than a normal distribution.

Another method used to compare two or more statistics (again such as means of heights of different subgroups of a population) at the same time is the analysis of variance (ANOVA). That's useful because the t test just compares two values. In this case an F-statistic is calculated (please note this is totally unrelated to the F1 score you calculate as a metric for classification problems) and an F-test
is performed on the F-distribution to see if the F statistic is significant, again returning a p-value.

Hence the
flow of such tests follow the same format. You calculate a statistic (Z-statistic, t-stastic, F-statistic), compare the value to the appropriate distribution (Z distribution, which is just the normal distribution, the t distribution, or the F distribution) and return a p value giving the probability of the statistic occuring. So the difference is in the calculation of the statistic and the exact form of the distribution used for the testing. The F distribution is positively skewed, unlike the normal and t distributions which are symmetric.

For the F-statistic, we calculate a sum of the variances of each grouping compared with the overall mean of the total, and divide this by a sum of the variances of each data point within the groupings compared with the mean of that grouping (with a correction for number of terms). That is to say, _a term for the variability between groups divided by a term for the variability within groups_.


In a classification problem this can be applied by considering the groupings to be each classification outcome. So to perform the test we need to input both our feature matrix and our outputs. Then the features are subset into those corresponding to each of the possible outcomes and we see if the means of the features in each grouping are significantly different from each other. If they are not then the feature seems to not provide much predictive power.

The equation below shows the F-statistic calculation for a binary outcome classification where (+) refers to the grouping of one outcome and (-) the grouping for the second outcome. Note that this generalises to multivariate classification, which is why it is convenient to use it even for the binary case rather than another test.

![](./assets/images/f_statistic.png)
