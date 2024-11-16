# [163. Missing Ranges 🔒](https://leetcode.com/problems/missing-ranges)

## Description

<!-- description:start -->

<p>You are given an inclusive range <code>[lower, upper]</code> and a <strong>sorted unique</strong> integer array <code>nums</code>, where all elements are within the inclusive range.</p>

<p>A number <code>x</code> is considered <strong>missing</strong> if <code>x</code> is in the range <code>[lower, upper]</code> and <code>x</code> is not in <code>nums</code>.</p>

<p>Return <em>the <strong>shortest sorted</strong> list of ranges that <b>exactly covers all the missing numbers</b></em>. That is, no element of <code>nums</code> is included in any of the ranges, and each missing number is covered by one of the ranges.</p>

<p>&nbsp;</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [0,1,3,50,75], lower = 0, upper = 99
<strong>Output:</strong> [[2,2],[4,49],[51,74],[76,99]]
<strong>Explanation:</strong> The ranges are:
[2,2]
[4,49]
[51,74]
[76,99]
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [-1], lower = -1, upper = -1
<strong>Output:</strong> []
<strong>Explanation:</strong> There are no missing ranges since there are no missing numbers.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>-10<sup>9</sup> &lt;= lower &lt;= upper &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= nums.length &lt;= 100</code></li>
	<li><code>lower &lt;= nums[i] &lt;= upper</code></li>
	<li>All the values of <code>nums</code> are <strong>unique</strong>.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Simulation

We can simulate the problem directly according to the requirements.

The time complexity is $O(n)$, where $n$ is the length of the array $nums$. Ignoring the space consumption of the answer, the space complexity is $O(1)$.

#### Java

```java
class Solution {
    public List<List<Integer>> findMissingRanges(int[] nums, int lower, int upper) {
        int n = nums.length;
        if (n == 0) {
            return List.of(List.of(lower, upper));
        }
        List<List<Integer>> ans = new ArrayList<>();
        if (nums[0] > lower) {
            ans.add(List.of(lower, nums[0] - 1));
        }
        for (int i = 1; i < n; ++i) {
            if (nums[i] - nums[i - 1] > 1) {
                ans.add(List.of(nums[i - 1] + 1, nums[i] - 1));
            }
        }
        if (nums[n - 1] < upper) {
            ans.add(List.of(nums[n - 1] + 1, upper));
        }
        return ans;
    }
}
```

<!-- solution:end -->

<!-- problem:end -->