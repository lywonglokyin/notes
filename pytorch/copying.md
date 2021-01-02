# Copying tensors

This note records multiple ways of copying tensors.

### `Tensor.detach()`

Shares the data, which will be used for computations that does not require gradient.

- Shares data
- Gradient will not be propagated

### `Tensor.clone()`

Create a copy that do not share data memory, but gradient will flow back to the input.

- Does not share data
- Gradient will be propagated

### `Tensor.clone().detach()`

Create a copy that do not share data memory and do not flow gradient back.

### `.copy_()`

Copy the target tensor content the the existing tensor.

- Different shapes will be broadcasted.