# Bayesian Statistics

Bayesian statistics, named for Thomas Bayes (1701–1761), is a theory in the field of statistics in which the evidence about the true state of the world is expressed in terms of degrees of belief known as Bayesian probabilities. Such an interpretation is only one of a number of interpretations of probability and there are other statistical techniques that are not based on 'degrees of belief'. One of the key ideas of Bayesian statistics is that "probability is orderly opinion, and that inference from data is nothing other than the revision of such opinion in the light of relevant new information."  

*Source: [Wikipedia](https://en.wikipedia.org/wiki/Bayesian_statistics)*

Useful link: http://www.kevinboone.net/bayes.html

## Additional learning
The guide _Bayesian Methods for Hackers_ has become a widely referenced resource amongst data scientists in the tech startup community, do take a look [here](http://nbviewer.jupyter.org/github/CamDavidsonPilon/Probabilistic-Programming-and-Bayesian-Methods-for-Hackers/blob/master/Chapter1_Introduction/Ch1_Introduction_PyMC3.ipynb). Another widely referenced guide is John Kruschke's book _Doing Bayesian Data Analysis_ [here](http://amzn.eu/5LLFC3W) (known as "the puppy book"). Note the reference in this book to Stan, a Python implementation of which - PyStan, see docs [here](https://pystan.readthedocs.io/en/latest/) - never really took off in popularity compared to its R implementation and has largely now been superseded by pymc.


## Bayes Breakdown

![bayes_breakdown](https://whenthenailsticksout.files.wordpress.com/2015/11/bayes_theorem_1.png?w=547)

### Applying Bayes in supervised machine learning

We can use this for classification problems; your hypothesis is hence the particular classification. Its canonical use case is text classification, where it works very well. The central difference for a Naive Bayes classification model is that it treats all predictors as conditionally independent. This makes its computation very fast, though of course not particularly realistic. For this reason it is often regarded as a base classifier against which others are judged for their ability to beat the Naive Bayes. What is surprising in the particular case of text classification is that Naive Bayes often actually outperforms models such as SVM whilst being computationally much faster too. This is hence the case where it is most often actually used in production, and in other scenarios it is more of interest in comparisons of multiple models as a baseline.

Given X contains n boolean attributes, our formula would appear for some particular case as:

$$P(\;Y=y_i\;|\;X=x_k\;) = \frac{P(\;X=x_k\;|\;Y=Y_i\;)P(\;Y=y_i\;)}{\Sigma_j\;P(\;X=x_k\;|\;Y=Y_i\;)P(\;Y=y_i\;)}$$

It should be stated that Naive Bayes is not the only possible Bayesian classifier; one could construct a Bayesian approach that did not rest on the independence assumption but this would be computationally intractable. Critically, given independence then our probability across all $X$ given $Y$ becomes a product for each $X_i$

 $$P(\;X_1...X_n\;|\;Y\;) = \prod_i P(\;X_i\;|\;Y\;) $$

Such that the fundamental equation for the Naive Bayes classifier becomes:
### $$P(\;Y=y_k\;|\;X_1...X_n\;) = \frac{P(\;Y=y_k\;) \prod_i P(\;X_i\;|\;Y=Y_k\;)}{\Sigma_j\;P(\;Y=y_j\;) \prod_i P(\;X_i\;|\;Y=y_j\;)}$$

Given some new unseen instance $X\;=\;<X_1...X_n>$, this equation shows us how to calculate the probability that $Y$ will take on the given value $Y\;=\;y_k$. In particular, we can simplify even further to return the most probable value of $Y$:

### $$Y \leftarrow argmax\; P(\;Y=y_k\;) \prod_i P(\;X_i\;|\;Y=Y_k\;) $$

We can estimate parameters with maximum likelihood by taking relative frequency counts for each case in the training dataset.

### Moving toward a production implementation

Possible issues to contend with:

- [Underflow](http://stackoverflow.com/questions/3704570/in-python-small-floats-tending-to-zero). Probabilites may very very small, too small for floating point arithmetic. We can solve by leveraging:

$$log(ab) = log\ a + log\ b$$

$$exp(log\ x) = x$$

So $P_1\ *\ P_2\ ...\ *\ P_2 = exp(log\ P_1 + ... + log\ P_n)$


- '0' probabilities. What if you never saw a feature value in your training data? We can use smoothing:

$$\hat\theta_i= \frac{x_i + \alpha}{N + \alpha d}  \qquad (i=1,\ldots,d)$$

- Continuous features. This brings us to *distributions*.

### The likelihood functions

Bayesians tend to talk in terms of distributions of belief. Rather than point estimates of probabilities, we can use distributions.

- For a binary event, probability can be modeled with the **binomial distribution**.
- For > 2 discrete outcomes, the **multinomial distribution**.
- And if features are continuous? **Gaussian**.

NOTE: Mutinomial - The multinomial Naive Bayes classifier is suitable for classification with discrete features (e.g., word counts for text classification). The multinomial distribution normally requires integer feature counts. However, in practice, fractional counts such as tf-idf may also work.

```
from sklearn import naive_bayes
  nb=naive_bayes.BernoulliNB()
  nb=naive_bayes.MultinomialNB()

from sklearn.naive_bayes import GaussianNB
```

##Priors and Conjugate Priors

| Prior  | Likelihood  |
|---|---|
 | Beta  | Binomial  |
 | Dirichlet  | Multinomial   |
 | Gamma  | Gaussian |
 | Gaussian  | Gaussian |

 The above table provides the likelihood/prior conjugate prior combinations that ensure computational tractability for calculation of posterior distributions

 | Name  | Probability Density Function (PDF) | Use
|---|---|
| Beta  | $\frac{1}{B(\alpha, \beta)}x^{\alpha-1}(1-x)^{\beta-1}$ | More of a family of distributions, usually representing probabilties of proportions, thus it's great to use as our prior belief (very important in Bayesian analysis)|
| Dirichlet  |  $\begin{equation*}\frac{1}{B(\alpha, ,\beta)}\prod_{i = 1}^{n}x_i^{\alpha-1}\end{equation*}$  | Similar to Beta above, just extended over multinomials, often used in NLP and other text-mining processes  |
| Gamma  |$ x^{a-i}e^{-x}\frac{1}{\Gamma(a)}$ | This distribution represents waiting time between Poisson distributed events (remember c_i in the previous lecture finger exercise |
| Gaussian  | $\frac{1}{\sigma \sqrt(2\pi)}e^{\frac{-(x-\mu)^{2}}{2\sigma^2}}$ | This is just the normal distribution |
| Binomial  | $\binom{n}{k}\cdot p^kq^{n-k} $   | Gives you the probability of getting k "success" in n iterations/trials |
| Multinomial  | $\frac{N!}{\prod_{i=1}^n x_i!} \prod_{i=1}^n \theta_i^{x_i} $| Similar to binomial distribution, but generalized to include multiple "buckets" (X, Y, Z,...) instead of just two (1 or 0), represents how many times each bucket X, Y, Z,... etc was observed over n trials |
