# [209. Minimum Size Subarray Sum](https://leetcode.com/problems/minimum-size-subarray-sum)

## Description

<!-- description:start -->

<p>Given an array of positive integers <code>nums</code> and a positive integer <code>target</code>, return <em>the <strong>minimal length</strong> of a </em><span data-keyword="subarray-nonempty"><em>subarray</em></span><em> whose sum is greater than or equal to</em> <code>target</code>. If there is no such subarray, return <code>0</code> instead.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> target = 7, nums = [2,3,1,2,4,3]
<strong>Output:</strong> 2
<strong>Explanation:</strong> The subarray [4,3] has the minimal length under the problem constraint.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> target = 4, nums = [1,4,4]
<strong>Output:</strong> 1
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> target = 11, nums = [1,1,1,1,1,1,1,1]
<strong>Output:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= target &lt;= 10<sup>9</sup></code></li>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>4</sup></code></li>
</ul>

<p>&nbsp;</p>
<strong>Follow up:</strong> If you have figured out the <code>O(n)</code> solution, try coding another solution of which the time complexity is <code>O(n log(n))</code>.

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Prefix Sum + Binary Search

First, we preprocess the prefix sum array $s$ of the array $nums$, where $s[i]$ represents the sum of the first $i$ elements of the array $nums$. Since all elements in the array $nums$ are positive integers, the array $s$ is also monotonically increasing. Also, we initialize the answer $ans = n + 1$, where $n$ is the length of the array $nums$.

Next, we traverse the prefix sum array $s$. For each element $s[i]$, we can find the smallest index $j$ that satisfies $s[j] \geq s[i] + target$ by binary search. If $j \leq n$, it means that there exists a subarray that satisfies the condition, and we can update the answer, i.e., $ans = min(ans, j - i)$.

Finally, if $ans \leq n$, it means that there exists a subarray that satisfies the condition, return $ans$, otherwise return $0$.

The time complexity is $O(n \times \log n)$, and the space complexity is $O(n)$. Here, $n$ is the length of the array $nums$.

#### Java

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int n = nums.length;
        long[] s = new long[n + 1];
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + nums[i];
        }
        int ans = n + 1;
        for (int i = 0; i <= n; ++i) {
            int j = search(s, s[i] + target);
            if (j <= n) {
                ans = Math.min(ans, j - i);
            }
        }
        return ans <= n ? ans : 0;
    }

    private int search(long[] nums, long x) {
        int l = 0, r = nums.length;
        while (l < r) {
            int mid = (l + r) >> 1;
            if (nums[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        return l;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int n = nums.size();
        vector<long long> s(n + 1);
        for (int i = 0; i < n; ++i) {
            s[i + 1] = s[i] + nums[i];
        }
        int ans = n + 1;
        for (int i = 0; i <= n; ++i) {
            int j = lower_bound(s.begin(), s.end(), s[i] + target) - s.begin();
            if (j <= n) {
                ans = min(ans, j - i);
            }
        }
        return ans <= n ? ans : 0;
    }
};
```
### Solution 2: Two Pointers

We can use two pointers $j$ and $i$ to maintain a window, where the sum of all elements in the window is less than $target$. Initially, $j = 0$, and the answer $ans = n + 1$, where $n$ is the length of the array $nums$.

Next, the pointer $i$ starts to move to the right from $0$, moving one step each time. We add the element corresponding to the pointer $i$ to the window and update the sum of the elements in the window. If the sum of the elements in the window is greater than or equal to $target$, it means that the current subarray satisfies the condition, and we can update the answer, i.e., $ans = \min(ans, i - j + 1)$. Then we continuously remove the element $nums[j]$ from the window until the sum of the elements in the window is less than $target$, and then repeat the above process.

Finally, if $ans \leq n$, it means that there exists a subarray that satisfies the condition, return $ans$, otherwise return $0$.

The time complexity is $O(n)$, and the space complexity is $O(1)$. Here, $n$ is the length of the array $nums$.

#### Java

```java
class Solution {
    public int minSubArrayLen(int target, int[] nums) {
        int l = 0, n = nums.length;
        long s = 0;
        int ans = n + 1;
        for (int r = 0; r < n; ++r) {
            s += nums[r];
            while (s >= target) {
                ans = Math.min(ans, r - l + 1);
                s -= nums[l++];
            }
        }
        return ans > n ? 0 : ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int l = 0, n = nums.size();
        long long s = 0;
        int ans = n + 1;
        for (int r = 0; r < n; ++r) {
            s += nums[r];
            while (s >= target) {
                ans = min(ans, r - l + 1);
                s -= nums[l++];
            }
        }
        return ans > n ? 0 : ans;
    }
};
```

<!-- solution:end -->

<!-- problem:end -->