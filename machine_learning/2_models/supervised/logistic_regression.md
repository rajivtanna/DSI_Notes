# Logistic Regression

Consider again the Default data set, where the response default falls into one of two categories, Yes or No. Rather than modeling this response Y directly, logistic regression models the probability that Y belongs to a particular category.

The values of Pr(default = Yes|balance), which we abbreviate p(balance), will range between 0 and 1. Then for any given value of balance, a prediction can be made for default. For example, one might predict default = Yes for any individual for whom p(balance) > 0.5

As the probability must vary between 0 and 1 we use a logistic regression to calculate this:

![logistic_functions](logistic_functions.png)

_The logistic function will always produce an S-shaped curve of this form, and so regardless of the value of X, we will obtain a sensible prediction_

After manipulation of the formula above we get the following:

![logistic_function_rearranged](logistic_functions_rearranged.png)

The left hand side is called the Log Odds or Log it. Note logit is equal to a linear function.

Note: The model is fit using maximum likelihood function.

__Threshold that is typically used: over 0.5__

Calculations
---
Probabilities to Odds
odds = probability / (1 - probability)
probability = odds / (1 + odds)

Logistic function
sigma(x) = 1 / (1 + np.exp(-x)) = np.exp(x) / (1 + np.exp(x))

Odds = probability/(1-probability)

Log Odds = natural logarithm of Odds
- note np.log() calculates natural logarithm as the default, natural logarithm is the log to the base of the Euler number e = 2.718...

Logarithm expresses the power a number must be raised by to get the result:
- Log2(8)=3, meaning the number 2 (the base) must be raised by 3 to obtain 8
- Log10(100)=2, meaning the base 10 must be raised by 2 to obtain 100

Logistic regression is a great classification model because we can really interpret and understand what it's doing. Other models like support vector machines and random forests are pretty cool and get great results, but interpretability can be an issue. There's really a sense in which logistic regression is much more interpretable and for this reason it continues to be very popular.

Stats Model - Example Code
```
import statsmodels.formula.api as smf

model1 = smf.logit(formula="label ~ is_news",data = data).fit()
model1.summary()

```

SciKit Learn - Example Code
```
from sklearn.linear_model import LogisticRegression

#Initiate the model
logreg = LogisticRegression()

# Fit the model
logreg.fit(X_train, Y_train)

# Returns the probability
Y_pred = logreg.predict(X_test)

# Returns the class
Y_probabilities=logreg.predict_proba(X_test)
```

__Function to examine coefficients__
```
def examine_coefficients(model, df):
    df = pd.DataFrame(
        { 'Coefficient' : model.coef_[0] , 'Feature' : df.columns}
    ).sort_values(by='Coefficient')
    return df[df.Coefficient !=0 ]
```

## Setting C hyperparameter in the context of Binomial Logistic Regression

The C hyperparameter is used as the inverse of the alpha for regularization of variables.
  * It is automatically set to 0 in the scikit learn implementation
  * If we want to "switch-off" regularization then we can set the C value to very high.


__The logistic Cv function can iterate over different C values (set by the Cs variable).__
  * NOTE: You can also enter a list of Cs.

```
logregCV = LogisticRegressionCV(Cs=15, cv=5, random_state=5, solver='liblinear')

logregCV.Cs_ # Returns the value for the 15 C hyperparamters

# Finds the best C
print('best C for class:')
best_C = {logreg_cv.classes_[i]:x for i, (x, c) in enumerate(zip(logreg_cv.C_, logreg_cv.classes_))}
print(best_C)
```  

If the objective is to maximise the accuracy then you can also use Grid Search

[Documentation](http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegressionCV.html)

## Monomial Logistic Regression

Can be used to predict the case where there are more than two outcomes.
__NOTE: This is automatically implemented in the SciKit learn logistic regression function__

Ok, so now we have some idea about this process let's also investigate for a multinomial logistic regression.

Previously we have introduced logistic regression as a format to return a probability for being in one of two classes
(e.g. defaulted on loan or not, passed test or not). However, we can also use logistic regression for multiclass problems. This is multinomial logistic regression (as opposed to binomial logistic regression). An example might
be whether someone will buy one of five different products from your website, depending on their browsing habits and geolocation.

In this case, we consider separate logistic fits for each outcome (actually, each outcome minus one) and then compare the results with each other (all within one model implementation).

For this to work there are some assumptions, such as that only one outcome is valid (i.e. outcomes should be mutually exclusive). They are also complete, in that there isn't another possible outcome (or rather, our model will miss this information if it is not included in the fit).

You can fit this in such a way that each possible outcome is fit against all others (one-vs-rest), and this is then cycled through for each of the possible outcomes (actually the total number of outcomes minus one, and you calculate the last probability by just seeing what the remaining probability is compared to a total of 1). Hence rather than fitting an outcome vs no outcome binomial case, you are fitting for an outcome vs all other outcomes multinomial case (note that it's also possible to implement a separate multinomial classifier algorithm, but we need not get into this distinction).

Hence, with each cycle through, you find the probability of one outcome (so that's your 1 in the binary case) compared to any other outcome (so that's your 0). You then go through the other cases until (if there are n outcomes) you have fit n-1 cases, and for the last you compare the sum of the previous probabilities to 1 so that in each case the total probability sums to 1 - meaning you have covered all possible outcomes in the model.

## Understanding Odds ratio

### What is an odds ratio?
An odds ratio (OR) is a measure of association between an exposure and an outcome. The OR represents the odds that an outcome will occur given a particular exposure, compared to the odds of the outcome occurring in the absence of that exposure. Odds ratios are most commonly used in case-control studies, however they can also be used in cross-sectional and cohort study designs as well (with some modifications and/or assumptions).

### When is it used?
Odds ratios are used to compare the relative odds of the occurrence of the outcome of interest (e.g. disease or disorder), given exposure to the variable of interest (e.g. health characteristic, aspect of medical history). The odds ratio can also be used to determine whether a particular exposure is a risk factor for a particular outcome, and to compare the magnitude of various risk factors for that outcome.

__* OR=1 Exposure does not affect odds of outcome
* OR>1 Exposure associated with higher odds of outcome
* OR<1 Exposure associated with lower odds of outcome__


[Worked Example](http://blog.yhat.com/posts/logistic-regression-python-rodeo.html)
[Useful link](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2938757/)


## Measuring the model
---

Can use:
* F1 score
* Accuracy
* AUC or a ROC Curve
* Confusion Matrix

_see [Confusion Matrix](confusion_matrix.md)_

```
from sklearn.metrics import accuracy_score

acc = accuracy_score(Y_test, Y_pred)
print(acc)


from sklearn.metrics import classification_report

cls_rep = classification_report(Y_test, Y_pred)
print(cls_rep)

```


http://www.indeed.co.uk/jobs?as_and=data+scientist&salary=&radius=25&l=London&fromage=any&limit=50
