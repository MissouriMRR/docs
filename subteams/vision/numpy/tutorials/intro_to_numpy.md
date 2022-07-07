---
permalink: /vision/numpy/intro/
---

# Intro to NumPy

[Back to NumPy](/docs/vision/numpy/)

[Download Tutorial as IPython Notebook]()

[More info on IPython Notebooks]()

## NumPy 101

[NumPy Documentation](https://docs.scipy.org/doc/)

[NumPy Book](https://docs.scipy.org/doc/\_static/numpybook.pdf)

The main use of NumPy for Python developers is to get access to contiguous memory arrays, just like the ones commonly used in C++. NumPy is far more popular than other libraries with this same data structure, largely because of its broadcasting feature, which allows operations acting on multiple arrays of different sizes to work without any special functions.

The NumPy interpreter is also significantly faster than the Python interpreter for many tasks. For example, if you had a python list with values 1 to 1 million, and you used a list comprehension to add 1 to each of the values, it takes .07 seconds on my PC. On the other hand, if you have a NumPy array with values 1 to 1 million, using broadcasting to add one to each value only takes .008 seconds. Nearly a 10x speed increase. This makes NumPy extremely useful for working with large amounts of data, including images.

```python
### Import Numpy
# To install on your local machine, open a terminal and type:
# Windows/Mac:
# python -m pip install numpy
# Linux:
# python3 -m pip install numpy
import numpy as np
```

```python
### Creating an ndarray
## Can pass any iterable (array-like) into np.array() to transform it to an ndarray
## Numpy arrays are statically typed, unlike python lists!
x = np.array(['a', 'b', 'c'])
print(x)
# array(['a', 'b', 'c'], dtype='<U1')
```
\['a' 'b' 'c']

```python
## Numpy version of range, generates an ndarray instead
x = np.arange(start=0, stop=3, step=1)
print(x)
# array([0, 1, 2])
```
\[0 1 2]

```python
## np.arange allows for non integer step sizes
x = np.arange(start=0, stop=1, step=.1)
print(x)
```
\[0.  0.1 0.2 0.3 0.4 0.5 0.6 0.7 0.8 0.9]

```python
## Many functions to create quick arrays exist
x = np.zeros(shape=6)
print(x)
```
\[0. 0. 0. 0. 0. 0.]

```python
## Generating n dimensional is usually done by passing a tuple
#  into an array generation function.
x = np.zeros(shape=(3, 5))
print(x)
```
\[\[0. 0. 0. 0. 0.]\
 \[0. 0. 0. 0. 0.]\
 \[0. 0. 0. 0. 0.]]

```python
## Numpy can generate large amounts of random numbers very quickly
x = np.random.uniform(0, 1, size=10)
print(x)
```
\[0.93155876 0.29574992 0.68585759 0.67382667 0.93817691 0.49314389\
 0.72856054 0.08583324 0.21892651 0.85923479]


## Reshaping ndarrays

```python
### Reshaping ndarrays
x = np.arange(1, 13)
print(x)
# array([ 1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12])
```
\[ 1  2  3  4  5  6  7  8  9 10 11 12]

```python
## Reshape by passing tuple w/ (n_rows, n_cols)
y = x.reshape((3, 4))
print(y)
# array([[ 1,  2,  3,  4],
#        [ 5,  6,  7,  8],
#        [ 9, 10, 11, 12]])
```
\[\[ 1  2  3  4]\
 \[ 5  6  7  8]\
 \[ 9 10 11 12]]

```python
## Transpose matrix
z = y.T
print(z)
```
\[\[ 1  5  9]\
 \[ 2  6 10]\
 \[ 3  7 11]\
 \[ 4  8 12]]

```python
## When reshaping can use -1 as parameter, the -1 is 
# switched to the maximum value it can be to generate 
# a valid array
x = np.random.power(7, size=4)
print(x)

print(x.T)

print(x.reshape((-1, 1)))
```
\[0.98993759 0.93097049 0.98429427 0.88478864]\
\[0.98993759 0.93097049 0.98429427 0.88478864]\
\[\[0.98993759]\
 \[0.93097049]\
 \[0.98429427]\
 \[0.88478864]]


## Combining ndarrays

```python
### Combining ndarrays
a = np.arange(6)
b = a.reshape((2, -1))

print(a)
print(b)
```
\[0 1 2 3 4 5]\
\[\[0 1 2]\
 \[3 4 5]]

```python
## Concatenate essentially glues arrays together along axis
x = np.concatenate((b, b), axis=0)
print(x)
```
\[\[0 1 2]\
 \[3 4 5]\
 \[0 1 2]\
 \[3 4 5]]

```python
x = np.concatenate((b, b), axis=1)
print(x)
```
\[\[0 1 2 0 1 2]\
 \[3 4 5 3 4 5]]

```python
## vstack is an alias to concatenate: axis=0
x = np.vstack((b, b))
print(x)
```
\[\[0 1 2]\
 \[3 4 5]\
 \[0 1 2]\
 \[3 4 5]]

```python
## hstack is an alias to concatenate: axis=1
x = np.hstack((b, b))
print(x)
```
\[\[0 1 2 0 1 2]\
 \[3 4 5 3 4 5]]


## Indexing and Slicing ndarrays

```python
### Indexing & Slicing ndarrays
x = np.arange(12).reshape((3, 4))
print(x)

# [row, column, channel]
print(x[1, 2])
```
\[\[ 0  1  2  3]\
 \[ 4  5  6  7]\
 \[ 8  9 10 11]]\
6

```python
x = np.arange(12).reshape((2, 2, 3))
print(x)
# array([[[ 0,  1,  2],
#         [ 3,  4,  5]],
#
#        [[ 6,  7,  8],
#         [ 9, 10, 11]]])

# The third index is the channel. In an image, this would be used for RGB.
print(x[0, 1, 2])
```
\[\[\[ 0  1  2]\
  \[ 3  4  5]]\
\
 \[\[ 6  7  8]\
  \[ 9 10 11]]]\
5

```python
## Multiple indexing, passing a list instead of an int
# Note: values are returned as ndarray
x = np.arange(0, 18, 3)
print(x)

print(x[[1, 3, 5]])
```
\[ 0  3  6  9 12 15]\
\[ 3  9 15]

```python
## np.where generates indexing matrix based on what values are true
#  in given matrix
x = np.arange(10).reshape((2, -1))
print(x)

is_odd = x % 2
# array([[0, 1, 0, 1, 0],
#        [1, 0, 1, 0, 1]], dtype=int32)

locs = np.where(is_odd)
print(locs)
# (array([0, 0, 1, 1, 1], dtype=int64), array([1, 3, 0, 2, 4], dtype=int64))
print(x[locs])
# array([1, 3, 5, 7, 9])
```
\[\[0 1 2 3 4]\
 \[5 6 7 8 9]]\
(array(\[0, 0, 1, 1, 1]), array(\[1, 3, 0, 2, 4]))\
\[1 3 5 7 9]

```python
## Slicing, the same as pure python with multiple dimensions.
x = np.arange(5)

print(x)

print(x[::1])

print(x[::2])

print(x[::-1])
```
\[0 1 2 3 4]\
\[0 1 2 3 4]\
\[0 2 4]\
\[4 3 2 1 0]

```python
x = np.arange(5) + np.arange(5).reshape((-1, 1))
print(x)
print()

print(x[1:4])
print()

print(x[:, 1:4])
print()

print(x[1:4, 1:4])
```
\[\[0 1 2 3 4]\
 \[1 2 3 4 5]\
 \[2 3 4 5 6]\
 \[3 4 5 6 7]\
 \[4 5 6 7 8]]\
\
\[\[1 2 3 4 5]\
 \[2 3 4 5 6]\
 \[3 4 5 6 7]]\
\
\[\[1 2 3]\
 \[2 3 4]\
 \[3 4 5]\
 \[4 5 6]\
 \[5 6 7]]\
\
\[\[2 3 4]\
 \[3 4 5]\
 \[4 5 6]]


## Broadcasting

```python
### Broadcasting
## Broadcasting is numpy's way of performing operations between
#  arrays of different lengths

## Quickly multiplying array by scalar
x = np.ones(shape=3) * 6
print(x)
```
\[6. 6. 6.]

```python
## Adding vector a with shape (1, 3) and b with shape (3, 1) = 
#  x with shape (3, 3)
a = np.arange(3)
b = np.arange(3).reshape((-1, 1))
print(a)

print(b)

x = a + b
print(x)
```
\[0 1 2]\
\[\[0]\
 \[1]\
 \[2]]\
\[\[0 1 2]\
 \[1 2 3]\
 \[2 3 4]]

```python
## Longer demonstration
x = np.array([-3, 0, 3]) + np.zeros(shape=(3, 1))
print(x)
print()
# array([[-3.,  0.,  3.],
#        [-3.,  0.,  3.],
#        [-3.,  0.,  3.]])

y = np.array([-10, 0, 10])
print(y)
print()

print(x * y)
print()

y = y.reshape((-1, 1))
print(y)
print()

print(x * y)
```
\[\[-3.  0.  3.]\
 \[-3.  0.  3.]\
 \[-3.  0.  3.]]\
\
\[-10   0  10]\
\
\[\[30.  0. 30.]\
 \[30.  0. 30.]\
 \[30.  0. 30.]]\
\
\[\[-10]\
 \[  0]\
 \[ 10]]\
\
\[\[ 30.  -0. -30.]\
 \[ -0.   0.   0.]\
 \[-30.   0.  30.]]

```python
## Arithmetic, Comparisons and set = are broadcastable operations
# generating a crosshatch pattern
x = np.arange(5) + np.zeros(shape=(3, 1))
print(x)
# array([[0., 1., 2., 3., 4.],
#        [0., 1., 2., 3., 4.],
#        [0., 1., 2., 3., 4.]])

y = np.array([[1], [2], [3]])
print(y)
# array([[1],
#        [2],
#        [3]])
```
\[\[0. 1. 2. 3. 4.]\
 \[0. 1. 2. 3. 4.]\
 \[0. 1. 2. 3. 4.]]\
\[\[1]\
 \[2]\
 \[3]]

```python
z = x + y
print(z)
# array([[1., 2., 3., 4., 5.],
#        [2., 3., 4., 5., 6.],
#        [3., 4., 5., 6., 7.]])
```
\[\[1. 2. 3. 4. 5.]\
 \[2. 3. 4. 5. 6.]\
 \[3. 4. 5. 6. 7.]]

```python
# ~ inverts boolean values
idx = ~np.bool_(z % 2)
print(idx)
```
\[\[False  True False  True False]\
 \[ True False  True False  True]\
 \[False  True False  True False]]

```python
z[idx] = 0
print(z)
```
\[\[1. 0. 3. 0. 5.]\
 \[0. 3. 0. 5. 0.]\
 \[3. 0. 5. 0. 7.]]

```python
crosshatch = z / z
print(crosshatch)
# Note: nan means "not a number" and results from a divide by zero error
```
\[\[ 1. nan  1. nan  1.]\
 \[nan  1. nan  1. nan]\
 \[ 1. nan  1. nan  1.]]\
/opt/conda/lib/python3.6/site-packages/ipykernel_launcher.py:1: RuntimeWarning: invalid value encountered in true_divide\
  \"\"\"Entry point for launching an IPython kernel.

```python
# We can use np.isnan to create a mask for nan values
crosshatch[np.isnan(crosshatch)] = 0
print(crosshatch)
```
\[\[1. 0. 1. 0. 1.]\
 \[0. 1. 0. 1. 0.]\
 \[1. 0. 1. 0. 1.]]


## Memory Nuances

```python
### Memory Nuances
## Slicing and indexing values of ndarray return actual
## memory locations that can be used to modify original
a = np.arange(10).reshape((2, 5))
print(a)

b = a[::-1, ::-1]
print(b)

a[0] += 1
print(a)

print(b)
```
\[\[0 1 2 3 4]\
 \[5 6 7 8 9]]\
\[\[9 8 7 6 5]\
 \[4 3 2 1 0]]\
\[\[1 2 3 4 5]\
 \[5 6 7 8 9]]\
\[\[9 8 7 6 5]\
 \[5 4 3 2 1]]

```python
## np.copy is a quick way to remove this issue
a = np.arange(10).reshape((2, 5))
print(a)

b = np.copy(a)[::-1, ::-1]
print(b)

a[0] += 1
print(a)

print(b)
```
\[\[0 1 2 3 4]\
 \[5 6 7 8 9]]\
\[\[9 8 7 6 5]\
 \[4 3 2 1 0]]\
\[\[1 2 3 4 5]\
 \[5 6 7 8 9]]\
\[\[9 8 7 6 5]\
 \[4 3 2 1 0]]


## Statistics with Numpy

```python
### Numpy Statistics
x = np.random.normal(loc=12, size=10000)
print(np.mean(x))

x = np.random.randint(0, 100, size=100000)
print(np.median(x))
```
11.974745668190877\
50.0


## Masked Arrays

```python
### Masked Arrays
# data[any], mask[bool], fill_value[any] <- value given w/ array-array comparison 
# when location is masked
x = np.ma.array(np.arange(10), mask=[False] * 10)
print(x)
# masked_array(data=[0, 1, 2, 3, 4, 5, 6, 7, 8, 9],
#              mask=[False, False, False, False, False, False, False, False,
#                    False, False],
#        fill_value=999999)
```
\[0 1 2 3 4 5 6 7 8 9]

```python
x.mask[np.where(x % 2)] = True
print(x)
# masked_array(data=[0, --, 2, --, 4, --, 6, --, 8, --],
#              mask=[False,  True, False,  True, False,  True, False,  True,
#                    False,  True],
#        fill_value=999999)
```
\[0 -- 2 -- 4 -- 6 -- 8 --]

```python
print(x >= 0)
# masked_array(data=[True, --, True, --, True, --, True, --, True, --],
#              mask=[False,  True, False,  True, False,  True, False,  True,
#                    False,  True],
#        fill_value=999999)
```
\[True -- True -- True -- True -- True --]

```python
print(np.sum(x))
# 20
```
20


## General Functions

```python
### General Functions
# Using Pure Python
a = list(range(5))
print(a)

b = a[::-1]
print(b)

for i, value in enumerate(b):
    a[i] += value
print(a)
```
\[0, 1, 2, 3, 4]\
\[4, 3, 2, 1, 0]\
\[4, 4, 4, 4, 4]

```python
# Now using Numpy
a = np.arange(10).reshape((2, 5))
print(a)

b = np.copy(a)[::-1, ::-1]
print(b)

for idx, value in np.ndenumerate(b):  # n dimensional enumerate, idx=tuple
    a[idx] += value
print(a)
```
\[\[0 1 2 3 4]\
 \[5 6 7 8 9]]\
\[\[9 8 7 6 5]\
 \[4 3 2 1 0]]\
\[\[9 9 9 9 9]\
 \[9 9 9 9 9]]

```python
x = np.arange(12).reshape((2, 2, 3))
print(x)
# array([[[ 0,  1,  2],
#         [ 3,  4,  5]],

#        [[ 6,  7,  8],
#         [ 9, 10, 11]]])

print(np.ravel(x))
```
\[\[\[ 0  1  2]\
  \[ 3  4  5]]\
\
 \[\[ 6  7  8]\
  \[ 9 10 11]]]\
\[ 0  1  2  3  4  5  6  7  8  9 10 11]

```python
x = np.arange(7)
print(x)

y = np.minimum(x, x[::-1])
print(y)

print(np.argmax(y))  # -> index of max value
```
\[0 1 2 3 4 5 6]\
\[0 1 2 3 2 1 0]\
3

## Practice Problems and Projects

Now that you have an idea of some of the things that NumPy can do, check out these practice problems and projects.

- **Project: Monte Carlo Craps Simulation** [Check out the problem here](/docs/vision/numpy/monte_carlo/)
