# [59. Spiral Matrix II](https://leetcode.com/problems/spiral-matrix-ii)

## Description

<!-- description:start -->

<p>Given a positive integer <code>n</code>, generate an <code>n x n</code> <code>matrix</code> filled with elements from <code>1</code> to <code>n<sup>2</sup></code> in spiral order.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0000-0099/0059.Spiral%20Matrix%20II/images/spiraln.jpg" style="width: 242px; height: 242px;" />
<pre>
<strong>Input:</strong> n = 3
<strong>Output:</strong> [[1,2,3],[8,9,4],[7,6,5]]
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> n = 1
<strong>Output:</strong> [[1]]
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 20</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Simulation

Directly simulate the generation process of the spiral matrix.

Define a two-dimensional array `ans` to store the spiral matrix. Use `i` and `j` to represent the row number and column number of the current position, use `k` to represent the current direction number, and `dirs` to represent the correspondence between the direction number and the direction.

Starting from `1`, fill in each position of the matrix in turn. After filling in a position each time, calculate the row number and column number of the next position. If the next position is not in the matrix or has been filled, change the direction, and then calculate the row number and column number of the next position.

The time complexity is $O(n^2)$, where $n$ is the side length of the matrix. Ignoring the output array, the space complexity is $O(1)$.

#### Java

```java
class Solution {
    public int[][] generateMatrix(int n) {
        int[][] ans = new int[n][n];
        int i = 0, j = 0, k = 0;
        int[][] dirs = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
        for (int v = 1; v <= n * n; ++v) {
            ans[i][j] = v;
            int x = i + dirs[k][0], y = j + dirs[k][1];
            if (x < 0 || y < 0 || x >= n || y >= n || ans[x][y] > 0) {
                k = (k + 1) % 4;
                x = i + dirs[k][0];
                y = j + dirs[k][1];
            }
            i = x;
            j = y;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    const int dirs[4][2] = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};

    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> ans(n, vector<int>(n));
        int i = 0, j = 0, k = 0;
        for (int v = 1; v <= n * n; ++v) {
            ans[i][j] = v;
            int x = i + dirs[k][0], y = j + dirs[k][1];
            if (x < 0 || y < 0 || x >= n || y >= n || ans[x][y]) {
                k = (k + 1) % 4;
                x = i + dirs[k][0], y = j + dirs[k][1];
            }
            i = x, j = y;
        }
        return ans;
    }
};
```
#### JavaScript

```js
/**
 * @param {number} n
 * @return {number[][]}
 */
var generateMatrix = function (n) {
    const ans = new Array(n).fill(0).map(() => new Array(n).fill(0));
    let [i, j, k] = [0, 0, 0];
    const dirs = [
        [0, 1],
        [1, 0],
        [0, -1],
        [-1, 0],
    ];
    for (let v = 1; v <= n * n; ++v) {
        ans[i][j] = v;
        let [x, y] = [i + dirs[k][0], j + dirs[k][1]];
        if (x < 0 || y < 0 || x >= n || y >= n || ans[x][y] > 0) {
            k = (k + 1) % 4;
            [x, y] = [i + dirs[k][0], j + dirs[k][1]];
        }
        [i, j] = [x, y];
    }
    return ans;
};
```

<!-- solution:end -->
<!-- problem:end -->