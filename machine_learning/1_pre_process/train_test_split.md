# Train-Test-Split

In this lesson we'll discuss a method for avoid overfitting that is commonly referred to a the _train/test split_. This is a type of cross-validation in that we split the dataset into two subsets:
* a subset to train our model on, and
* a subset to test our model's predictions on

This serves two useful purposes:
* We prevent overfitting by not using all the data, and
* We have some remaining data to evaluate our model.

While it may seem like a relatively simple idea, there are some caveats to putting it into practice. For example, if you are not careful it is easy to take a non-random split. Suppose we have salary data on technical professionals that is composed 80% of data from the UK and 20% from Ireland and is sorted by country. If we split our data into 80% training data and 20% testing data we might inadvertantly select all the UK data to train and all the non-UK data to test. In this case we've still overfit on our data set despite our train-test split because we did not sufficiently randomise the data.

In a situation like this we can randomise our data sorting beforehand, or we can use a stratified split to ensure equal distribution of the underlying sets.

A particularly popular approach, and one that is most useful if data is relatively scarce, is to use _k-fold cross validation_, which is cross validation applied to more than two subsets. In particular, we partition our data into $k$ subsets and train on $k-1$ one of them, holding the last slice for testing. We then iterate this for each of the possible $k$ subsets. Then we report statistics that are combined from each subset. Note that we should still have a separate test dataset which is untouched for a final validation, but here we rather focus on the mechanics of the cross validation.

### Use sk learn cross validation to create train and test data
```
from sklearn.cross_validation import train_test_split

X_train, X_test, y_train, y_test = train_test_split(df, y, test_size=0.4)
print(X_train.shape, y_train.shape)
print(X_test.shape, y_test.shape)
```

### Can also use pandas function
```
df.sample(frac=0.1, replace=False)
```

### Can and should also use K fold cross validation
```
from sklearn.cross_validation import cross_val_score, cross_val_predict

# Perform 10-fold cross validation
scores = cross_val_score(model, df, y, cv=10, scoring="r2")
print("Cross-validated scores:", scores)
print("Mean of the cross-validation scores:",np.mean(scores))
# Make cross validated predictions
predictions = cross_val_predict(model, df, y, cv=10)
```

NOTE:
Use the stratify function to make sure you have the same split of your predicted Y in each of the splits.
