---
title: 'Binary Search'
date: 2022-09-02
permalink: /posts/2022/09/binary-search
release: true
categories: 
  - Algorithm
---

## Introduction

While we discuss an algorithm problem, we focus on time and space complexity. `O(n)`, linear time, is a good enough solution. There are a few methods that can beat linear time. However, binary search is one of the `O(logN)` solutions. The timing that we can use the binary search is whether we discover some kind of monotonicity. Usually, a sorted structure problem is a fine candidate. 

## Binary Search - Leftmost

There are two kinds of binary search. The leftmost one finds the index which suits the condition with its leftmost position. If the target doesn't exist in the search space, it would return the position whose value is the next greater value. If the target is the biggest value in the search space, `len(space)` would return. This index doesn't valid in the current space, though it's the right position.

[Leetcode 704. Binary Search [easy]](https://leetcode.com/problems/binary-search/)
```python
def search(self, nums: List[int], target: int) -> int:
    
    l, r = 0, len(nums)
    while l < r:
        m = (l+r)//2
        
        if nums[m] >= target:
            r = m
        else:
            l = m+1
    
    return (-1 if l==len(nums) or nums[l]!=target else l)

```
The `nums` list is sorted in ascending order. We want to find the target in the `nums`. If we don't find the target in the list, return -1. `l` is the left boundary. `r` is the right boundary, and `m` is the middle position. We find that middle is greater than the target, so we move the `r` to the `m` due to the sorted property - `|l---t--m------r|` according to `nums[m] >= target`. In contrast, we move the `l` to the `m`. In the end, the `l` is the index of the target. Be aware of the situation where the target is not in the space.

## Binary Search - Rightmost

The rightmost one is the reverse of the leftmost. It finds the rightmost position which fits the condition, `nums[m] <= target`. When the target is the smallest value not in the `nums`, the function would return -1. In the case where the target is not in the search space, but the previous less value exists, the function would return the previous less value position.

```python
def search(self, nums: List[int], target: int) -> int:
    
    l, r = 0, len(nums)
    while l < r:
        m = (l+r)//2
        
        if nums[m] <= target:
            l = m+1
        else:
            r = m
    
    return (-1 if r-1==-1 or nums[r-1]!=target else r-1)
```

## Application - [bisect](https://docs.python.org/zh-tw/3/library/bisect.html)

After understanding the concept of the binary search, you can use the library provided by Python. `bisect` can help you do the entire above work. `bisect.bisect_left` is the leftmost binary search. `bisect.bisect_right` is the rightmost binary search. Now, how can we apply this algorithm to some problems?
[34. Find First and Last Position of Element in Sorted Array [medium]](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/) is a leetcode problem can use both types.

```python
def searchRange(self, nums: List[int], target: int) -> List[int]:
    
    leftmost = bisect.bisect_left(nums, target)
    rightmost = bisect.bisect_right(nums, target)-1
    
    if leftmost == len(nums) or rightmost == -1 or \
    nums[leftmost] != target or nums[rightmost] != target:
        return [-1, -1]
    else:
        return [leftmost, rightmost]

```

The description shows that it's a sorted array, so we can use the binary search. Yet, there are duplicate elements in the array. To handle this situation, we require `bisect_left` to find the left boundary, and `bisect_right` to find the right boundary of the same target. These four statements, `leftmost == len(nums) or rightmost == -1 or nums[leftmost] != target or nums[rightmost] != target`, mean that `bisect` cannot find the target in the `nums`.

> ⛔ `bisect_right` return the `r`. We need to minus 1 to correct the wrong index.








------