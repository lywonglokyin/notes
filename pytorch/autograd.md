# Autograd

One big functionality of Pytorch is that it supports auto back propogation.

## Example

```python
# A simple fully connected network
y_pred = x.mm(w1).clamp(min=0).mm(w2)

# A simple sum of square error
loss = (y_pred - y).pow(2).sum()

# This is the MAGIC.
# After this call, all Tensors with requires_grad=True would compute a
# corresponing gradient, stored in, e.g. w1.grad
loss.backward()

# This tell pytorch to ignore grad calculation, because, we are just updating the weights.
# An alternative way is to operate on w1.data.
with torch.no_grad():
    w1 -= learning_rate * w1.grad
    w2 -= learning_rate * w2.grad

    # Zeroing the gradient
    w1.grad.zero_()
    w2.grad.zero_()
```

## Defining custom autograd

https://pytorch.org/tutorials/beginner/pytorch_with_examples.html#pytorch-defining-new-autograd-functions

## PyTorch nn

Data scientist are all lazy. Even if the above is good enough for small model (and actually with good flexibility), it is still troublesome when building large network. Thus, the **nn** module defined some common layers for use.

See https://pytorch.org/tutorials/beginner/pytorch_with_examples.html#pytorch-nn.

## Optimizer

And, with even more simplicity, use **optimizer**

```python
model = torch.nn.Sequential(
    torch.nn.Linear(D_in, H),
    torch.nn.ReLU(),
    torch.nn.Linear(H, D_out)
)
loss_fn = torch.nn.MSELoss(reduction='sum')

learning_rate = 1e-4
optimizer = torch.optim.Adam(model.parameter(), lr=learning_rate)
for t in range(500):
    # Forward pass: compute predicted y by passing x to the model.
    y_pred = model(x)

    # Compute and print loss.
    loss = loss_fn(y_pred, y)
    if t % 100 == 99:
        print(t, loss.item())

    # Before the backward pass, use the optimizer object to zero all of the
    # gradients for the variables it will update (which are the learnable
    # weights of the model). This is because by default, gradients are
    # accumulated in buffers( i.e, not overwritten) whenever .backward()
    # is called. Checkout docs of torch.autograd.backward for more details.
    optimizer.zero_grad()

    # Backward pass: compute gradient of the loss with respect to model
    # parameters
    loss.backward()

    # Calling the step function on an Optimizer makes an update to its
    # parameters
    optimizer.step()
```
