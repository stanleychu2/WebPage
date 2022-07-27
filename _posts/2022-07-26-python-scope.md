---
title: 'Python - Scope (nonlocal, global)'
date: 2022-07-26
permalink: /posts/2022/07/python-scope
release: true
categories: 
  - Python
---

## Intorductoin

Python does not need to declare variables. However, when we assign a value to a variable. It would decide its scope, the valid zone where you can see the variable. Python searches a variable from inside to outside.

```python
x = 10
def printX():
    return x

printX() # output: x = 10

```
There is a function named `printX`. Its purpose is to return the current `x` it can get. When we don't assign a value in the function, we find the nearest x in the global.

```python
x = 10
def printX():
    x = 20
    return x

printX() # output: x = 20

```
If there is a closer `x` near `return x`, we need to follow the rule from inside to outside, so the output is 20.

## Nonlocal / Global
The last part is the scenario to call the value. What if we want to modify the local or global variables. To do so, Python provides us with two keywords - `nonlocal` and `global`.
`global` helps us assign a new value to a variable from the global zone. It changes the value permanently. After the function call, the value still maintains the value you set in the function.

```python

x = 10
print(x) # output: x = 10

def changeX():
    global x
    x = 20

changeX()
print(x) # output: x = 20

```
We build a function to change the variable `x` from the global this time. We use the `global` to tell the `changeX` that we want to modify the `x` outside instead to construct a new local variable `x`.  We observe the new `x` value, as soon as the `changeX` is called.

`nonlocal` does a similar thing yet it apply in the nested function. A nested function is a function inside another function. Apart from nested function, other things are as same as `global`.

```python
def outer():
    x = 10
    print(x) # output: x = 10

    def inner():
        nonlocal x
        x = 20
    inner()
    print(x) # output: x = 20

```
There are two functions in the upside code. The `inner` function represents the nested function in the `outer` function. The `nonlocal x` in the `inner` show that we want to get the modification permission to the `x`. It is useful when you want to get some values in the middle of the recursive process.

## Notice
If you just want to use the function inside the object, you don't need to use the keywords. You use the keywords only if you need to assign,`=`, `+=`, `-=`,  a new value.

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

result = []
def dfs(node: Optional[TreeNode]):
    
    if node:
        dfs(node.left)
        result.append(node.val)
        dfs(node.right)
```
This is a pre-order `dfs`. We append a node value to the `result` outside the function. However, we don't need to use any keywords. The `append` changes the list object itself.

There are no block variables in Python, so the variable in the `if` statement can be access outside the `if` block.

```python
if True:
    x = 10
print(x) # output: x = 10
```

------