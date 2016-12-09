

## Linear Regressions
From a technical point of view, a linear regression is a method for determining the coefficients of a liner combination of variables:

e.g. Y = a0.X0 + ... + an.Xn (Written this was to show that you can use this for multiple variables / vectors)

Coefficients should define a line that is close as possible to our data points.
  Closeness is usually defined as the sum of the residuals which is the distance of the line for the points squared -- Residual Sum of Squares (RSS) (model.mse_model)


### Types of models
* Ordinary mean squares - aims to minimise the MSE (smf.OLS - using stats model)
* Quantile reqression - aims to minimise the MAE (smf.quantreg - using stats model)


### Using Stats model Learn
[Linear Regression Models](http://statsmodels.sourceforge.net/devel/examples/#regression)

* Using ordinary least squares models (most basic model) - [Wikipedia Article](https://en.wikipedia.org/wiki/Ordinary_least_squares)
* Use the r-squared to understand the performance of the model and to compare between models.
* As default it will force the y-intercept to go through 0 -- see below the code snippet to add this

```
# This is the way to import the model
import statsmodels.api as sm

# Running the actual models
X = df["RM"]
X = sm.add_constant(X) # Use this to add a y-intercept constant  
y = targets["MEDV"]

# Note the difference in argument order
model = sm.OLS(y, X)
trained_model = model.fit()
predictions = trained_model.predict(X)

# Print out the statistics and mean squared errors
trained_model.summary()

# Printing MSE
print "MSE:", model.mse_model



# Using SciKit Learn
from sklearn.metrics import mean_squared_error, r2_score
```

### Using SciKit Learn

__Simple Linear Regression__
```
from sklearn import linear_model
lm = linear_model.LinearRegression() #Add (fit_intercept = False) to force through zero
from sklearn.metrics import r2_score, mean_squared_error, mean_absolute_error

model = lm.fit(X,y)
predictions = model.predict(X)

# model.score(X, y) <-- returns the r2 for the model.

R2 = r2_score(y,predictions)
MSE = mean_squared_error(y,predictions)
MAE = mean_absolute_error(y_test,y_pred)

print "R2: ", R2
print "MSE: ", MSE
print "MAE: ", MAE

print "Coefficient:", model.coef_[0]
print "Intercept:", model.intercept_
```

__Polynomial Linear Regression__
```
model = make_pipeline(PolynomialFeatures(degree), Ridge())
model = model.fit(X_train_1, y_train)
y_pred = model.predict(X_test_1)
```

### Plotting (really helpful to visualise findings)

__Using Matplotlib__

```
plt.scatter(predictions, y, s=30, c='r', marker='+', zorder=10)
plt.xlabel("Predicted Values using all dataframe variables")
plt.ylabel("Actual Values MEDV")
plt.show()

# Or with  a trend line
fig, ax = plt.subplots()
fit = np.polyfit(predictions, y, deg=1)
ax.plot(predictions, fit[0] * predictions + fit[1], color='blue')

ax.scatter(predictions, y, s=30, c='r', marker='+', zorder=10)
plt.xlabel
```
__Using Seaborn (with annotations)__

```
ax = plt.gca()
ax.annotate("R2 = {:.4f}".format(R2),
                xy=(.1, 0.9), xycoords=ax.transAxes)
ax.annotate("MSE = {:.4f}".format(MSE),
                xy=(.1, .84), xycoords=ax.transAxes)
ax.annotate("MAE = {:.4f}".format(MAE),
                xy=(.1, .78), xycoords=ax.transAxes)

sns.regplot(predictions, y, marker='+',ax=ax,fit_reg=False)
plt.xlabel("Predicted Values")
plt.ylabel("Actual Values")


# or with polynomial fit line:

ax = plt.gca()

sns.regplot(X_test_1, y_test, marker='+',fit_reg=False,ax=ax)

# Plot the polynomial fit on top
x_line = pd.DataFrame(np.arange(X_test_1.min(),X_test_1.max(),0.5))
y_line = model.predict(x_line)

line = plt.plot(x_line, y_line , marker='+')
plt.show()
```

### Using Patsy - [link to documentation](https://patsy.readthedocs.io/en/latest/)
* `Patsy` is a python package that makes preparing data a bit easier. It uses a special formula syntax to create the `X` and `y` matrices we use to fit our models with.
  * You can extract columns from a dataframe before applying to a models
  * You can also use Patsy to create dummy variables  

Examples
```
y, X = patsy.dmatrices("MEDV ~ RM + LSTAT", data=df)
y, X = patsy.dmatrices("MEDV ~ LSTAT + np.power(LSTAT,2)", data=df)

# Create dummy variables
from patsy import dmatrix, demo_data

data = demo_data("a", nlevels=4) # This is just demo data
print data # This is just demo data
dmatrix("a", data)

# Can also do this directly with stats model:
# NOTE: It is implicit that the formula has an intercept (ADD a -1 to the end of the formula)

import statsmodels.formula.api as smf

mod=smf.ols('y ~ x', data=df)
res=mod.fit()
res.summary()
```
