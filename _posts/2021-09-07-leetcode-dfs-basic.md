---
layout: post
title: Leetcode - DFS - Basic
author: gassa
comments: true
category: leetcode
tags: [algorithm, dfs]
---
## Combination, Permutation and Subsets

这些题的关键点是如何用深度优先的方式，搜索所有可能的combination或者是permutation, 从中找出符合要求的解。

```
基础题目：LC78 Subsets:
Given two integers n and k, return all possible combinations of k numbers out of the range [1, n].
```

![subsets search graph](/pics/leetcode/dfs/lc78-subsets.png)

在上面的图中，下面一层是上面一层在`dfs` for-loop中的`dfs`的展开，由于要去掉重复的subset，所以每次要把`i+1`. 

LC90基本算是LC78的follow-up，输入数组有重复的元素。排重的方法是：在`dfs`里做操作前，检查一下当前元素`nums[i]`是不是与前一个元素`nums[i-i]`相同，如果相同就跳过。

LC39和LC40(Combination Sum I and II)，需要检查`dfs`停止的条件。
LC46和LC47(Permutaion I and II), 需要注意的是每次`dfs`中for-loop从0开始，因为[1, 2, 3]和[2, 1, 3]是不用的permutation。同时，还要保证每个元素之用一次。


### LC 78. Subsets

#### Solution 1 using DFS backtracing
<details>
<summary>Solution</summary>
{% highlight python %}
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        ans = []
        n = len(nums)
        def dfs(path, start):
          ans.append(path.copy())
          for i in range(start, n):
            path.append(nums[i])
            dfs(path, i+1)
            path.pop()
          return
        dfs([], 0)
        return ans
{% endhighlight %}
</details>

### Solution 2 using bit mask
<details>
<summary>Hints:</summary>
<pre>
each subset can be mapped to an binary interger between 0, 2^1, 2^2, ..., 2^n
For example, [1, 2, 3]
0 => 000 => []
1 => 001 => [1]
2 => 010 => [2]
3 => 011 => [1, 2]
4 => 100 => [3]
5 => 101 => [1, 3]
6 => 110 => [2, 3]
7 => 111 => [1, 2, 3]
</pre>
</details>

<details>
<summary>Solution</summary>
{% highlight python %}
class Solution:
  def subsets(self, nums: List[int]) -> List[List[int]]:
    n = len(nums)
    nth_bit = 1 << len(nums)
    ans = []
    for i in range(2**n):
      mask = bin(i | nth_bit)[3:]
      ans.append([ nums[j] for j in range(n) if mask[j] == '1' ])
    return ans
{% endhighlight %}
</details>

### LC 90. Subsets II

**Key points**: inputs contain duplicates, the solution must not contain duplicate subsets.

<details>
<summary>Solution</summary>
{% highlight python %}
class Solution:
  def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
    ans = []
    n = len(nums)
    nums = sorted(nums)
    def dfs(path, start):
      ans.append(path.copy())
      for i in range(start, n):
        if i > start and nums[i-1] == nums[i]:
          continue;
        path.append(nums[i])
        dfs(path, i+1)
        path.pop()
      return
    dfs([], 0)
    return ans
{% endhighlight %}
</details>

### LC 39. Combination Sum I

**Key points**: The same number may be chosen from candidates an unlimited number of times.

<details>
<summary>Solution</summary>
{% highlight python %}

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
{% endhighlight %}
</details>

### LC 40. Combination Sum II 

**Key points**: Each number in candidates may only be used once in the combination.

<details>
<summary>Solution</summary>
{% highlight python %}
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
{% endhighlight %}
</details>

### LC 46. Permutation I

**Key points**: all possible permutation in any order.

<details>
<summary>Solution</summary>
{% highlight python %}
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
{% endhighlight %}
</details>

### LC 47. Permutation II

**Key points**: Permutation I with duplicated inputs.

<details>
<summary>Solution</summary>
{% highlight python %}
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
{% endhighlight %}
</details>

### LC 77. Combinations

<details>
<summary>Solution</summary>
{% highlight python %}class Solution:
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
{% endhighlight %}
</details>


