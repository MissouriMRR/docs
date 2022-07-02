---
permalink: /vision/numpy/intro/
---

# Intro to NumPy

[Back to NumPy](https://missourimrr.github.io/docs/vision/numpy/)

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

```
### Creating an ndarray
## Can pass any iterable (array-like) into np.array() to transform it to an ndarray
## Numpy arrays are statically typed, unlike python lists!
x = np.array(['a', 'b', 'c'])
print(x)
# array(['a', 'b', 'c'], dtype='<U1')
```
['a' 'b' 'c']

```
## Numpy version of range, generates an ndarray instead
x = np.arange(start=0, stop=3, step=1)
print(x)
# array([0, 1, 2])
```
[0 1 2]

```
## np.arange allows for non integer step sizes
x = np.arange(start=0, stop=1, step=.1)
print(x)
```
[0.  0.1 0.2 0.3 0.4 0.5 0.6 0.7 0.8 0.9]

```
## Many functions to create quick arrays exist
x = np.zeros(shape=6)
print(x)
```
[0. 0. 0. 0. 0. 0.]

```
## Generating n dimensional is usually done by passing a tuple
#  into an array generation function.
x = np.zeros(shape=(3, 5))
print(x)
```
\[[0. 0. 0. 0. 0.]
 [0. 0. 0. 0. 0.]
 [0. 0. 0. 0. 0.]]

```
## Numpy can generate large amounts of random numbers very quickly
x = np.random.uniform(0, 1, size=10)
print(x)
```
[0.93155876 0.29574992 0.68585759 0.67382667 0.93817691 0.49314389
 0.72856054 0.08583324 0.21892651 0.85923479]


## Reshaping ndarrays
```
### Reshaping ndarrays
x = np.arange(1, 13)
print(x)
# array([ 1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12])
```
[ 1  2  3  4  5  6  7  8  9 10 11 12]

```
## Reshape by passing tuple w/ (n_rows, n_cols)
y = x.reshape((3, 4))
print(y)
# array([[ 1,  2,  3,  4],
#        [ 5,  6,  7,  8],
#        [ 9, 10, 11, 12]])
```
\[[ 1  2  3  4]
 [ 5  6  7  8]
 [ 9 10 11 12]]

```
## Transpose matrix
z = y.T
print(z)
```
\[[ 1  5  9]
 [ 2  6 10]
 [ 3  7 11]
 [ 4  8 12]]

```
## When reshaping can use -1 as parameter, the -1 is 
# switched to the maximum value it can be to generate 
# a valid array
x = np.random.power(7, size=4)
print(x)

print(x.T)

print(x.reshape((-1, 1)))
```
[0.98993759 0.93097049 0.98429427 0.88478864]
[0.98993759 0.93097049 0.98429427 0.88478864]
\[[0.98993759]
 [0.93097049]
 [0.98429427]
 [0.88478864]]