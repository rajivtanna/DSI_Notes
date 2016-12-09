## Stats
#### Introduction
Use z-test for large sample sizes and t-test for small sample sizes

* z-test - is with respect to the normal distribution
  * know the population standard deviation and a mean

* t-test - is with respect to the T distribution
  * Allows you to understand how likely an outcome is vs. the population mean
  * Allows for a higher probability of more extreme values
  * don't know the population standard deviation
  * sample size is small (i.e. less than 30)

z-statistic or t-statistic -- how may standard deviations are we from the population mean?
p-value -- the probability of getting the sample mean vs. the population mean

[__USEFUL SITE__](stattrek.com)
* Other interesting sites
  * http://daniellakens.blogspot.co.uk/2015/01/always-use-welchs-t-test-instead-of.html
  * https://en.wikipedia.org/wiki/Welch%27s_t-test#Advantages_and_limitations


#### T-test
##### Calculating the statistic (manually)
t_statistic = (sample_mean - population_mean) / (std/ sqrt(number_of_sample))
  * t_statistic=(np.mean(one_sample_data)-population_mean)/((np.std(one_sample_data, ddof=1))/(np.sqrt(len(one_sample_data))))

p_value = t_survival_function * 2 (survival function tells us 1 - cdf)
  * (p_value = stats.distributions.t.sf(np.abs(t_statistic), (len(one_sample_data)-1) ** 2)

##### Using scipy
```
stats.ttest_lsamp(one_sample_data, population_mean)
```

Statistical Significance - should be used carefully
Accuracy - when results are closer to a known value
Precision - if you repeat your test several times do they have the same result

#### z-test
##### Calculating the statistic (manually)
z-statistic = (sample_mean - population_mean)/(population_std) / sqrt(number_of_sample))
  * z_statistic=(sample_mean - population_mean)/(population_std/(np.sqrt(sample_size)))
  * for a large enough sample the sample_std can be used instead of population_std

p_value = normal_survival_function (z_statistic) * 2
  * p_value=stats.norm.sf(np.abs(z_statistic)) * 2 -- for one sided otherwise do not times by 2

NOTE - Once you have the z-statistic there is a 1-2-1 relationship to the p-value

#### Errors
* Type 1 - reject null hypothesis when it's True
* Type 2 - accept null hypothesis with it's False

#### Code snippets
```
#t-test
one_sample_data = [177.3, 182.7, 169.6, 176.3, 180.3, 179.4, 178.5, 177.2, 181.8, 176.5]
population_mean=175.3

t_statistic=(np.mean(one_sample_data)-population_mean)/((np.std(one_sample_data, ddof=1))/(np.sqrt(len(one_sample_data))))
print(t_statistic)

p_value=stats.distributions.t.sf(np.abs(t_statistic), (len(one_sample_data)-1)) * 2
print(p_value)

# OR

one_sample = stats.ttest_1samp(one_sample_data, population_mean)

print("--")
print(one_sample)
print("--")
print("The t-statistic is %.3f and the p-value is %.3f." % one_sample)
print("--")

# z-test

population_mean=175.3
population_std=6.23
sample_size=100
sample_mean=175.8

z_statistic=(sample_mean-population_mean)/(population_sd/(np.sqrt(sample_size)))
print z_statistic

p_value=stats.norm.sf(np.abs(z_statistic)) * 2
print p_value
```

#### Welch's t-test
Used to compare two sample sets and see if their underlying population means are not different
So a low p-value

##### Calculating the statistic (manually)
t_statistic=(mean1 - mean2)/sqrt((sd1**2/n1)+(sd2**2/n2))
 * t_statistic=(np.mean(female_heights)-np.mean(male_heights))/np.sqrt((np.var(female_heights, ddof=1)/len(female_heights))+(np.var(male_heights, ddof=1))/len(male_heights))
 * p_value=stats.distributions.t.sf(np.abs(t_statistic), degrees_of_freedom) * 2
  * NOTE - Degrees of freedom needs to be calculated carefully


#### Code snippets
```
female_heights = [163.8, 156.4, 155.2, 158.5, 164.0, 151.6, 154.6, 171.0]
male_heights = [175.5, 183.9, 175.7, 172.5, 156.2, 173.4, 167.7, 187.9]

two_sample = stats.ttest_ind(male_heights, female_heights, equal_var=False)
print("The t-statistic is %.3f and the p-value is %.3f." % two_sample)

```

#### Useful Libraries
* Stats Models
