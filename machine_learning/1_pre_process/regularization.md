# Regularization

__[USEFUL BLOG](https://www.analyticsvidhya.com/blog/2016/01/complete-tutorial-ridge-lasso-regression-python/)__

In modelling terms, we want the least complex model that captures all the information in our data
  * If we have two models that explain our data equally well and one of the models has fewer parameter then the that model is better, and less likely to overfit the data.
    * i.e. the in the case that the test MSE is the same of the two models the one that has the lowest train MSE should be used.

Oscam's Razor --> Among competing hypothesis, the one with the fewest assumptions should be selected.

As an alternative to remove predictors (p) we can fit a model containing all predictors using a technique that constrains or regularizes the coefficient estimates, or equivalently, that shrinks the coefficient estimates towards zero.

Regularization is a way to bound the coefficients in a case when there is high collinearity between certain variables. It is a way to add a second term to the model that penalises for this collinearity. There are two ways that this can be done:

 It may not be immediately obvious why such a constraint should improve the fit, but it turns out that shrinking the coefficient estimates can significantly reduce their variance. The two best-known techniques for shrinking the regression coefficients towards zero are ridge regression and the lasso.

### Ridge Regression:
  * __Shrinks the value of coefficients but they reach zero so can't be used as a feature selection tool__
  * Ridge regression’s advantage over least squares is rooted in the bias-variance trade-off. As λ increases, the flexibility of the ridge regression fit decreases, leading to decreased variance but increased bias.
  * Ridge regression does have one obvious disadvantage. Unlike best subset, forward stepwise, and backward stepwise selection, which will generally select models that involve just a subset of the variables, ridge regression will include all p predictors in the final model.
  * Coding
  ```
  from sklearn import linear_model
  rlm = linear_model.Ridge(alpha=4, normalize=True) #where alpha is the same as Lambda in wikipedia or notebooks

  # You can also use RidgeCV function which automatically tries different values of alpha
  linear_model.RidgeCV(normalize=True)

  # You can also try different Alphas
  linear_model.RidgeCV(alphas=[0.1, 1, 10]) # Try different alphas
  ```
  * [Example on Scikit learn](http://scikit-learn.org/stable/auto_examples/linear_model/plot_ols_ridge_variance.html)


### Lasso Regression:
  * __Can be used as a feature selection tool as coefficients can got to zero__
  * Lasso came later but is now the most common one that is used.
  * Coding
  ```
  from sklearn import linear_model
  lacv = linear_model.LassoCV(normalize=True)
  ```
  * **Note**: Since Lasso tries to constrain the size of parameters, it's necessary to scale your data. You can normalize the data by passing `normalize=True` into `Lasso`, or by using the preprocessing methods we covered earlier


!(Ridge vs Lasso)[Lasso came later but is now the most common one that is used.]


## NOTE: Vector Magnitudes

* L2 Norm: The cartesian distance
  * sqrt( sum of x**2 )

* L1 Norm: Sum of the absolute values of the vectors
  * sum of abs(x)
