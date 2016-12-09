## Bias-Variance Trade Off

__WE WANT A MODEL THAT BOTH ACCURATELY CAPTURES THE REGULARITIES IN ITS TRAINING DATA, BUT ALSO GENERALISES WELL TO UNSEEN DATA.__

__WE WANT A MODEL THAT MINIMISES MSE FOR IN AND OUT OF SAMPLE DATA__
  - You will always have some irreducible error!

explained best by images:
![Bias-Variance Visually](http://i.stack.imgur.com/r7QFy.png)
* __BIAS__ - is the error in our model due to approximate a real life model (which maybe extremely complicated) by a much simpler model
  - We can't reduce it by using bigger Sample_size
  - Systematically over or underestimating the real world results
  - __HIGH BIAS --> UNDERFITTING THE DATA__ (there is still information in the data that is not being captured)
  - [Wikipedia link](https://en.wikipedia.org/wiki/Bias_of_an_estimator)
  - [Andrew Ng video](https://www.youtube.com/watch?v=g4XluwGYPaA)

* __VARIANCE__ - It is the amount by which our F would change if we estimated it by using a different training set
  - The less prescriptive we are the mode noise we will fit, the more "sensitive the model is to small fluctuations in the training set"
  - __HIGH VARIANCE --> OVERFITTING THE DATA__ - Our model is overfitting to the noise in our training data set.

* __How to deal with Bias Variance__

![Bias-Variance Visually](Bias_Variance - Andrew Ng.png)
![Bias-Variance Visually](http://scott.fortmann-roe.com/docs/docs/BiasVariance/biasvariance.png)

  1. Decrease Variance
    - reduce features and model complexity
    - more sample data (reflects the population better)

  2. Decrease Bias
    - Check the sample for sampling error
    - Use additional features
    - Try adding polynomial features

NOTE: If we have high multicollinearity (our independent variables are highly correlated) then variance will also be high and we will be overestimating the coefficients. 

* USEFUL ESSAY - [Understanding the Bias-Variance Tradeoff](http://scott.fortmann-roe.com/docs/BiasVariance.html)

### CALCULATING BIAS - VARIANCE

MSE = Variance +  Bias**2 + variance of the test error

Code Example:
```
# Example of two scales measuring weight when known weight is 150
import numpy as np
List1 = np.array([152.0,151.0,151.5,150.5,152.0])
List2 = np.array([145.0,155.0,154.0,146.0,150.0])

# Calculate Bias, Variance and STD for scale 1
resd = List1 - 150
print np.sum(resd) / len(resd)
print np.var(List1)
print np.std(List1)

# Calculate Bias, Variance and STD for scale 2
resd = List2 - 150
print np.sum(resd) / len(resd)
print np.var(List2)
print np.std(List2)
```

* Variance and Bias both are positive so their lowest value can be 0.
* Therefore the best the MSE can be is the variance of the test error
