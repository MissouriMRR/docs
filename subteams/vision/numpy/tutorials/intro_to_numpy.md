---
permalink: /vision/numpy/intro/
---

# Intro to NumPy

## NumPy 101

[NumPy Documentation](https://docs.scipy.org/doc/)

[NumPy Book](https://docs.scipy.org/doc/\_static/numpybook.pdf)

The main use of NumPy for Python developers is to get access to contiguous memory arrays, just like the ones commonly used in C++. NumPy is far more popular than other libraries with this same data structure, largely because of its broadcasting feature, which allows operations acting on multiple arrays of different sizes to work without any special functions.

The NumPy interpreter is also significantly faster than the Python interpreter for many tasks. For example, if you had a python list with values 1 to 1 million, and you used a list comprehension to add 1 to each of the values, it takes .07 seconds on my PC. On the other hand, if you have a NumPy array with values 1 to 1 million, using broadcasting to add one to each value only takes .008 seconds. Nearly a 10x speed increase. This makes NumPy extremely useful for working with large amounts of data, including images.

```
### Import Numpy
# To install on your local machine, open a terminal and type:
# Windows/Mac:
# python -m pip install numpy
# Linux:
# python3 -m pip install numpy
import numpy as np
```

Input:
```
### Creating an ndarray
## Can pass any iterable (array-like) into np.array() to transform it to an ndarray
## Numpy arrays are statically typed, unlike python lists!
x = np.array(['a', 'b', 'c'])
print(x)
# array(['a', 'b', 'c'], dtype='<U1')
```
Output:
```
['a' 'b' 'c']
```

