# [3152. Special Array II](https://leetcode.com/problems/special-array-ii)

## Description

<!-- description:start -->

<p>An array is considered <strong>special</strong> if every pair of its adjacent elements contains two numbers with different parity.</p>

<p>You are given an array of integer <code>nums</code> and a 2D integer matrix <code>queries</code>, where for <code>queries[i] = [from<sub>i</sub>, to<sub>i</sub>]</code> your task is to check that <span data-keyword="subarray">subarray</span> <code>nums[from<sub>i</sub>..to<sub>i</sub>]</code> is <strong>special</strong> or not.</p>

<p>Return an array of booleans <code>answer</code> such that <code>answer[i]</code> is <code>true</code> if <code>nums[from<sub>i</sub>..to<sub>i</sub>]</code> is special.<!-- notionvc: e5d6f4e2-d20a-4fbd-9c7f-22fbe52ef730 --></p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">nums = [3,4,1,2,6], queries = [[0,4]]</span></p>

<p><strong>Output:</strong> <span class="example-io">[false]</span></p>

<p><strong>Explanation:</strong></p>

<p>The subarray is <code>[3,4,1,2,6]</code>. 2 and 6 are both even.</p>
</div>

<p><strong class="example">Example 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">nums = [4,3,1,6], queries = [[0,2],[2,3]]</span></p>

<p><strong>Output:</strong> <span class="example-io">[false,true]</span></p>

<p><strong>Explanation:</strong></p>

<ol>
	<li>The subarray is <code>[4,3,1]</code>. 3 and 1 are both odd. So the answer to this query is <code>false</code>.</li>
	<li>The subarray is <code>[1,6]</code>. There is only one pair: <code>(1,6)</code> and it contains numbers with different parity. So the answer to this query is <code>true</code>.</li>
</ol>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= queries.length &lt;= 10<sup>5</sup></code></li>
	<li><code>queries[i].length == 2</code></li>
	<li><code>0 &lt;= queries[i][0] &lt;= queries[i][1] &lt;= nums.length - 1</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Record the Leftmost Special Array Position for Each Position

We can define an array $d$ to record the leftmost special array position for each position, initially $d[i] = i$. Then we traverse the array $nums$ from left to right. If $nums[i]$ and $nums[i - 1]$ have different parities, then $d[i] = d[i - 1]$.

Finally, we traverse each query and check whether $d[to] <= from$ holds.

The time complexity is $O(n)$ and the space complexity is $O(n)$, where $n$ is the length of the array.

#### Python3

```python
class Solution:
    def isArraySpecial(self, nums: List[int], queries: List[List[int]]) -> List[bool]:
        n = len(nums)
        d = list(range(n))
        for i in range(1, n):
            if nums[i] % 2 != nums[i - 1] % 2:
                d[i] = d[i - 1]
        return [d[t] <= f for f, t in queries]
```

#### Java

```java
class Solution {
    public boolean[] isArraySpecial(int[] nums, int[][] queries) {
        int n = nums.length;
        int[] d = new int[n];
        for (int i = 1; i < n; ++i) {
            if (nums[i] % 2 != nums[i - 1] % 2) {
                d[i] = d[i - 1];
            } else {
                d[i] = i;
            }
        }
        int m = queries.length;
        boolean[] ans = new boolean[m];
        for (int i = 0; i < m; ++i) {
            ans[i] = d[queries[i][1]] <= queries[i][0];
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    vector<bool> isArraySpecial(vector<int>& nums, vector<vector<int>>& queries) {
        int n = nums.size();
        vector<int> d(n);
        iota(d.begin(), d.end(), 0);
        for (int i = 1; i < n; ++i) {
            if (nums[i] % 2 != nums[i - 1] % 2) {
                d[i] = d[i - 1];
            }
        }
        vector<bool> ans;
        for (auto& q : queries) {
            ans.push_back(d[q[1]] <= q[0]);
        }
        return ans;
    }
};
```

<!-- solution:end -->

<!-- problem:end -->