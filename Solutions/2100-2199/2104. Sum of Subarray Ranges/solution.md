# [2104. Sum of Subarray Ranges](https://leetcode.com/problems/sum-of-subarray-ranges)

## Description

<!-- description:start -->

<p>You are given an integer array <code>nums</code>. The <strong>range</strong> of a subarray of <code>nums</code> is the difference between the largest and smallest element in the subarray.</p>

<p>Return <em>the <strong>sum of all</strong> subarray ranges of </em><code>nums</code><em>.</em></p>

<p>A subarray is a contiguous <strong>non-empty</strong> sequence of elements within an array.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,2,3]
<strong>Output:</strong> 4
<strong>Explanation:</strong> The 6 subarrays of nums are the following:
[1], range = largest - smallest = 1 - 1 = 0 
[2], range = 2 - 2 = 0
[3], range = 3 - 3 = 0
[1,2], range = 2 - 1 = 1
[2,3], range = 3 - 2 = 1
[1,2,3], range = 3 - 1 = 2
So the sum of all ranges is 0 + 0 + 0 + 1 + 1 + 2 = 4.</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,3,3]
<strong>Output:</strong> 4
<strong>Explanation:</strong> The 6 subarrays of nums are the following:
[1], range = largest - smallest = 1 - 1 = 0
[3], range = 3 - 3 = 0
[3], range = 3 - 3 = 0
[1,3], range = 3 - 1 = 2
[3,3], range = 3 - 3 = 0
[1,3,3], range = 3 - 1 = 2
So the sum of all ranges is 0 + 0 + 0 + 2 + 0 + 2 = 4.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> nums = [4,-2,-3,4,1]
<strong>Output:</strong> 59
<strong>Explanation:</strong> The sum of all subarray ranges of nums is 59.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 1000</code></li>
	<li><code>-10<sup>9</sup> &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
</ul>

<p>&nbsp;</p>
<p><strong>Follow-up:</strong> Could you find a solution with <code>O(n)</code> time complexity?</p>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1

#### Python3

```python
class Solution:
    def subArrayRanges(self, nums: List[int]) -> int:
        ans, n = 0, len(nums)
        for i in range(n - 1):
            mi = mx = nums[i]
            for j in range(i + 1, n):
                mi = min(mi, nums[j])
                mx = max(mx, nums[j])
                ans += mx - mi
        return ans
```

#### Java

```java
class Solution {
    public long subArrayRanges(int[] nums) {
        long ans = 0;
        int n = nums.length;
        for (int i = 0; i < n - 1; ++i) {
            int mi = nums[i], mx = nums[i];
            for (int j = i + 1; j < n; ++j) {
                mi = Math.min(mi, nums[j]);
                mx = Math.max(mx, nums[j]);
                ans += (mx - mi);
            }
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long subArrayRanges(vector<int>& nums) {
        long long ans = 0;
        int n = nums.size();
        for (int i = 0; i < n - 1; ++i) {
            int mi = nums[i], mx = nums[i];
            for (int j = i + 1; j < n; ++j) {
                mi = min(mi, nums[j]);
                mx = max(mx, nums[j]);
                ans += (mx - mi);
            }
        }
        return ans;
    }
};
```
### Solution 2

#### Python3

```python
class Solution:
    def subArrayRanges(self, nums: List[int]) -> int:
        def f(nums):
            stk = []
            n = len(nums)
            left = [-1] * n
            right = [n] * n
            for i, v in enumerate(nums):
                while stk and nums[stk[-1]] <= v:
                    stk.pop()
                if stk:
                    left[i] = stk[-1]
                stk.append(i)
            stk = []
            for i in range(n - 1, -1, -1):
                while stk and nums[stk[-1]] < nums[i]:
                    stk.pop()
                if stk:
                    right[i] = stk[-1]
                stk.append(i)
            return sum((i - left[i]) * (right[i] - i) * v for i, v in enumerate(nums))

        mx = f(nums)
        mi = f([-v for v in nums])
        return mx + mi
```

#### Java

```java
class Solution {
    public long subArrayRanges(int[] nums) {
        long mx = f(nums);
        for (int i = 0; i < nums.length; ++i) {
            nums[i] *= -1;
        }
        long mi = f(nums);
        return mx + mi;
    }

    private long f(int[] nums) {
        Deque<Integer> stk = new ArrayDeque<>();
        int n = nums.length;
        int[] left = new int[n];
        int[] right = new int[n];
        Arrays.fill(left, -1);
        Arrays.fill(right, n);
        for (int i = 0; i < n; ++i) {
            while (!stk.isEmpty() && nums[stk.peek()] <= nums[i]) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                left[i] = stk.peek();
            }
            stk.push(i);
        }
        stk.clear();
        for (int i = n - 1; i >= 0; --i) {
            while (!stk.isEmpty() && nums[stk.peek()] < nums[i]) {
                stk.pop();
            }
            if (!stk.isEmpty()) {
                right[i] = stk.peek();
            }
            stk.push(i);
        }
        long s = 0;
        for (int i = 0; i < n; ++i) {
            s += (long) (i - left[i]) * (right[i] - i) * nums[i];
        }
        return s;
    }
}
```

#### C++

```cpp
class Solution {
public:
    long long subArrayRanges(vector<int>& nums) {
        long long mx = f(nums);
        for (int i = 0; i < nums.size(); ++i) nums[i] *= -1;
        long long mi = f(nums);
        return mx + mi;
    }

    long long f(vector<int>& nums) {
        stack<int> stk;
        int n = nums.size();
        vector<int> left(n, -1);
        vector<int> right(n, n);
        for (int i = 0; i < n; ++i) {
            while (!stk.empty() && nums[stk.top()] <= nums[i]) stk.pop();
            if (!stk.empty()) left[i] = stk.top();
            stk.push(i);
        }
        stk = stack<int>();
        for (int i = n - 1; i >= 0; --i) {
            while (!stk.empty() && nums[stk.top()] < nums[i]) stk.pop();
            if (!stk.empty()) right[i] = stk.top();
            stk.push(i);
        }
        long long ans = 0;
        for (int i = 0; i < n; ++i) {
            ans += (long long) (i - left[i]) * (right[i] - i) * nums[i];
        }
        return ans;
    }
};
```
<!-- solution:end -->

<!-- problem:end -->