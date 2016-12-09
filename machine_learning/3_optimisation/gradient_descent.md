# Gradient Descent

# Gradients and Gradient Descent

The [gradient of a function](https://en.wikipedia.org/wiki/Gradient) is a multivariate derivate with a crucial property -- the gradient points in the direction of the greatest rate of increase of the function. Many physical processes can be modeled by gradient and gradient flows, such as the flow of water down a mountain and the movement of charged particles in electromagnetic potentials. Dealing with such problems is hence very well established.

__It is used in neural networks and support vector machines, amongst other algorithms. It is not generally used for linear regression because an exact solution can be calculated quite straightforwardly in that case (and this is what sklearn and statsmodels will implement for ordinary least squares, not a gradient descent method)__

As data scientists we use gradient descent to maximize or minimize various functions. For example, to find a good model fit we could attempt to minimize a loss function by following the gradient through many iterations in parameter-space. Let's take a close look.

If we want to minimize a multivariate function $f(\mathbf{a})$ -- typically a function of our parameters $\mathbf{a} = (a_1, \ldots, a_n)$ computed on our dataset -- we start with a guess $\mathbf{a}_1$ and compute the next step using the gradient, denoted by $\nabla f$:

$$ \mathbf{a}_2 = \mathbf{a}_1 - \lambda \nabla f(\mathbf{a}_1)$$

Note the differences in notation carefully -- bold face indicates a vector of parameters. The variable $\lambda$ is a parameter that controls the step size and is sometimes called the _learning rate_. Essentially we are taking a local linear approximation to our function, stepping a small bit in the direction of greatest change, and computing a new linear approximation to the function. We repeat the process until we converge to a minimum:

$$ \mathbf{a}_{n+1} = \mathbf{a}_n - \lambda \nabla f(\mathbf{a}_n)$$

This is the _gradient descent_ algorithm. It is used for a variety of machine learning models including some that you will learn about soon, such as support vector machines and neural networks.

To be clear gradient descent is not implemented for linear regression in the examples we have studied so far. This is because there are good linear algebra methods for solving the minimum of the loss function without needing to use an iterative process. Statsmodels and sklearn will use these linear algebra methods to find the exact solution in one step without iteration. These linear algebra methods are beyond the scope of this course but do have a look at the source code if you are curious and mathematically inclined. However other more complex machine learning processes (such as neural networks) will have loss functions that cannot be solved in this manner and require some iterative analytical process. Hence we learn about this now before diving into such algorithms from week 4 onwards.

As a final note it is actually also possible to implement a gradient descent method for linear regression in sklearn with SGDRegressor, which you might want to do if you had a very large dataset for example (the stochastic gradient descent in that implementation uses a subset of the training set in order to speed up the implementation of the algorithm). This would be unusual however.

![](https://upload.wikimedia.org/wikipedia/commons/7/79/Gradient_descent.png)

```
# implementation of gradient descent
def gradient_descent(gradient_func, x, l=0.1):
    vector = np.array(x)
    g = gradient_func(x)
    return vector - l * np.array(gradient(x))
```

__Implementation in Scikit Learn:__
  * [Documentation](http://scikit-learn.org/stable/modules/sgd.html)
  * [sklearn.linear_model.SGDClassifier](http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.SGDClassifier.html#sklearn.linear_model.SGDClassifier)

  * Example
  ```
  >>> import numpy as np
  >>> from sklearn import linear_model
  >>> X = np.array([[-1, -1], [-2, -1], [1, 1], [2, 1]])
  >>> Y = np.array([1, 1, 2, 2])
  >>> clf = linear_model.SGDClassifier()
  >>> clf.fit(X, Y)
  ...
  SGDClassifier(alpha=0.0001, average=False, class_weight=None, epsilon=0.1,
          eta0=0.0, fit_intercept=True, l1_ratio=0.15,
          learning_rate='optimal', loss='hinge', n_iter=5, n_jobs=1,
          penalty='l2', power_t=0.5, random_state=None, shuffle=True,
          verbose=0, warm_start=False)
  >>> print(clf.predict([[-0.8, -1]]))
  ```


Advantages:
* Relatively simple algorithm to code, many [packages available](http://scikit-learn.org/stable/modules/sgd.html)
* Efficient (linear complexity in the size of the training set)
* Works for a variety of models

Disadvantages:
* Only works for differentiable functions
* Can get stuck in local minimums (rather than global optimums)
* Can be sensitive to learning rate and scaling
* For smaller datasets other algorithms can outperform gradient descent

## Derivates

The derivative of a function measures the rate of change of the values of the function with respect to another quantity.

* Find the derivative of a function: http://www.wolframalpha.com

![tangent](https://upload.wikimedia.org/wikipedia/commons/0/0f/Tangent_to_a_curve.svg)

The gradient is a derivative for multi-variable functions that gives us the
direction in which the function changes most quickly. Not all functions have
derivatives at every point. For example, the absolute value function does not
have a well-defined derivative at the origin because there are many possible
tangent lines:

![absolute value](https://upload.wikimedia.org/wikipedia/commons/6/6b/Absolute_value.svg)

This is one reason why squared error is used as a loss function whenever
possible. Even though we've seen a scenario in which the least absolute
deviation model (LAD) produced a better fit, finding the LAD can be tricky
because of the lack of derivative at the origin. However many loss functions
do have derivatives at every point and we can use derivatives and gradients
to minimize loss functions, thereby finding bit fit models for a variety of
machine learning algorithms.

### Additional Materials
- [Derivatives on Khan Academy](https://www.khanacademy.org/math/differential-calculus/taking-derivatives/derivative_intro/v/calculus-derivatives-1)
- [Partial Derivates](https://www.khanacademy.org/math/differential-calculus/taking-derivatives/chain_rule/v/chain-rule-with-triple-composition)
- A [nice demo](https://spin.atomicobject.com/2014/06/24/gradient-descent-linear-regression/) of gradient descent for linear regression.
- [Quora Article](https://www.quora.com/How-do-I-know-if-gradient-descent-will-find-a-local-optimum-when-the-objective-is-not-convex)
