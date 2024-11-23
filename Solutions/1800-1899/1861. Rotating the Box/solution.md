# [1861. Rotating the Box](https://leetcode.com/problems/rotating-the-box)

## Description

<!-- description:start -->

<p>You are given an <code>m x n</code> matrix of characters <code>box</code> representing a side-view of a box. Each cell of the box is one of the following:</p>

<ul>

    <li>A stone <code>&#39;#&#39;</code></li>

    <li>A stationary obstacle <code>&#39;*&#39;</code></li>

    <li>Empty <code>&#39;.&#39;</code></li>

</ul>

<p>The box is rotated <strong>90 degrees clockwise</strong>, causing some of the stones to fall due to gravity. Each stone falls down until it lands on an obstacle, another stone, or the bottom of the box. Gravity <strong>does not</strong> affect the obstacles&#39; positions, and the inertia from the box&#39;s rotation <strong>does not </strong>affect the stones&#39; horizontal positions.</p>

<p>It is <strong>guaranteed</strong> that each stone in <code>box</code> rests on an obstacle, another stone, or the bottom of the box.</p>

<p>Return <em>an </em><code>n x m</code><em> matrix representing the box after the rotation described above</em>.</p>

<p>&nbsp;</p>

<p><strong class="example">Example 1:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1800-1899/1861.Rotating%20the%20Box/images/rotatingtheboxleetcodewithstones.png" style="width: 300px; height: 150px;" /></p>

<pre>

<strong>Input:</strong> box = [[&quot;#&quot;,&quot;.&quot;,&quot;#&quot;]]

<strong>Output:</strong> [[&quot;.&quot;],

&nbsp;        [&quot;#&quot;],

&nbsp;        [&quot;#&quot;]]

</pre>

<p><strong class="example">Example 2:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1800-1899/1861.Rotating%20the%20Box/images/rotatingtheboxleetcode2withstones.png" style="width: 375px; height: 195px;" /></p>

<pre>

<strong>Input:</strong> box = [[&quot;#&quot;,&quot;.&quot;,&quot;*&quot;,&quot;.&quot;],

&nbsp;             [&quot;#&quot;,&quot;#&quot;,&quot;*&quot;,&quot;.&quot;]]

<strong>Output:</strong> [[&quot;#&quot;,&quot;.&quot;],

&nbsp;        [&quot;#&quot;,&quot;#&quot;],

&nbsp;        [&quot;*&quot;,&quot;*&quot;],

&nbsp;        [&quot;.&quot;,&quot;.&quot;]]

</pre>

<p><strong class="example">Example 3:</strong></p>

<p><img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/1800-1899/1861.Rotating%20the%20Box/images/rotatingtheboxleetcode3withstone.png" style="width: 400px; height: 218px;" /></p>

<pre>

<strong>Input:</strong> box = [[&quot;#&quot;,&quot;#&quot;,&quot;*&quot;,&quot;.&quot;,&quot;*&quot;,&quot;.&quot;],

&nbsp;             [&quot;#&quot;,&quot;#&quot;,&quot;#&quot;,&quot;*&quot;,&quot;.&quot;,&quot;.&quot;],

&nbsp;             [&quot;#&quot;,&quot;#&quot;,&quot;#&quot;,&quot;.&quot;,&quot;#&quot;,&quot;.&quot;]]

<strong>Output:</strong> [[&quot;.&quot;,&quot;#&quot;,&quot;#&quot;],

&nbsp;        [&quot;.&quot;,&quot;#&quot;,&quot;#&quot;],

&nbsp;        [&quot;#&quot;,&quot;#&quot;,&quot;*&quot;],

&nbsp;        [&quot;#&quot;,&quot;*&quot;,&quot;.&quot;],

&nbsp;        [&quot;#&quot;,&quot;.&quot;,&quot;*&quot;],

&nbsp;        [&quot;#&quot;,&quot;.&quot;,&quot;.&quot;]]

</pre>

<p>&nbsp;</p>

<p><strong>Constraints:</strong></p>

<ul>

    <li><code>m == box.length</code></li>

    <li><code>n == box[i].length</code></li>

    <li><code>1 &lt;= m, n &lt;= 500</code></li>

    <li><code>box[i][j]</code> is either <code>&#39;#&#39;</code>, <code>&#39;*&#39;</code>, or <code>&#39;.&#39;</code>.</li>

</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Queue Simulation

First, we rotate the matrix 90 degrees clockwise, then simulate the falling process of the stones in each column.

The time complexity is $O(m \times n)$, and the space complexity is $O(m \times n)$. Where $m$ and $n$ are the number of rows and columns of the matrix, respectively.

#### Java

```java
class Solution {
    public char[][] rotateTheBox(char[][] box) {
        int m = box.length, n = box[0].length;
        char[][] ans = new char[n][m];
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                ans[j][m - i - 1] = box[i][j];
            }
        }
        for (int j = 0; j < m; ++j) {
            Deque<Integer> q = new ArrayDeque<>();
            for (int i = n - 1; i >= 0; --i) {
                if (ans[i][j] == '*') {
                    q.clear();
                } else if (ans[i][j] == '.') {
                    q.offer(i);
                } else if (!q.isEmpty()) {
                    ans[q.pollFirst()][j] = '#';
                    ans[i][j] = '.';
                    q.offer(i);
                }
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
    vector<vector<char>> rotateTheBox(vector<vector<char>>& box) {
        int m = box.size(), n = box[0].size();
        vector<vector<char>> ans(n, vector<char>(m));
        for (int i = 0; i < m; ++i) {
            for (int j = 0; j < n; ++j) {
                ans[j][m - i - 1] = box[i][j];
            }
        }
        for (int j = 0; j < m; ++j) {
            queue<int> q;
            for (int i = n - 1; ~i; --i) {
                if (ans[i][j] == '*') {
                    queue<int> t;
                    swap(t, q);
                } else if (ans[i][j] == '.') {
                    q.push(i);
                } else if (!q.empty()) {
                    ans[q.front()][j] = '#';
                    q.pop();
                    ans[i][j] = '.';
                    q.push(i);
                }
            }
        }
        return ans;
    }
};
```

<!-- solution:end -->

<!-- problem:end -->