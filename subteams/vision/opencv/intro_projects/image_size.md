---
permalink: /vision/opencv/image_size/
---

# Change Image Size

[https://en.wikipedia.org/wiki/Interpolation](https://en.wikipedia.org/wiki/Interpolation)

Write a function that when passed a scaling factor, the function will resize the image accordingly. For example, if passed 2, the function would resize the image to 2x the original dimensions. When passed 0.5, the function would resize the image to 0.5x the original dimensions.
You can shrink an image through selective column dropping. You can grow an image using interpolation (estimation). My solution uses a simple interpolation to do both growing and shrinking.


[My Solution](https://github.com/fallscameron01/Resize_Image/blob/master/resize.py)

## Starting Image

![image](https://raw.githubusercontent.com/MissouriMRR/docs/main/subteams/vision/opencv/intro_projects/images/image_size/cat.jpg)

[Download Image](https://raw.githubusercontent.com/MissouriMRR/docs/main/subteams/vision/opencv/intro_projects/images/image_size/cat.jpg)

## Example Solutions

Generated Images of Various Sizes

![image](https://raw.githubusercontent.com/MissouriMRR/docs/main/subteams/vision/opencv/intro_projects/images/image_size/big_cat.jpg)


![image](https://raw.githubusercontent.com/MissouriMRR/docs/main/subteams/vision/opencv/intro_projects/images/image_size/small_cat.jpg)

