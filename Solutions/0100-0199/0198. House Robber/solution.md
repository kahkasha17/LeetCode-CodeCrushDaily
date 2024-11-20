# [198. House Robber](https://leetcode.com/problems/house-robber)

## Description

<!-- description:start -->

<p>You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and <b>it will automatically contact the police if two adjacent houses were broken into on the same night</b>.</p>

<p>Given an integer array <code>nums</code> representing the amount of money of each house, return <em>the maximum amount of money you can rob tonight <b>without alerting the police</b></em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [1,2,3,1]
<strong>Output:</strong> 4
<strong>Explanation:</strong> Rob house 1 (money = 1) and then rob house 3 (money = 3).
Total amount you can rob = 1 + 3 = 4.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [2,7,9,3,1]
<strong>Output:</strong> 12
<strong>Explanation:</strong> Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
Total amount you can rob = 2 + 9 + 1 = 12.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 100</code></li>
	<li><code>0 &lt;= nums[i] &lt;= 400</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Memoization Search

We design a function $\textit{dfs}(i)$, which represents the maximum amount of money that can be stolen starting from the $i$-th house. Thus, the answer is $\textit{dfs}(0)$.

The execution process of the function $\textit{dfs}(i)$ is as follows:

-   If $i \ge \textit{len}(\textit{nums})$, it means all houses have been considered, and we directly return $0$;
-   Otherwise, consider stealing from the $i$-th house, then $\textit{dfs}(i) = \textit{nums}[i] + \textit{dfs}(i+2)$; if not stealing from the $i$-th house, then $\textit{dfs}(i) = \textit{dfs}(i+1)$.
-   Return $\max(\textit{nums}[i] + \textit{dfs}(i+2), \textit{dfs}(i+1))$.

To avoid repeated calculations, we use memoization search. The result of $\textit{dfs}(i)$ is saved in an array or hash table. Before each calculation, we first check if it has been calculated. If so, we directly return the result.

The time complexity is $O(n)$, and the space complexity is $O(n)$, where $n$ is the length of the array.

#### Java

```java
class Solution {
    private Integer[] f;
    private int[] nums;

    public int rob(int[] nums) {
        this.nums = nums;
        f = new Integer[nums.length];
        return dfs(0);
    }

    private int dfs(int i) {
        if (i >= nums.length) {
            return 0;
        }
        if (f[i] == null) {
            f[i] = Math.max(nums[i] + dfs(i + 2), dfs(i + 1));
        }
        return f[i];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        int f[n];
        memset(f, -1, sizeof(f));
        auto dfs = [&](auto&& dfs, int i) -> int {
            if (i >= n) {
                return 0;
            }
            if (f[i] < 0) {
                f[i] = max(nums[i] + dfs(dfs, i + 2), dfs(dfs, i + 1));
            }
            return f[i];
        };
        return dfs(dfs, 0);
    }
};
```
#### JavaScript

```js
function rob(nums) {
    const n = nums.length;
    const f = Array(n).fill(-1);
    const dfs = i => {
        if (i >= n) {
            return 0;
        }
        if (f[i] < 0) {
            f[i] = Math.max(nums[i] + dfs(i + 2), dfs(i + 1));
        }
        return f[i];
    };
    return dfs(0);
}
```
### Solution 2: Dynamic Programming

We define $f[i]$ as the maximum total amount that can be robbed from the first $i$ houses, initially $f[0]=0$, $f[1]=nums[0]$.

Consider the case where $i \gt 1$, the $i$th house has two options:

-   Do not rob the $i$th house, the total amount of robbery is $f[i-1]$;
-   Rob the $i$th house, the total amount of robbery is $f[i-2]+nums[i-1]$;

Therefore, we can get the state transition equation:

$$
f[i]=
\begin{cases}
0, & i=0 \\
nums[0], & i=1 \\
\max(f[i-1],f[i-2]+nums[i-1]), & i \gt 1
\end{cases}
$$

The final answer is $f[n]$, where $n$ is the length of the array.

The time complexity is $O(n)$, and the space complexity is $O(n)$. Where $n$ is the length of the array.

#### Java

```java
class Solution {
    public int rob(int[] nums) {
        int n = nums.length;
        int[] f = new int[n + 1];
        f[1] = nums[0];
        for (int i = 2; i <= n; ++i) {
            f[i] = Math.max(f[i - 1], f[i - 2] + nums[i - 1]);
        }
        return f[n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        int f[n + 1];
        memset(f, 0, sizeof(f));
        f[1] = nums[0];
        for (int i = 2; i <= n; ++i) {
            f[i] = max(f[i - 1], f[i - 2] + nums[i - 1]);
        }
        return f[n];
    }
};
```
#### JavaScript

```js
function rob(nums) {
    const n = nums.length;
    const f = Array(n + 1).fill(0);
    f[1] = nums[0];
    for (let i = 2; i <= n; ++i) {
        f[i] = Math.max(f[i - 1], f[i - 2] + nums[i - 1]);
    }
    return f[n];
}
```

### Solution 3: Dynamic Programming (Space Optimization)

We notice that when $i \gt 2$, $f[i]$ is only related to $f[i-1]$ and $f[i-2]$. Therefore, we can use two variables instead of an array to reduce the space complexity to $O(1)$.

#### Java

```java
class Solution {
    public int rob(int[] nums) {
        int f = 0, g = 0;
        for (int x : nums) {
            int ff = Math.max(f, g);
            g = f + x;
            f = ff;
        }
        return Math.max(f, g);
    }
}
```

#### C++

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        int f = 0, g = 0;
        for (int& x : nums) {
            int ff = max(f, g);
            g = f + x;
            f = ff;
        }
        return max(f, g);
    }
};
```
#### JavaScript

```js
function rob(nums) {
    let [f, g] = [0, 0];
    for (const x of nums) {
        [f, g] = [Math.max(f, g), f + x];
    }
    return Math.max(f, g);
}
```
<!-- solution:end -->

<!-- problem:end -->