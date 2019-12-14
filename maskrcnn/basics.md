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

## Training the model
