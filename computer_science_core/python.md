## Python

ternory operator
```
a if condition else b
```

# Useful snippets of code

# Best way to remove punctuation in Python3
```
import string

def normalize(s):
    translator = str.maketrans({key: None for key in string.punctuation})
    s = s.lower()
    out = s.translate(translator)
    return out

def normalize_values(s):
    translator = str.maketrans({key: None for key in string.punctuation})
    s = s.lower()
    out = s.translate(translator)
    try:
        out = int(out)
    except Exception:
        out = 0
    return out
```

```
# you might find this package useful, see what the example does
import collections
labels = ['b','b','d','e','e']
print collections.Counter(labels), collections.Counter(labels).most_common(1)[0][0]
```
