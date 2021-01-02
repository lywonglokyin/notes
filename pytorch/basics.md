# Pytorch

## Why use Pytorch?

- Tensors are available, that behaves similar to numpy but can run on GPU.
- Automatic differentiation
- (?) Easy creation of common layers of deep neural networks.

## Tensor creation

Quite similar to numpy: `torch.zeros`, `torch.rand`, ...

Also:

- new_xxxx: e.g. `new_ones()`, a quicker way to define a new tensor with the same dtype and device.

- xxxx_like: e.g. `randn_like()`, a quicker way to define a new tensor with the same size.


## Operations

1. Direct operate

```python
x + y
# or
torch.add(x, y)
```

2. Specify output

```python
torch.add(x, y, out=result)
```

3. Inplace

```python
y.add_(x)
# The naming convention of pytorch is that, when there is a trailing _ in the method, this method will change the object.
```

## Resizing

### `view()`

Resize the tensor to the desired shape

### `item()`

Get the single item tensor get value.

## From numpy to tensor

```python
b = torch.from_numpy(a)
```

## Using CUDA

```python
device = torch.device("cuda")
# Directly create on GPU
x = torch.ones([5, 3], device=device)
# Relocate to GPU
y = y.to(device)
# Relocate back to CPU
y = y.to('cpu')
```

