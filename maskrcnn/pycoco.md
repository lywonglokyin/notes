# Pycoco dataset

Pycoco is an API built for ease-of-use of the image segmentation, labeling, etc. dataset.

To use pycoco, type:

```python
from pycocotools.coco import COCO
```

## Flow

1. Load instance annotations with `COCO` api. e.g.:

```python
annFile = 'path/to/annotations/instances_val2017.json'
coco = COCO(annFile)
```

From this we can get some information about the dataset, e.g.:

2. Catergories and super-categories

```python
cats = coco.loadCats(coco.getCatIds())
cats_name = [cat['name'] for cat in cats]

super_cats_name = set([cat['supercategory'] for cat in cats])
```