# Linear Discriminant analysis

```
from sklearn.discriminant_analysis import LinearDiscriminantAnalysis as LDA

lda_classifier = LDA(n_components=2)
lda_x_axis = lda_classifier.fit(Xt, yt).transform(Xt)

lda_df = pd.DataFrame(lda_x_axis, columns=['LDA1','LDA2'])
lda_df['target'] = yt
lda_df.head()

# Graphing

color_scheme = ['r', 'g', 'b', 'y']

for c, i, target_name in zip(color_scheme, [0, 1, 2, 3], target_names):
    plt.scatter(lda_df.loc[pca_df['target'] == i, 'LDA1'], lda_df.loc[pca_df['target'] == i, 'LDA2'], c = c, label = target_names)

plt.xlabel('First LDA'); plt.ylabel('Second LDA')
plt.show()

```

LDA works ll when the discriminatory information is in the mean.

[Wikipedia Article](https://en.wikipedia.org/wiki/Discriminant_function_analysis)
