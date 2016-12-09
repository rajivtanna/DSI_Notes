# Evaluating Model Fit

__We compare models by using various metrics:__
* __MSE = ((Sum of Squared Residuals) ** 2) / n__
  * __Best MSE will always the lowest MSE__
  * Also see notes on Bias Variance

* __r2 = 1 - (SSR / original Variance)__
  * __How much of the original Variance is captured by the model__
  * __The more variable you add the higher r2 --> so when you compare models the one with more features will always mean__
  * __So you cannot use to compare models of different complexity__

* __Adj. r2 = 1 - ((1 - r2)(n - 1)) / n - p - 1)__
  * __n = sample size, p = number of predictors__
  * __Adjust r2 for the number of dimensions__


NOTE: Overfitting - is the situation in which we could have a better MSE (test) by using a simpler model


## Regression Metrics and Loss Functions

**NOTE: mean squared error is the most commonly used. For example in SciKit learn this is used as the default for linear regressions**

Regarding loss functions, for a model of the form $y = f(x) + \epsilon$ with predictions $\hat{y}_i$ and true values $y_i$ we have:

* The sum of squared errors:
$$\text{SSE} = \sum_{i}{\left(\hat{y}_i - y_i \right)^2}$$

In this lesson we're going to dig deeper into loss functions and their applications. Different loss functions are useful in different scenarios and there are two very popular loss functions that are used in conjuction with regression. In this case they are sometimes referred to as _regression metrics_.

The first is the _mean squared error_ or _MSE_ and it is the mean of the squared errors. If we have $n$ regression points and their predictions, the [MSE](https://en.wikipedia.org/wiki/Mean_squared_error) is:

$$\text{MSE} = {\frac{\sum_{i}{\left(\hat{y}_i - y_i \right)^2}}{n}}$$

The second is the _mean absolute error_ or _MAE_, and it differs by use of an absolute value instead of a square. The [MAE](https://en.wikipedia.org/wiki/Average_absolute_deviation) is:

$$\text{MAE} = \frac{\sum_{i}{|\hat{y}_i - y_i |}}{n}$$

### Why have different regression metrics?

You might be thinking, _what's all the fuss about_? It turns out that there are lots of good reasons to use different loss functions. We'll consider the effects of outliers on these two metrics.

First let's try a very simplified statistics problem. Given a dataset, how can we summarize it with a single number? Do you know any ways?

This is equivalent to fitting a constant model to the data. It turns out that the _mean_ minimizes the MSE and the _median_ minimizes the MAE. By analogy, when fitting a model, MAE is more tolerant to outliers. In other words, the degree of error of an outlier has a large impact when using MSE versus the MAE. Since the choice of loss function affects model fit, it's important to consider how you want errors to impact your models.

**Summary**
* Use MAE when how far off an error is makes little difference
* Use MSE when more extreme errors should have a large impact

Finally, note that linear regressions with MAE instead of MSE are called _least absolute deviation_ regressions rather than _least squares_ regressions.

### Bonus: Modes

It turns out the _mode_ minimizes the sum:
$$\frac{\sum_{i}{|\hat{y}_i - y_i |^{0}}}{n}$$
where $0^0=0$ and $x^0=1$ otherwise. Can you see why?

### Measuring r2

So far we've used the sum of the squared errors as a measure of model fit, looking for models with smaller errors. In this lab we'll investigate a new mesure of model fit, the [coefficient of determination](https://en.wikipedia.org/wiki/Coefficient_of_determination) $r^2$, and see how it's influenced by outliers.

R-squared is defined in terms of a ratio of the variance of the data, $SS_{tot}$ and the sum of squared error of the residuals of the model fit $SS_{res}$. Let's assume that our model has the form

$$y_i = f(x_i) + e_i$$

For some model function $f$. The mean of the data targets is $\bar{y}$. We can write $r^2$ as:

$$ r^2  = 1 - \frac{SS_{res}}{SS_{tot}} = 1 - \frac{\sum_{i}{\left(y_i - f_i \right)^2}}{\sum_{i}{\left(y_i - \bar{y} \right)^2}}$$

### Understanding $r^2$

**NOTE: In a one variable case r2 is the square of the persons coefficient but this is not the case or multiple variables**

To help understand this measure, let's consider a few special cases.
* If our model is a perfect fit then the predictions of the model always match the true values, i.e. $y_i = f(x_i) = f_i$. This means that the squared error of the residuals is 0, so r^2 is:

$$ r^2  = 1 - \frac{SS_{res}}{SS_{tot}} =  1 - \frac{0}{SS_{tot}} = 1$$

* If our model always predicted the mean value, $y_i = \bar{y}$ for all data points, then the two sum of squares terms are equal:

$$ r^2  = 1 - \frac{SS_{res}}{SS_{tot}} =  1 - 1 = 0$$

This is not a very good model -- it's simply a constant prediction, and does not vary over the data points.

* Typically the better the model the larger the value of $r^2$, with $r^2=1$ being an exact fit.

**Check**: It is possible for $r^2$ to be negative, despite the name. How could that happen?

**Useful code snippet for computing the r2 for all elements in a dataframe. returns a dictionary:**
```
r_sq={}
for col2 in df:
   for col in df:
       if col==col2:
           pass
       else:
           model=smf.ols(str(col)+ ' ~ '+str(col2), df).fit()
           predictions=model.predict()
           if (col2,col) in r_sq.keys():
               pass
           else:
               r_sq[col,col2]=model.rsquared

print(max(r_sq.items(), key=lambda k: k[1]))
print(min(r_sq.items(), key=lambda k: k[1]))
r_sq
```

* Can also use numpy functions:
  * NOTE: the degrees below is how many powers the model can be adjusted for.
```
coef   = np.polyfit(xs, ys, deg=2) # builds the model
predictions = np.polyval(coef, xs) # creates the predicted values
```

### How to detect outliers

You might be thinking: how can we detect and exclude outliers? It turns out that this is a [hard question to answer](https://en.wikipedia.org/wiki/Outlier#Identifying_outliers) and is often subjective. There are some methods, such as [Dixon's Q test](https://en.wikipedia.org/wiki/Dixon's_Q_test) and many others. Always make visualizations of your data when possible and remove outliers as appropriate, making sure that you can justify your selections!

* Dixon's Q-Test - [wikipedia link](https://en.wikipedia.org/wiki/Dixon%27s_Q_test)

### Confounding Variables

Another important topic when it comes to goodness of fit is [confounding variables](https://en.wikipedia.org/wiki/Confounding). It's tempting to think of models as causal but as you have likely heard before, [correlation is not causation](https://en.wikipedia.org/wiki/Correlation_does_not_imply_causation). Similarly, a high $r^2$ doesn't necessarily mean that two quantities are related in a predictive or causal manner. There are a number of examples [here](http://blog.searchmetrics.com/us/2015/09/11/ranking-factors-infographic-correlation-vs-causality/), including a nice plot of a seemingly strong relationship between per capita cheese consumption and the number of people who died by becoming tangled in their bedsheets! There is a very nice [case study](http://ocw.jhsph.edu/courses/fundepiii/pdfs/lecture18.pdf) of bias and confounding in disease studies. It's worth your time to read through the slides.

The takeaway message is that you always need to check that your conclusions make sense rather than blindly interpretting statistical values. As a data scientist you will often present analyses to stakeholders and they will ask questions about the causes of the relationships you find and the logical basis of the models you fit.
