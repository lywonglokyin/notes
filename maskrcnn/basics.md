# Mask RCNN usage

The below would use the "shape" example for illustration.


## Config

Before training, or applying the model, it is necessary to define config for your system. It can be done by subclassing the `Config` class from `mrcnn.config`. E.g.:

```python
class ShapeConfig(Config):
    NAME = "shapes"  # Name of config

    GPU_COUNT = 1
    IMAGE_PER_GPU = 8

    NUM_CLASSES = 1 + 3 # Number of classes (including background)
                        # In this case, 1 background + 3 shapes
    
    IMAGE_MIN_DIM = 128
    IMAGE_MAX_DIM = 128

    RPN_ANCHOR_SCALES = (8, 16, 32, 64, 128) # Anchor size in pixel
                                             # Usuall small for small picture
    
    TRAIN_ROIS_PER_IMAGE = 32  # Number of ROI (Region of interest)

    STEPS_PER_EPOCH = 100  # Small steps per simple data

    VALIDATION_STEP = 5  # Small validation step for simple data

config = ShapeConfig()
config.display()
```

## Training the model (with the help of "Shapes" example)

1. Data preparation

Because of the nature of `ShapesDataset()`, the following code would differs from actual training sets.

```python
dataset_train = ShapesDataset()
dataset_train.load_shapes(500, config.IMAGE_SHAPE[0], config.IMAGE_SHAPE[1])
dataset_train.prepare()
```

In short, what `ShapesDataset()` does is create training and validation datas on the go. The fundamental requirement of traning set and validation set is:

### Inheriting `utils.dataset`

The training and validation data must inherit the class `mrcnn.utils.dataset`. Useful functions are:

`add_class(self, source, class_id, class_name)` : Add the corresponding class to the source. E.g.:
```python
    self.add_class("shapes", 1, "squares")
```

`add_image(self, source, image_id, path, **kwargs)` : Add the corresponding image to the source.




2. Training

The code for training is as simple as:

Creating a model for training:

```python
model = modellib.MaskRCNN(mode="training", config=config,
                          model_dir=MODEL_DIR)
```

```python
model.train(dataset_train, dataset_val,
            learning_rate = config.LEARNING_RATE,
            epoch=1,
            layers='heads')
```

 Passing layers="heads" freezes all layers except the head layers.