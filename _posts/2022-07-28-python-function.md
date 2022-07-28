---
title: 'Python - Map, Filter, Reduce'
date: 2022-07-28
permalink: /posts/2022/07/python-function
release: true
categories: 
  - Python
---

## Introduction

There are three functions I frequently use in Python - `map`, `filter`, and `reduce`. They make complicated code simple, clear, and readable. With these powerful tools, we can reduce the repeated block that we need to apply to the array every time. The `for` loop and temporary variables are replaced.

## Map

As we want to apply the same operation on items in an array, it is a great time to use `map`. `map(function, iterable)` takes two parameters. `function` is the action you want to do on items. It can be a separately defined function or a `lambda`. `iterable` is the target you want to change. It doesn't have a specific type that can be iterated is valid.

```python
nums = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# no map
tempt = []
for index in range(len(nums)):
    tempt.append(nums[index]+2)

# with map
newNums = list(map(lambda x: x+2, nums))
# output: [3, 4, 5, 6, 7, 8, 9, 10, 11, 12]

```
We want to increase 2 on the elements in `nums`. The `map` gets the job done without writing the loop in the code while they are equally functional except that the `map` return an iterable object. If you have to get items by the index, the type transformation is required. Overall, it gets the code neater.

## Filter

`filter` is easy to understand literally. `filter(function, iterable)` has the same format as `map`. However, `function` here has a constrain which has to return a `bool` (`True` or `False`). As the returned value is `True`, it means that this value would stay and store. We always use it to pick the desired values in the array.

```python
nums = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# no filter
tempt = []
for index in range(len(nums)):
    if nums[index] % 2 == 0:
        tempt.append(nums[index])

# with filter
newNums = list(filter(lambda x: x%2==0, nums))
# output: [2, 4, 6, 8, 10]

```
`filter` resembles a `for` loop yet faster. We gain even numbers in the `nums` through a `lambda` whose returned type is `bool` - `x%2==0`. As the `map`, the output needs a type conversion to act like a `list`.

## Reduce

As for `Reduce`, it's an aggregation function unlike the previous two functions. The output of `reduce` is a single item. Besides, it isn't a built-in function and need to be imported from the `functools`. The `function` in `reduce(function, iterable)` is a two-parameters function. `reduce` can inmitate the built-in function `sum` which tells us the summation for elements in the array by setting the `function` to `lambda x, y: x+y`.
```python
from functools import reduce

nums = [1, 2, 3, 4, 5]

# no reduce
tempt = 1
for index in range(len(nums)):
    tempt *= nums[index]

# with reduce
aggreg = reduce(lambda x, y: x*y, nums)
# output: 120
# (1*2), [3, 4, 5]
# ((1*2)*3) [4, 5]
# (((1*2)*3)*4) [5]
# ((((1*2)*3)*4)*5) []

```
The above code is the implementation of factorial. First, we take the first two elements to multiply. Second, the result of the first two elements then is multiplied by the third element. This process keeps going until running out of the element in the array. It can do things like `max` and `min` through the designed `function`.

## Others

`chain` is also a practical function in real-world scenarios. `chain (*iterables)` means you can put several iterable objects at that position. It concats the 2-dim objects to the 1-dim. On the other hand, we can say that it is a function that takes a series of iterables and returns one iterable.
```python
def chain(*iterables):
    for it in iterables:
        for each in it:
            yield each
```
`chain` internal implementation is about [yield](/posts/2022/07/python-yield_function). The function iterates through the outer dimension and returns every element in the inner object one by one.

```python
from itertools import chain

# flatten the string
strChain = list(chain("abc", "def", "ghi"))
# output: ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i']

# turn 2-dim list into 1-dim
nums = [[1, 2], [3, 4], [5, 6]]
numsChain = list(chain(*nums))
# output: [1, 2, 3, 4, 5, 6]

```

------