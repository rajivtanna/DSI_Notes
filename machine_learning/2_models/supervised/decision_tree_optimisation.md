# CODE EXAMPLE
```
from sklearn.preprocessing import LabelEncoder
y = LabelEncoder().fit_transform(df['acceptability'])
X = pd.get_dummies(df.drop('acceptability', axis=1))

from sklearn.cross_validation import cross_val_score, StratifiedKFold
from sklearn.tree import DecisionTreeClassifier

# Advanced Tree Classifiers
from sklearn.ensemble import RandomForestClassifier, ExtraTreesClassifier, BaggingClassifier

# Boosting Classifier
from sklearn.ensemble import AdaBoostClassifier, GradientBoostingClassifier

cv = StratifiedKFold(y, n_folds=3, shuffle=True, random_state=41)

# Decision Tree Classifier
dt = DecisionTreeClassifier(class_weight='balanced')
s = cross_val_score(dt, X, y, cv=cv, n_jobs=-1)
print "{} Score:\t{:0.3} ± {:0.3}".format("Decision Tree", s.mean().round(3), s.std().round(3))

# Random Forest Classifier
dtr = RandomForestClassifier(class_weight='balanced')
sr = cross_val_score(dtr, X, y, cv=cv, n_jobs=-1)
print "{} Score:\t{:0.3} ± {:0.3}".format("Random Forest", sr.mean().round(3), sr.std().round(3))

# Extra Tree Classifier
dte = ExtraTreesClassifier(class_weight='balanced')
se = cross_val_score(dte, X, y, cv=cv, n_jobs=-1)
print "{} Score:\t{:0.3} ± {:0.3}".format("Extra Tree Classifier", se.mean().round(3), se.std().round(3))

# Bagging Classifier
dtb = BaggingClassifier()
sb = cross_val_score(dtb, X, y, cv=cv)
print "{} Score:\t{:0.3} ± {:0.3}".format("Bagging Classifier", sb.mean().round(3), sb.std().round(3))
