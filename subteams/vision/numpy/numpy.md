---
permalink: /vision/numpy/
---

# NumPy

[Back to Vision Docs](/docs/vision/)

NumPy is a Python library that provides contiguous memory arrays. That means that each element of the array is stored next to each other in system memory. These are called `ndarray`s. These arrays are considered "C-Style" since they are similar to arrays in C/C++.

Python's `List` type does store elements contiguously. Elements can only be accessed by first finding the previous element. (These are similar to linked lists if you've taken Data Structures. See relevant [Stackoverflow](https://stackoverflow.com/questions/3917574/how-is-pythons-list-implemented).)

`ndarray`s are much faster than lists in many different ways. Accessing elements is much quicker. Additionally, operations can be performed on these arrays that take much longer in standard Python.

\
**As an example:**

```python
# Pure Python code
arr = list(range(10000000))
for i in range(10000000):
    arr[i] += arr[i]
```

```python
# Equivalent Python code using NumPy
arr = np.arange(10000000)
arr += arr
```

Not only is the NumPy code shorter and easier to understand, it takes much less time. On my computer, the pure Python code took 1.02 seconds to execute. Doing the same operation with NumPy took only 0.02 seconds (51 times faster!).

`ndarray`s are so efficient that many other popular Python libraries use them such as OpenCV, SciPy, and Matplotlib.

\
To learn more, check out the tutorials and projects below. If you're just getting started, first check out the [Intro to NumPy](/docs/vision/numpy/intro/) tutorial, then try out some of the problems and projects.

## Tutorials

- [Intro to NumPy](/docs/vision/numpy/intro/)

## Projects and Problems

- [Monte Carlo Simulation](/docs/vision/numpy/monte_carlo/)

