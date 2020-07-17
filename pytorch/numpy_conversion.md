# Numpy conversion

This file provide some common numpy convertion to pytorch.

## Using GPU

To use GPU, specify:

```python
device = torch.device("cuda:0")
# Or, if you want to use CPU instaed
#     device = torch.device("cpu")
```

## Data initialization

1. Random tensor

    numpy:
    ```python
    np.random.randn(dim_1, dim_2)
    ```

    pytorch:
    ```python
    device = torch.device("cuda:0")

    torch.randn(dim_1, dim_2, device=device, dtype=torch=torch.float)
    ```


## Common layers

1. ReLU

    numpy:
    ```python
    relu_layer = np.maximum(prev, 0)
    ```

    pytorch:
    ```python
    relu_layer = prev.clamp(min=0)
    ```

2. Fully connected layer

    numpy:
    ```python
    h = x.dot(w1)
    ```

    pytorch:
    ```python
    h = x.mm(w1)  # Stands for matrix multiplication
    ```