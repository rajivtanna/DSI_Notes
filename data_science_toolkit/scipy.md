#### __Creating Random Variables__

```
stats.norm.rvs(size=100000,random_state=100)
```

* Can use the describe function to get stats on the sample
```
stats.describe(sample)
```

* Random number generation algorithm
```
num_1=67890
print(num_1)
def rand_algo(num):
    num_2_staging=num**2
    num_2_staging=str(num_2_staging)
    num_2=int(num_2_staging[3:8])
    return num_2
num_2=rand_algo(num_1)
print(num_2)
num_3=rand_algo(num_2)
print(num_3)
```

* NOTE - you can also use numpy random function
```
variables are rows and columns
np.random.randn(10,4)
```
