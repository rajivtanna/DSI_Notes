# Confusion Matrix

In the field of machine learning and specifically the problem of statistical classification, a confusion matrix, also known as an error matrix,[4] is a specific table layout that allows visualization of the performance of an algorithm, typically a supervised learning one (in unsupervised learning it is usually called a matching matrix).

![Confusion Matrix](assets/confusion_matrix.png)

__Accuracy__ - What % of the time is the model correct.

__F1 Score__ - In statistical analysis of binary classification, the F1 score (also F-score or F-measure) is a measure of a test's accuracy. It considers both the precision p and the recall r of the test to compute the score: p is the number of correct positive results divided by the number of all positive results, and r is the number of correct positive results divided by the number of positive results that should have been returned. The F1 score can be interpreted as a weighted average of the precision and recall, where an F1 score reaches its best value at 1 and worst at 0.

SciKit Learn Code Example
```
from sklearn.metrics import confusion_matrix

conmat = np.array(confusion_matrix(Y_test, Y_pred))
confusion = pd.DataFrame(conmat, index=['is_benign', 'is_malignant'],columns=['predicted_benign', 'predicted_malignant'])
confusion
```

__Useful Code to visualise the confusion matrix__
```
# As we know, this output of a confusion matrix is a bit awkward and confusing
# Here is some nice code to plot a confusion matrix in an easier to read way,
# this is from sklearn's documentation

import itertools
from sklearn.metrics import confusion_matrix

def plot_confusion_matrix(cm, classes, title='Confusion matrix', cmap=plt.cm.Blues):
    plt.imshow(cm, interpolation='nearest', cmap=cmap)
    plt.title(title)
    plt.colorbar()
    tick_marks = np.arange(len(classes))
    plt.xticks(tick_marks, classes, rotation=45)
    plt.yticks(tick_marks, classes)
    thresh = cm.max() / 2.
    for i, j in itertools.product(range(cm.shape[0]), range(cm.shape[1])):
        plt.text(j, i, cm[i, j],
                 horizontalalignment="center",
                 color="white" if cm[i, j] > thresh else "black")
    plt.ylabel('True label')
    plt.xlabel('Predicted label')
    plt.show()
    return
```

The difference between precision and recall can be tricky to interpret. The intuition here is
that precision is a measure of the QUALITY of results: of the houses labeled as over 200k,
how many of those were correct?

Recall, on the other hand, is a measure of the COMPLETENESS of results: of all the houses
that sold for over 200k, how many were identified by the classifier?

On the flip side, you can have a high precision but identify very few of the total 200k houses.
Likewise, you can have a high recall but also incorrectly identify many under 200k houses
as over 200k houses.

Say as a real estate agent, I prioritize minimizing false positives (predicting a house will sell for over 200k when it actually sells for under) because false positives make me lose money.
Change the decision threshold to lower the false positive rate and then print out the new confusion matrix. What is the downside to lowering the false positive rate?

* See [Wikipedia Articles](https://en.wikipedia.org/wiki/Confusion_matrix)
