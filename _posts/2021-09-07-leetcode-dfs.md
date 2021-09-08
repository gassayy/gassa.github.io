---
layout: post
title: Leetcode - DFS
author: gassa
comments: true
category: leetcode
tags: [algorithm, dfs]
---

## Combination I and II

### LC 39. Combination Sum I

**Key points**: The same number may be chosen from candidates an unlimited number of times.

```python
class Solution:
    def combinationSum(self, candidates: List[int], target: int) -> List[List[int]]:
      ans = []
      candidates = sorted(candidates)
      L = len(candidates)
      def dfs(path: List[int], acc: int, start: int): 
        if acc == target:
          ans.append(path.copy())
          return
        if acc > target:
          return
        for idx in range(start, L):
          path.append(candidates[idx])
          dfs(path, acc + candidates[idx], idx)
          path.pop()
        return
      dfs([], 0, 0)
      return ans
```

### LC 40. Combination Sum II 

**Key points**: Each number in candidates may only be used once in the combination.

```python
class Solution:
    def combinationSum2(self, candidates: List[int], target: int) -> List[List[int]]:
        ans = []
        candidates = sorted(candidates)
        print(candidates)
        L = len(candidates)
        def dfs(path: List[int], acc: int, start: int): 
          if acc == target:
            ans.append(path.copy())
            return
          if acc > target:
            return
          for idx in range(start, L):
            if idx > start and candidates[idx-1] == candidates[idx]:
              continue
            path.append(candidates[idx])
            dfs(path, acc + candidates[idx], idx+1)
            path.pop()
          return
        dfs([], 0, 0)
        return ans
```

### LC 46. Permutation I

**Key points**: all possible permutation in any order.

```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
      ans = []
      L = len(nums)
      def dfs(path):
        if len(path) == L:
          ans.append(path.copy())
          return 
        for idx in range(0, L):
          if nums[idx] not in path:
            path.append(nums[idx])
            dfs(path)
            path.pop()
        return
      dfs([])
      return ans
```

### LC 47. Permutation II

**Key points**: Permutation I with duplicated inputs.

```python
class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
      ans = []
      L = len(nums)
      visited = [False] * L
      nums = sorted(nums)
      def dfs(path):
        if len(path) == L:
          ans.append(path.copy())
          return 
        for idx in range(0, L):
          if visited[idx] or (idx != 0 and visited[idx-1] and nums[idx-1] == nums[idx]):
              continue
          path.append(nums[idx])
          visited[idx] = True
          dfs(path)
          path.pop()
          visited[idx] = False
        return
      dfs([])
      return ans
```

### LC 77. Combinations

```python
class Solution:
    def combine(self, n: int, k: int) -> List[List[int]]:
      ans = []
      def dfs(path, start):
        if len(path) == k:
          ans.append(path.copy())
          return
        for i in range(start, n+1):
          path.append(i)
          dfs(path, i + 1)
          path.pop()
        return
      dfs([], 1)
      return ans
```



