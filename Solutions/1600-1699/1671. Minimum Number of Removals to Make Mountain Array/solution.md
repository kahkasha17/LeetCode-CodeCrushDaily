# [1671. Minimum Number of Removals to Make Mountain Array](https://leetcode.com/problems/minimum-number-of-removals-to-make-mountain-array)


## Description

<!-- description:start -->

<p>You may recall that an array <code>arr</code> is a <strong>mountain array</strong> if and only if:</p>

<ul>
	<li><code>arr.length &gt;= 3</code></li>
	<li>There exists some index <code>i</code> (<strong>0-indexed</strong>) with <code>0 &lt; i &lt; arr.length - 1</code> such that:
	<ul>
		<li><code>arr[0] &lt; arr[1] &lt; ... &lt; arr[i - 1] &lt; arr[i]</code></li>
		<li><code>arr[i] &gt; arr[i + 1] &gt; ... &gt; arr[arr.length - 1]</code></li>
	</ul>
	</li>
</ul>

<p>Given an integer array <code>nums</code>​​​, return <em>the <strong>minimum</strong> number of elements to remove to make </em><code>nums<em>​​​</em></code><em> </em><em>a <strong>mountain array</strong>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,3,1]
<strong>Output:</strong> 0
<strong>Explanation:</strong> The array itself is a mountain array so we do not need to remove any elements.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [2,1,1,5,6,2,3,1]
<strong>Output:</strong> 3
<strong>Explanation:</strong> One solution is to remove the elements at indices 0, 1, and 5, making the array nums = [1,5,6,3,1].
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>3 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li>It is guaranteed that you can make a mountain array out of <code>nums</code>.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Dynamic Programming

This problem can be transformed into finding the longest increasing subsequence and the longest decreasing subsequence.

We define $left[i]$ as the length of the longest increasing subsequence ending with $nums[i]$, and define $right[i]$ as the length of the longest decreasing subsequence starting with $nums[i]$.

Then the final answer is $n - \max(left[i] + right[i] - 1)$, where $1 \leq i \leq n$, and $left[i] \gt 1$ and $right[i] \gt 1$.

The time complexity is $O(n^2)$, and the space complexity is $O(n)$. Here, $n$ is the length of the array $nums$.


#### Java

```java
class Solution {
    public int minimumMountainRemovals(int[] nums) {
        int n = nums.length;
        int[] left = new int[n];
        int[] right = new int[n];
        Arrays.fill(left, 1);
        Arrays.fill(right, 1);
        for (int i = 1; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                if (nums[i] > nums[j]) {
                    left[i] = Math.max(left[i], left[j] + 1);
                }
            }
        }
        for (int i = n - 2; i >= 0; --i) {
            for (int j = i + 1; j < n; ++j) {
                if (nums[i] > nums[j]) {
                    right[i] = Math.max(right[i], right[j] + 1);
                }
            }
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if (left[i] > 1 && right[i] > 1) {
                ans = Math.max(ans, left[i] + right[i] - 1);
            }
        }
        return n - ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minimumMountainRemovals(vector<int>& nums) {
        int n = nums.size();
        vector<int> left(n, 1), right(n, 1);
        for (int i = 1; i < n; ++i) {
            for (int j = 0; j < i; ++j) {
                if (nums[i] > nums[j]) {
                    left[i] = max(left[i], left[j] + 1);
                }
            }
        }
        for (int i = n - 2; i >= 0; --i) {
            for (int j = i + 1; j < n; ++j) {
                if (nums[i] > nums[j]) {
                    right[i] = max(right[i], right[j] + 1);
                }
            }
        }
        int ans = 0;
        for (int i = 0; i < n; ++i) {
            if (left[i] > 1 && right[i] > 1) {
                ans = max(ans, left[i] + right[i] - 1);
            }
        }
        return n - ans;
    }
};
```


<!-- solution:end -->

<!-- problem:end -->