---
comments: true
difficulty: Medium
edit_url: https://github.com/doocs/leetcode/edit/main/solution/0900-0999/0974.Subarray%20Sums%20Divisible%20by%20K/README_EN.md
tags:
    - Array
    - Hash Table
    - Prefix Sum
---

<!-- problem:start -->

# [974. Subarray Sums Divisible by K](https://leetcode.com/problems/subarray-sums-divisible-by-k)

[中文文档](/solution/0900-0999/0974.Subarray%20Sums%20Divisible%20by%20K/README.md)

## Description

<!-- description:start -->

<p>Given an integer array <code>nums</code> and an integer <code>k</code>, return <em>the number of non-empty <strong>subarrays</strong> that have a sum divisible by </em><code>k</code>.</p>

<p>A <strong>subarray</strong> is a <strong>contiguous</strong> part of an array.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [4,5,0,-2,-3,1], k = 5
<strong>Output:</strong> 7
<strong>Explanation:</strong> There are 7 subarrays with a sum divisible by k = 5:
[4, 5, 0, -2, -3, 1], [5], [5, 0], [5, 0, -2, -3], [0], [0, -2, -3], [-2, -3]
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [5], k = 9
<strong>Output:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>-10<sup>4</sup> &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
	<li><code>2 &lt;= k &lt;= 10<sup>4</sup></code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Hash Table + Prefix Sum

1. **Key Insight**:

    - If there exist indices $i$ and $j$ such that $i \leq j$, and the sum of the subarray $nums[i, ..., j]$ is divisible by $k$, then $(s_j - s_i) \bmod k = 0$, this implies: $s_j \bmod k = s_i \bmod k$
    - We can use a hash table to count the occurrences of prefix sums modulo $k$ to efficiently check for subarrays satisfying the condition.

2. **Prefix Sum Modulo**:

    - Use a hash table $cnt$ to count occurrences of each prefix sum modulo $k$.
    - $cnt[i]$ represents the number of prefix sums with modulo $k$ equal to $i$.
    - Initialize $cnt[0] = 1$ to account for subarrays directly divisible by $k$.

3. **Algorithm**:
    - Let a variable $s$ represent the running prefix sum, starting with $s = 0$.
    - Traverse the array $nums$ from left to right.
        - For each element $x$:
            - Compute $s = (s + x) \bmod k$.
            - Update the result: $ans += cnt[s]$.
            - Increment $cnt[s]$ by $1$.
    - Return the result $ans$.

> Note: if $s$ is negative, adjust it to be non-negative by adding $k$ and taking modulo $k$ again.

The time complexity is $O(n)$ and space complexity is $O(n)$ where $n$ is the length of the array $nums$.

<!-- tabs:start -->

#### Python3

```python
class Solution:
    def subarraysDivByK(self, nums: List[int], k: int) -> int:
        cnt = Counter({0: 1})
        ans = s = 0
        for x in nums:
            s = (s + x) % k
            ans += cnt[s]
            cnt[s] += 1
        return ans
```

#### Java

```java
class Solution {
    public int subarraysDivByK(int[] nums, int k) {
        Map<Integer, Integer> cnt = new HashMap<>();
        cnt.put(0, 1);
        int ans = 0, s = 0;
        for (int x : nums) {
            s = ((s + x) % k + k) % k;
            ans += cnt.getOrDefault(s, 0);
            cnt.merge(s, 1, Integer::sum);
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int subarraysDivByK(vector<int>& nums, int k) {
        unordered_map<int, int> cnt{{0, 1}};
        int ans = 0, s = 0;
        for (int& x : nums) {
            s = ((s + x) % k + k) % k;
            ans += cnt[s]++;
        }
        return ans;
    }
};
```

#### Go

```go
func subarraysDivByK(nums []int, k int) (ans int) {
	cnt := map[int]int{0: 1}
	s := 0
	for _, x := range nums {
		s = ((s+x)%k + k) % k
		ans += cnt[s]
		cnt[s]++
	}
	return
}
```

#### TypeScript

```ts
function subarraysDivByK(nums: number[], k: number): number {
    const counter = new Map();
    counter.set(0, 1);
    let s = 0,
        ans = 0;
    for (const num of nums) {
        s += num;
        const t = ((s % k) + k) % k;
        ans += counter.get(t) || 0;
        counter.set(t, (counter.get(t) || 0) + 1);
    }
    return ans;
}
```

<!-- tabs:end -->

<!-- solution:end -->

<!-- problem:end -->
