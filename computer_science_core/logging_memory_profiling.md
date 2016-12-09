

# Logging
```
import logging
logging.basicConfig(filename='test.log', level=logging.INFO)
log = logging.getLogger(__name__)
```

# Memory Profiling
```
from memory_profiler import profile

@profile
# Add this in front of functions you want to profile
```

# Put it together
```
Put it together! If you use the syntax:
fp=open('memory_profiler.log','w+')
@profile(stream=fp)
def your_function():
  return something
```
