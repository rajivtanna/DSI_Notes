# Principal Component Analysis Overview

```
from sklearn.decomposition import PCA

# select n_components for the number of principal components you wish to use
pca = PCA(n_components=2)
pca.fit(df_stats_scaled)
```

Understanding the eigenvalues that are created by the process
```
sns.heatmap(pd.DataFrame(pca.components_.transpose(), index = df_stats.columns))
```


Plotting the covariance matrix
```
# We calculate the covariance matrix for all of our features, the transposing is a trivial
# step because the numpy function expects our features to be the rows and not the columns
cov_mat = np.cov(X_standardised.T)

# Here is a covariance plot / correlation plot
plt.pcolor(cov_mat, cmap=plt.cm.Blues)
plt.xlim([0,11])
plt.ylim([0,11])
plt.colorbar()
plt.show()
```

PCA is a very popular technique for performing dimensionality reduction. It is the most widely used general technique, which
is why it is wrapped up in this presentation with the whole concept of dimensionality reduction itself.
As we have said, dimensionality reduction is the process of combining or collapsing your existing features (columns in X)
into new features that not only retain the original information but also ideally reduce noise. PCA finds a linear combinations of your current predictor variables that will create new "principal components" to explain the maximum amount of variance in your predictors. Intuitively, PCA transforms the coordinate system so that the axes become the  most concise, informative descriptors of our data as a whole. The new axes are the principal components.

### The Process of PCA

Say we have a matrix $X$ of predictor variables. PCA will give us the ability to transform our $X$ matrix into a new matrix $Z$. First we will derive a weighting matrix $W$ from the correlational/covariance structure of $X$ that allows us to perform the transformation. Each successive dimension (column) in $Z$ will be rank-ordered according to the variance in its values.

**There are 3 assumptions that PCA makes:**
1. Linearity: Our data does not hold nonlinear relationships.
2. Large variances define importance: our dimensions are constructed to maximize remaining variance.
3. Principal components are orthogonal: each component (columns of $Z$) is completely un-correlated with the others.

**EIGENVECTORS:** An eigenvector specifies a direction with respect to the original coordinate space. The eigenvector with the highest corresponding eigenvalue is the first principal component.

**EIGENVALUES:** Eigenvalues indicate the amount of variance in the direction of the corresponding eigenvector.

Every eigenvector has a corresponding eigenvalue.


### Principal Components

What is a principal component? Principal components are the vectors that define the new coordinate system for your data. Transforming your original data columns onto the principal component axes constructs new variables that are optimized to explain as much variance as possible, and to be independent (uncorrelated). Creating these variables is a well-defined mathematical process, but in essence each component is created as a weighted sum (linear combination) of your original columns, such that all components are orthogonal (perpendicular) to each other.

[Try this out at setosa.io – it's a nice way to explore the intuition](http://setosa.io/ev/principal-component-analysis/)

### Why would we want to do PCA?

- We can reduce the number of dimensions (remove bottom number of components) whilst losing the least possible amount of variance information in our data.
- Since we are assuming our variables are interrelated (at least in the sense that they together explain a dependent variable), the information of interest should exist along directions with largest variance.
- The directions of largest variance should have the highest signal to noise ratio (SNR).
- Correlated predictor variables (also referred to as "redundancy" of information) are combined into independent variables. Our predictors from PCA are guaranteed to be independent, as part of their definition they are orthogonal.

[Paper explaining some more detail on PCA](./assets/papers/PCA-Tutorial-Intuition_jp.pdf)




# Dimensionality Reduction or Feature Elimination

- create new features based on some highly correlated features in the base data

## When to use dimensionality reduction
- To search for patterns in the data, to find the latent features that describe underlying correlations
- Visualising high dimensional data (particularly useful for visualising clustering)
- To reduce noise, by fitting to the signal and discarding other components
- To reduce computational time and/or optimise algorithm fitting from high dimensional data inputs

## Singular Value Decomposition (SUD)
- Any single matrix can be decomposed into three separate sub matrices.

## Singular Value Decomposition (SVD)

The formulation to find the eigenvectors and eigenvalues can implement a solution involving an eigendecomposition of the covariance matrix (which is what we will see below), or alternatively it can be found via a singular value
decomposition. Which is used will depend on the computational complexity of either approach (sklearn generally uses an automated method to assess which of the two approaches will be faster). Again, we will not go much into the details
of this but a singular value decomposition of the matrix $X$ would take the form:

$X$ = $U \Sigma V ^ T$

Where $\Sigma$ is a diagonal matrix (containing values only on the diagonal), which contain the eigenvalues (actually the square roots of the eigenvalues) of the covariance matrix of $X$. $U$ contains the left singular vectors (which we do not take) and $V$ the right singular vectors (which are the eigenvectors). Note the SVD has many applications and so its solution is highly optimised computationally via several different algorithmic implementations. It was also used to solve the ordinary least squares regression under the hood. If you are interested I would again refer you to _Linear Algebra and its Applications_.

Ultimately, **this simply means that any matrix satisfying certain conditions can be factorised into three separate matrices**. You do not particularly need to know the details of these things I am mentioning them so you have some idea what the terms mean when you come across them.
