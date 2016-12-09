# Time Series Analysis

1 - Extract meaningful statistics
2 - Identify the underlying mechanisms
3 - Predict / Forecast future values

```
DataFrame.asfreq(freq, method=None, how=None, normalize=False)
```
* Convert TimeSeries to specified frequency.

method : {‘backfill’/’bfill’, ‘pad’/’ffill’}, default None
Method to use for filling holes in reindexed Series (note this does not fill NaNs that already were present):
‘pad’ / ‘ffill’: propagate last valid observation forward to next valid
‘backfill’ / ‘bfill’: use NEXT valid observation to fill

```
DataFrame.resample(rule, how=None, axis=0, fill_method=None, closed=None, label=None, convention='start', kind=None, loffset=None, limit=None, base=0, on=None, level=None)

```
* Convenience method for frequency conversion and resampling of time series. Object must have a datetime-like index (DatetimeIndex, PeriodIndex, or TimedeltaIndex), or pass datetime-like values to the on or level keyword.

## Useful functions

Rolling Means
```
close_price.rolling(window=7)
```

Resampling the Data
```
stock_dict['goog'].resample('M').mean()['Mean'].plot()
```

## Autocorrelations

Autocorrelation and the autocorrelation function (acf)
Unlike regular randomly sampled data, time series data has an ordering - in time. This provides extra information and implies additional analysis methods. Of course, there may not be any extra information if, for example, the system from which we sample would be steady-state. While in previous weeks, our analyses has been concerned with the correlation between two or more variables (height and weight, education and salary, etc.), in time series data, autocorrelation is in a sense a measure of how correlated a variable is with itself (or rather, itself at different points in time). We are hence interested in patterns or trends in the data.
Hence in stock market data the stock price at one point is correlated with the stock price of the point directly prior in time. In sales data, sales on a Saturday are correlated with sales on the next Saturday and the previous Saturday, as well as other days to greater or lesser extent. We might expect for example that in stock market data the autocorrelation will be strong from one moment to the next but weak as the time difference grows larger.
Below is the formula for the autocorrelation funtion (acf):
$\text{Given measurements } x_1, x_2, x_3 ... x_n \text{ at time points } t_1, t_2, t_3 ... t_n:$
$$acf = \frac{\sum_{t=k+1}^{n}\left(\;x_t - \bar{x}\;\right)\left(\;x_{t-k} - \bar{x}\;\right)}{\sum_{t=1}^n\left(\;x_t - \bar{x}\;\right)^2}$$
Remember comparisons to correlation and covariance for similar concepts. The autorcorrelation (acf) will lie between -1 and +1.


# ARIMA & Time Series Modeling

Train and Test set-up
* We can
