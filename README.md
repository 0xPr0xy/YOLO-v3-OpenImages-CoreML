# YOLOv3 CoreML

[Paper on YOLOv3](https://arxiv.org/abs/1804.02767)

[Darknet Github](https://github.com/pjreddie/darknet)

[Darknet Website](https://pjreddie.com/darknet/)

The [converted CoreML model](https://github.com/0xPr0xy/YOLO-v3-OpenImages-CoreML/blob/master/YOLO-CoreML/yolo-openimages.mlmodel) is included in this repository

## YOLOv3 on Openimages dataset

### Create keras model

1. Download the [openimages.cfg](https://github.com/pjreddie/darknet/blob/master/cfg/yolov3-openimages.cfg)

2. Download the [openimages.weights](https://pjreddie.com/media/files/yolov3-openimages.weights)

3. Install [keras-yolov3](https://github.com/qqwweee/keras-yolo3.git)

4. Run the following:

```Shell
python3 convert.py yolov3-openimages.cfg yolov3-openimages.weights model_data/yolo-openimages.h5
```

### Convert keras model to coreml

1. Install coremltools:

```
pip install coremltools
```

2. Create and run this file:

```
import coremltools

coreml_model = coremltools.converters.keras.convert(
    './model_data/yolo-openimages.h5',
    input_names='image',
    image_input_names='image',
    image_scale=1/255.,
    input_name_shape_dict = {'image': [None, 608, 608, 3]})

coreml_model.author = 'Original paper: Joseph Redmon, Ali Farhadi'
coreml_model.license = 'Public Domain'
coreml_model.short_description = "The YOLO network from the paper 'YOLOv3: An Incremental Improvement' (2018)"
coreml_model.input_description['image'] = 'Input image'

print(coreml_model)
coreml_model.save('yolo-openimages.mlmodel')
```

Now you have a CoreML model!

### Implementation info

To implement something usefull with this you'll need the anchors, found in the `yolov3 -openimages.cfg`

`[10.13,  16.30,  33.23,  30.61,  62.45,  59.119,  116.90,  156.198,  373.326]`

And the labels for the openimages dataset categories

[openimages.labels](https://raw.githubusercontent.com/pjreddie/darknet/master/data/openimages.names)

### TODO

Tasks for converting this code from YOLOv3 COCO dataset usage to OpenImages dataset use include:

- Convert YOLO v3 OpenImages to CoreML ✅
- Change anchors in code to reflect the OpenImages dataset anchors ✅
- Update colors in code to not generate and create 80 for COCO but instead use 1 color instead of 601 colors for OpenImages ✅
- fix crashes in `computeBoundingBoxes` 1 due to code not being compatible with the now smaller anchors array, another due to offset calculation

