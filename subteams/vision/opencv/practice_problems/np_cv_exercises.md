---
permalink: /vision/opencv/exercises/
---

# NumPy and OpenCV Exercises

## Getting Started

*If you haven't done this stuff to set up your environment, make sure to do so before getting started.*

### Installing Packages

**Linux:**

```
# pip install numpy
# python -m pip install numpy
# python3 -m pip install numpy
```

**Windows:**

```
# py -m pip install numpy
# py -m pip install scipy
# py -m pip install matplotlib
# py -m pip install opencv-python
```

### Importing Packages

```python
import numpy as np
import cv2
from matplotlib import pyplot as plt
from matplotlib import image as mpimg
from scipy.ndimage import convolve
```

### Load an Images

```python
img_name = mpimg.imread(“filename”)
```

### Show an Image

```python
plt.imshow(img_name)
plt.show()
```
or
```python
cv2.imshow("Window Name", image_var)
cv2.waitKey(0)
cv2.destroyAllWindows()
```

### Save an Image

```python
plt.imsave(img_name, “filename”)
```

## Exercises

### Exercise 1

Create and show/save a 10x10 spiral 1-bit image by modifying np.zeros() using only manual slicing operations (For help, see #3 of [Intro to Computer Vision 1](/docs/vision/opencv/intro1/)).

The completed image, when shown, should look like this:

![image]()

### Exercise 2

### Exercise 3

### Exercise 4

### Exercise 5

### Exercise 6
