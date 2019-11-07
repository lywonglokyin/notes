# Generator

Generator is a way to return an iterator without using much memory. 

Suppose we want to generate a list of integer, and suppose each integer is memory-expensive.

Old code:
```python
def firstn(n):
  num, nums = 0 ,[]
  while num < n:
    nums.append(num)
    num+=1
  return nums

sum_of_first_n = sum(firstn(100000))
```

More efficient code:
```python
class firstn(object):
  def __init__(self, n):
    self.n = n
    self.num, self.nums = 0 ,[]

  def __iter__(self):
    return self

  # For python3 compatability
  def __next__(self):
    return self.next()
  
  def next(self):
    if self.num < self.n:
      cur, self.num = self.num, self.num+1
      return cur
    else:
      raise StopIteration()

sum_of_first_n = sum(firstn(100000)) # memory-efficient as sum can work on iterable
```
## Generator approach

```python
def firstn(n):
  num = 0
  while num < n:
    yield num
    num += 1

sum_of_first_n = sum(firstn(100000))
```

## List vs Generator

We can turn list comprehension into generators:

```python
# List comprehension
doubles = [ 2*n for n in range(50)]

# Same as comprehension above
doubles = list(2*n for n in range(50))
```