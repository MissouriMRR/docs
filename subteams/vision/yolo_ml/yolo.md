# **How to YOLO(v8)**

## Dependencies

1. Python version team is using
   1. Currently 3.10.x
   2. Also need pip
2. Python Pip packages needed (name --- `pip package name` [*opt*])
   1. Numpy --- `numpy`
   2. OpenCV --- `opencv-python`
   3. PyTorch --- `torch`
      1. PyTorch Vision --- `torchvision`
      2. PyTorch Audio --- `torchaudio` *opt?*
   4. SciPy --- `scipy` *opt*
   5. matplotlib --- `matplotlib` *opt*
   6. Other packages approved/required for vision team and needed for completion of the task

## Training

### Datasets

To train a YOLO model, you need images to feed it.

A **LOT** of images.

Additionally, these images need to be *annotated*, annotations are additional files that correspond to images. These files contain data about relevant objects in the image.

Each different type of object that the model needs to identify needs to be represented as a class. The dataset to be used for training needs to include: the definition of all the classes, the images, and a description of which objects are in each image and where they are.

#### The file hierarchy

(this is not strict, but let's stick to convention)

	datasets/
	|
	├── dataset_name/
	|	|
	|	├── train/
	|	|	|
	|	|	├── images/
	|	|	|	└── all_your_pics.jpg
	|	|	|
	|	|	└── labels/
	|	|		└── all_your_pics.txt *(same names as image files)*
	|	|
	|	└── valid/
	|		|
	|		├── images/
	|		|	└── different_pics.jpg
	|		|
	|		└── labels/
	|			└── different_pics.txt *(same names as image files)*
	|
	└── dataset_name.yaml

#### The files

Images:
- Sets of pictures that contain some/all of the objects to identify
- Need to have varied angles/rotations/distances/etc.
  - Ensures that model will function in all plausible scenarios

Labels (annotations):
- One file per

## References

- **<https://learnopencv.com/train-yolov8-on-custom-dataset/>**
- **<https://docs.ultralytics.com/>**

## Credits

- **File author:** Nathan Mejia
