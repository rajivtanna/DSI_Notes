
# Measuring how good the model is

## Cophenetic Distance

The height of the dendogram where the two tranches that include the two observations merge into a single branch.

## Cophenetic coefficient

It's a measure of how well the  dendogram, preserves the pair-wise distances between the original observations.

Correlation  ( euclidean distance, cophenetic distance)

Code Examples
```

# Perform clustering
Z = linkage(dfm, 'ward')

# Calculating cophenetic correlation coefficient
c, coph_dists = cophenet(Z, pdist(dfm))
print c

# Plot the diagram
plt.title('Dendrogram')
plt.xlabel('Index Numbers')
plt.ylabel('Distance')
dendrogram(
    Z,
    leaf_rotation=90.,  
    leaf_font_size=8.,
)
plt.show()

# Getting the labels
max_d = 15
clusters = fcluster(Z, max_d, criterion='distance')
clusters

```
