# Visualisations

## Tool Options

* Matplotlib
* Seaborn
* Pandas plotting
* Tableau
* [plot.ly](https://plot.ly/powerpoint-online/)
  * Can build presentations - https://formidable.com/open-source/spectacle/
  * Uses D3.js ?

* Intro to D3.js (https://square.github.io/intro-to-d3/parts-of-a-graph/)

Useful links
* http://flowingdata.com

* Plotting using a log scale
```
plt.yscale('log')
```

* Boiler plate code for a characters
```
fig = plt.figure(figsize=[20,12])
ax1 = fig.add_subplot(221)
ax2 = fig.add_subplot(222)
ax3 = fig.add_subplot(223)
ax4 = fig.add_subplot(224)
ax1.set_title('Random Distribution (randint)')
ax2.set_title('Binomial Distribution (binom)')
ax3.set_title('Pareto Distribution (pareto - alpha 2.5)')
ax4.set_title('Pareto Distribution (pareto - alpha 1.5)')
sns.distplot(randint_sample,bins=b,ax=ax1)
sns.distplot(binom_sample,bins=b,ax=ax2)
sns.distplot(pareto_25_sample,bins=b,ax=ax3)
sns.distplot(pareto_15_sample,bins=b,ax=ax4)
```

* Boiler plate code for pairplot with correlation statsmodels
```
# visualize the relationship between each of the features and the response using scatterplots
from scipy import stats
def corrfunc(x, y, **kws):
    r, _ = stats.pearsonr(x, y)
    ax = plt.gca()
    ax.annotate("r = {:.2f}".format(r),
                xy=(.1, .9), xycoords=ax.transAxes)
g = sns.pairplot(data=data)
g.map_lower(corrfunc)
```
* SEABORN HEATMAP
Really useful to visually see the correlation
```
sns.heatmap(df.corr())
```
