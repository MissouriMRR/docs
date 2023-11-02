---
permalink: /vision/yolo_ml/
---

# How to YOLO(v8)

[Back to Vision Docs](/docs/vision/)

YOLO (You Only Look Once) is a deep learning object detection algorithm family made by the Ultralytics company. It uses a convolutional neural network ([CNN](https://en.wikipedia.org/wiki/Convolutional_neural_network)) to effectively identify objects based on their features.

There are different versions of YOLO. This documentation is for YOLOv8. Additionally, YOLOv8 has multiple available sizes: yolov8n (Nano), yolov8s (Small), yolov8m (Medium), yolov8l (Large), and yolov8x (Extra Large). The smallest size is the least accurate but is the fastest to train and run. As size increases the models get more accurate, but they also get slower.

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

This is the file that specifies the location of the dataset, training data, validation data, and the classes of objects to will identify (with an index of the class)

```yaml
# the dataset YAML file
path: dataset_name/
train: 'train/images'
val: 'valid/images'

# class names
names:
	0: 'obj_class_0'
	1: 'obj_class_1'
	2: 'obj_class_2'
	.: 'obj_class_...'
	.: 'foo'
	.: 'obj_class_...'
	.: 'bar'
	.: 'obj_class_...'
	n: 'obj_class_n'
```

### Actually Training

With a dataset created, the next step is to train a model on the dataset

```python
from ultralytics import YOLO

# Load a model
model = YOLO('yolov8n.pt')

# Train the model
results = model.train(data='dataset_name.yaml', epochs=100, imgsz=640)
```

**Breakdown:**
 - `yolov8n.pt`
   - This specifies the model to use, YOLOv8n is the nano model, other sizes may be used and may be more accurate but will be slower
 - `dataset_name.yaml`
   - This is the previously created YAML file that specifies information about the training dataset
 - epochs
   - This is the number of times the model will train on the input data
   - More epochs will take longer but (probably) result in a more accurate model
 - imgsz
   - The size of the input images
   - If images are not this size, then they will automatically be resized, preserving the aspect ratio of objects in the image
     - the picture will not be stretched, ie. circles will stay circles
 - **Other parameters**
   - There are other optional parameters that can be passed to this function to control aspects of the training
   - These parameters can be seen on Ultralytics's [Docs](https://docs.ultralytics.com/models/train/#arguments)

### Using the Trained Model

The new model can be loaded, once it has been saved as a `.pt` file. Then the model can be passed an image (can be passed as a numpy array or image path). The `save=True` and `save_txt=True` kwargs can be passed to the `predict()` function to have more verbose output saved to files in the project directory.

```python
from ultralytics import YOLO

# load your trained model
model = YOLO("path/to/your/trained/model.pt")

# generate the results
results = model.predict(img)
```

## References

- **[YOLO Tutorial](https://learnopencv.com/train-yolov8-on-custom-dataset/)**
- **[Ultralytics YOLO Docs](https://docs.ultralytics.com/)**
- **[Data Augmentation Whitepaper](https://journalofbigdata.springeropen.com/articles/10.1186/s40537-019-0197-0)**
- **[Wikipedia CNN Page](https://en.wikipedia.org/wiki/Convolutional_neural_network)**

