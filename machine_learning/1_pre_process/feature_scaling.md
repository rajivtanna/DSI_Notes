## Feature Scaling

* It helps to speed up the training of models and ensure that models to overestimate one feature or other.
* It is not as important in linear models

There are three main methods:
* __Standerdisation:__
  * F(x) = (x - mean) / std
  * The coefficients now indicate how many SD will Y change when X changes by one SD (not necessarily of a normal distribution)
  * The intercept become the expected value of Y when the predictor values are set to their mean
  * Code Example:

    ```
    # Manual
    xs = df[col]
    mean = np.mean(xs)
    std = np.std(xs)
    xs = [(x - mean) / std for x in xs]

    # Using sklearn
    from sklearn import preprocessing

    xs = preprocessing.scale(df[col])
    ```

* __Min-Max Scaling:__
  * F(x) =(x - min) / (max - min)
  * Some applications might require a particular range for their input data, most commonly in the [0,1] range (ex. Artifical Neural Networks)
  * Code Example:

    ```
    # Manual
    xs = df[col]
    xmin = np.min(xs)
    xmax = np.max(xs)
    xs = [(x - xmin) / (xmax - xmin) for x in xs]

    # Using sklearn
    from sklearn import preprocessing

    scaler = preprocessing.MinMaxScaler()
    xs = scaler.fit_transform(df[col])
    ```

* __Normalisation:__
  * F(x) = x / sum(x)
  * Can actually be done it two ways:
    * L1 - simply the sum of all x values

      ```
      # Manual
      xs = df[col]
      denom_xs = sum(xs)
      xs = xs / denom_ys

      * Using sklearn
      from sklearn import preprocessing

      xs = df[col]
      xs = preprocessing.normalize(xs, norm='l1')
      ```

    * L2 - or can be the sum of euclidean sum of squares distance --> sqrt(sum(x**2))

      ```
      # Manual
      xs = df[col]
      denom_xs = np.sqrt(sum([x**2 for x in xs]))
      xs = xs / denom_ys

      * Using sklearn
      from sklearn import preprocessing

      xs = df[col]
      xs = preprocessing.normalize(xs, norm='l2')
      # note l2 is the default so don't have to call this.
      ```
