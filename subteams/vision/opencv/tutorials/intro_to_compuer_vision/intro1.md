---
permalink: /vision/opencv/intro1/
---

# Intro to Computer Vision Part 1

[Back to OpenCV](/docs/vision/opencv)

[Download Tutorial as IPython Notebook]()

[More info on IPython Notebooks]()

Need a refresher on NumPy? [Check out the Intro to NumPy tutorial.](/docs/vision/numpy/intro/)


## Computer Vision 101 with NumPy

In Python, all of the popular image libraries use Numpy arrays to store images. This is because arrays in Numpy has an array structure that allows for contiguous memory in Python. Numpy has optimized various matrix and other general mathematical operations. This makes Numpy arrays much faster than built-in Python Lists.

Numpy is super fast and powerful.

While OpenCV and similar libraries implement most of this functionality for you, it is important to know how they work at a lower level for debugging and writing your own functionality when necessary.

```python
## pip install numpy
import numpy as np

## pip install matplotlib
import matplotlib.image as mpimg  # mpimg.imread(path) to read in an image
import matplotlib.pyplot as plt  # plt.imshow(np.array) to show an image
```

```python
## Creating an image
img = np.zeros(shape=(8, 8))

plt.imshow(img, cmap='gray')
plt.show()
```

![image](https://raw.githubusercontent.com/MissouriMRR/docs/main/subteams/vision/opencv/tutorials/intro_to_compuer_vision/intro_1_images/__results___2_0.png)

