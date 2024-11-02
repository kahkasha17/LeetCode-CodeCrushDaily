# [64. Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum)

## Description

<!-- description:start -->

<p>Given a <code>m x n</code> <code>grid</code> filled with non-negative numbers, find a path from top left to bottom right, which minimizes the sum of all numbers along its path.</p>

<p><strong>Note:</strong> You can only move either down or right at any point in time.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0000-0099/0064.Minimum%20Path%20Sum/images/minpath.jpg" style="width: 242px; height: 242px;" />
<pre>
<strong>Input:</strong> grid = [[1,3,1],[1,5,1],[4,2,1]]
<strong>Output:</strong> 7
<strong>Explanation:</strong> Because the path 1 &rarr; 3 &rarr; 1 &rarr; 1 &rarr; 1 minimizes the sum.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> grid = [[1,2,3],[4,5,6]]
<strong>Output:</strong> 12
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>m == grid.length</code></li>
	<li><code>n == grid[i].length</code></li>
	<li><code>1 &lt;= m, n &lt;= 200</code></li>
	<li><code>0 &lt;= grid[i][j] &lt;= 200</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Dynamic Programming

We define $f[i][j]$ to represent the minimum path sum from the top left corner to $(i, j)$. Initially, $f[0][0] = grid[0][0]$, and the answer is $f[m - 1][n - 1]$.

Consider $f[i][j]$:

-   If $j = 0$, then $f[i][j] = f[i - 1][j] + grid[i][j]$;
-   If $i = 0$, then $f[i][j] = f[i][j - 1] + grid[i][j]$;
-   If $i > 0$ and $j > 0$, then $f[i][j] = \min(f[i - 1][j], f[i][j - 1]) + grid[i][j]$.

Finally, return $f[m - 1][n - 1]$.

The time complexity is $O(m \times n)$, and the space complexity is $O(m \times n)$. Here, $m$ and $n$ are the number of rows and columns of the grid, respectively.

#### Java

```java
class Solution {
    public int minPathSum(int[][] grid) {
        int m = grid.length, n = grid[0].length;
        int[][] f = new int[m][n];
        f[0][0] = grid[0][0];
        for (int i = 1; i < m; ++i) {
            f[i][0] = f[i - 1][0] + grid[i][0];
        }
        for (int j = 1; j < n; ++j) {
            f[0][j] = f[0][j - 1] + grid[0][j];
        }
        for (int i = 1; i < m; ++i) {
            for (int j = 1; j < n; ++j) {
                f[i][j] = Math.min(f[i - 1][j], f[i][j - 1]) + grid[i][j];
            }
        }
        return f[m - 1][n - 1];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        int f[m][n];
        f[0][0] = grid[0][0];
        for (int i = 1; i < m; ++i) {
            f[i][0] = f[i - 1][0] + grid[i][0];
        }
        for (int j = 1; j < n; ++j) {
            f[0][j] = f[0][j - 1] + grid[0][j];
        }
        for (int i = 1; i < m; ++i) {
            for (int j = 1; j < n; ++j) {
                f[i][j] = min(f[i - 1][j], f[i][j - 1]) + grid[i][j];
            }
        }
        return f[m - 1][n - 1];
    }
};
```
#### JavaScript

```js
/**
 * @param {number[][]} grid
 * @return {number}
 */
var minPathSum = function (grid) {
    const m = grid.length;
    const n = grid[0].length;
    const f = Array(m)
        .fill(0)
        .map(() => Array(n).fill(0));
    f[0][0] = grid[0][0];
    for (let i = 1; i < m; ++i) {
        f[i][0] = f[i - 1][0] + grid[i][0];
    }
    for (let j = 1; j < n; ++j) {
        f[0][j] = f[0][j - 1] + grid[0][j];
    }
    for (let i = 1; i < m; ++i) {
        for (let j = 1; j < n; ++j) {
            f[i][j] = Math.min(f[i - 1][j], f[i][j - 1]) + grid[i][j];
        }
    }
    return f[m - 1][n - 1];
};
```

<!-- solution:end -->

<!-- problem:end -->