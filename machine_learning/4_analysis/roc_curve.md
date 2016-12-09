# ROC Curve

The ROC curve compares the true positive rate against the false positive rate. It is unaffected by the distribution
of class labels since it is only comparing the correct vs. incorrect label assignments for one class.

To plot this we will use the roc_curve() and auc() functions from sklearn. We will also use the decision_function()
method of the logistic regression, which essentially gives us the confidence of each observation being in one class or
another.


Code example using sklearn.metrics
```
from sklearn.metrics import roc_curve, auc

Y_score = logreg.decision_function(X_test)

FPR, TPR, THR = roc_curve(Y_test, Y_score)
ROC_AUC = auc(FPR, TPR)
```

__Boiler Plate code__
```
from sklearn.metrics import roc_curve, auc
import matplotlib.pyplot as plt
plt.style.use('seaborn-white')
%matplotlib inline

def ROC_curve(X, y, title):

    plt.style.use('seaborn-white')

    y_score = lm.decision_function(X)
    FPR, TPR, THR = roc_curve(y, y_score)
    ROC_AUC = auc(FPR, TPR)

    plt.plot(FPR, TPR, label='ROC curve (area = %0.2f)' % ROC_AUC, linewidth=4)
    plt.plot([0, 1], [0, 1], 'k--', linewidth=4)
    plt.xlim([0.0, 1.0])
    plt.ylim([0.0, 1.05])
    plt.xlabel('False Positive Rate (1-specificity)', fontsize=18)
    plt.ylabel('True Positive Rate (sensitivity)', fontsize=18)
    plt.title(title, fontsize=18)
    plt.legend(loc="lower right")
    plt.show()
```


Ok, so right now you're wondering what this is. This is a ROC curve, and don't
worry about the acronym it makes not a lot of sense. It's to do with how it was originally developed during WWII when testing RADAR (RAdio Detection And Ranging... you see, rubbish acronyms) and trying to determine if they could tell the difference between planes
and flocks of birds (that's what you call a false positive!). So they unhelpfully called it Receiver Operating Characteristic.

The main takeaway is that the bigger
the area under the curve (measured from 0 to 1) the better the classifier. If the area was 0.5 the classifier would follow the
dotted diagonal and be no better than guessing. So we want the line to hug the top left hand corner - so
this one looks good!

But how did we calculate this? What was that code in the cell above? The main thing to understand is that we vary the threshold across a range of possible values to produce this curve, and then visualise what the true positive and false positive rates would be under these thresholds (however the threshold is defined differently from our hack previously, but we don't need to worry too much about the details).

The roc_curve function from sklearn returns three arrays:
first is the true positive rate, second is the false positive rate, and third is the thresholds on the
decision function (these are actually defined differently from the thresholds we were talking about in being 0-1 on probability, but same effect).

We will not go much more into the mechanics of the ROC curve but to say that each point is a different
model essentially with different decision boundaries, so generally we would select the one with the most
optimal characteristics for our situation (i.e. balancing false positives and false negatives), which generally
means the one closest to the top left. Since this model is so good, this is a bit difficult to see so we will look at another.

The ROC curve can a pretty good one to show non-technical people since you can explain that a classifier which follows the diagonal line is just chance, so anything above that is good - this is nice and visual and clear to see.




See Wikipedia link - https://en.wikipedia.org/wiki/Receiver_operating_characteristic
