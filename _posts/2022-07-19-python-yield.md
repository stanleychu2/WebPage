---
title: 'Python - Yield'
date: 2022-07-19
permalink: /posts/2022/07/python-yield_function
release: true
categories: 
  - Python
---

## Intorductoin

`yield` is a special keyword like `return`. It can create an iterator in Python. If you want to get a value from an iterator, you need to use the `next` function or a `for` loop. Sometimes, we find that the list index can’t be used directly, `map` and `filter` output. It might mean that the output is an iterator yet to be called in the normal way.

```python
hello = "Hello world!"
helloIter = iter(hello)

while True:
    # when no val to return, next return deault "None"
    nxt = next(helloIter, None)
    if not nxt: break
    print(nxt)
```

Also, we can save memory by using the iterator. The iterator returns a value once a time. Thus, we don't need a local variable to store all the outputs. As soon as we require the value, we call the iterator which just maintains the function state. In addition to `iter`, `yield` is another way to create the iterator. Once we use `yield` to return the value, we consider that function a generator (iterator).

```python
def constructList(length: int) -> List[int]:

    output = []
    for num in range(1, length + 1):
        output.append(num)

    return output

for n in constructList(10000):
    print(n)
```

```python
def constructList(length: int) -> int:

    for num in range(1, length + 1):
        yield num

for n in constructList(10000):
    print(n)
```


## Usage

[Leetcode 173. Binary Search Tree Iterator [medium]](https://leetcode.com/problems/binary-search-tree-iterator/) is a good example in practice. In the description, we know that it’s a BST problem. In-order-traversal can help us gain the element in ascending order. However, the problem wants to get the elements one by one, so we need to turn our DFS function into a generator (iterator).

```python
class BSTIterator:
        
    def __init__(self, root: Optional[TreeNode]):
        self.root = root
        self.iter = self.dfs(self.root)
        self._cur = next(self.iter)
    
    def dfs(self, node: Optional[TreeNode]):
        
        if node:
            yield from self.dfs(node.left)
            yield node.val
            yield from self.dfs(node.right)

    def next(self) -> int:
        result = self._cur
        self._cur = next(self.iter, None)
        return result

    def hasNext(self) -> bool:
        return (False if self._cur is None else True)
```
`yield from generator` is equal to `for v in generator: yield v`. We first need to get the elements from the left subtree. Second, return the current node value. The last one to return is the right subtree.

------