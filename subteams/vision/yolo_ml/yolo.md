---
permalink: /vision/yolo_ml/
---

# How to YOLO(v8)

[Back to Vision Docs](/docs/vision/)

YOLO (You Only Look Once) is a deep learning object detection algorithm made by the Ultralytics company. It uses a convolutional neural network ([CNN](https://en.wikipedia.org/wiki/Convolutional_neural_network)) to effectively identify objects based on their features.

## Dependencies

1. Python version team is using
   1. Currently 3.10.x
   2. Also need pip
2. Python Pip packages needed (name --- `pip-package-name` [*opt*])
   1. Numpy --- `numpy`
   2. OpenCV --- `opencv-python`
   3. Ultralytics (YOLOv8) --- `ultralytics`
   4. PyTorch --- `torch`
      1. PyTorch Vision --- `torchvision`
      2. PyTorch Audio --- `torchaudio` *opt?*
   5. SciPy --- `scipy` *opt*
   6. matplotlib --- `matplotlib` *opt*
   7. **NOTE**: if an importing error occurs saying that the `_lzma` package is missing, then you need to:
      1. run `$ sudo apt install lzma liblzma-dev`
      2. uninstall and reinstall python
         1. and redownload all needed packages
   8. Other packages approved/required for vision team and needed for completion of the task

## Training

### Datasets

To train a YOLO model, you need images to feed it.

A **LOT** of images.

Additionally, these images need to be *annotated*, annotations are additional files that correspond to images. These files contain data about relevant objects in the image.

Each different type of object that the model needs to identify needs to be represented as a class. The dataset to be used for training needs to include: the definition of all the classes, the images, and a description of which objects are in each image and where they are.

#### The file hierarchy

(this is not strict, but let's stick to one convention)

	your_project/
	|	
	├── datasets/
	|	|
	|	└── dataset_name/
	|		|
	|		├── train/
	|		|	|
	|		|	├── images/
	|		|	|	└── all_your_pics.jpg
	|		|	|
	|		|	└── labels/
	|		|		└── all_your_pics.txt (same names as image files)
	|		|
	|		└── valid/
	|			|
	|			├── images/
	|			|	└── different_pics.jpg
	|			|
	|			└── labels/
	|				└── different_pics.txt (same names as image files)
	|
	└── dataset_name.yaml

#### The files

##### Images

- Sets of pictures that contain some/all of the objects to identify
- Need to have varied pictures for best results

##### Labels (annotations)

- One file per image (must have same name as image file)
- One line in the file per object in the corresponding image

File format:
```
<object-class> <x-center> <y-center> <width> <height>
<object-class> <x-center> <y-center> <width> <height>
						  .
						  .
						  .
```

 - `object-class`
   - The index (0 - n) of the class in `dataset_name.yaml` (see below)
 - `x-center` , `y-center`
   - x and y coordinates of the center of a minimum upright bounding box around the object in the image
   - Normalized to [0, 1]
 - `width` , `height`
   - width and height of the minimum bounding box around the object in the image
   - Normalized to [0, 1]

#### YAML file (`dataset_name.yaml`)



## References

- **<https://learnopencv.com/train-yolov8-on-custom-dataset/>**
- **<https://docs.ultralytics.com/>**

