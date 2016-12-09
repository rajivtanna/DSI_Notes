# Missing Data

This lab will focus on handling missing data in a new way: leveraging some of the models you've learned to use.

In general this topic is more on the "art" side of the science/art spectrum, but there are some well-established ways to deal with and impute missing data, depending on what you want to accomplish in the end (increase the power, remove NaNs, impute with a numerical/label to prevent errors from your ML algorithms, etc.).

Our overall goal is to see that there can be a "functional relationship" between the "missingness" of the data, and features found in our data. By doing this, we can categorize the kind of "missingness" we are dealing with for a particular dataset.

## Types of "Missingness"

| Type  | Description  |
|---|---|
 | Missing Completely at Random  | This is basically the best scenario, all NaN, NA, or blanks are distributed totally at random can be safely omitted. As throwing out cases with missing data does not bias your inferences.  |
 | Missing at Random  | A more general assumption, missing at random, is that the probability a variable is missing depends only on available information. Thus, if sex, race, education, and age are recorded for all the people in the survey, then “earnings” is missing at random if the probability of nonresponse to this question depends only on these other, fully recorded variables. It is often reasonable to model this process as a logistic regression, where the outcome variable equals 1 for observed cases and 0 for missing.  When an outcome variable is missing at random, it is acceptable to exclude the missing cases (that is, to treat them as NA’s), as long as the regression controls for all the variables that affect the probability of missingness. Thus, any model for earnings would have to include predictors for ethnicity, to avoid nonresponse bias. |
 | Missingness that depends on unobserved predictors.  | "There is a data generating process that yields missing values". Basically, it means there is some "pattern" to the 'missingness'. If missingness is not at random, it must be explicitly modeled, or else you must accept some bias in your inferences. |
 |Missingness that depends on the missing value itself| A particularly dif- ficult situation arises when the probability of missingness depends on the (po- tentially missing) variable itself. For example, suppose that people with higher earnings are less likely to reveal them. In the extreme case (for example, all persons earning more than $100,000 refuse to respond), this is called censoring, but even the probabilistic case causes difficulty.|

## Introducing the Inclusion Indicator 

 As stated, the type of “missingness” we are most concerned about is the last row, "Missing not at Random". If there is a data generating process, this means we can model the “missingness” in our data set. If we can convincingly show that this model accounts for "most" (we're not being stringent statisticians, so that word will be left up to you to define) of the observable variation, we can be (relatively) well-at-ease that our "missingness" isn't functionally related to some features we don't have control/accounted/recorded in our data.

Before we move forward, we have to define the "inclusion indicator". We say I is an inclusion indicator if : $$\begin{array}{cc}
  I=\{ &
    \begin{array}{cc}
      1 & x: missing \\
      0 & x: \neg{missing} \\
    \end{array}
\end{array} $$
