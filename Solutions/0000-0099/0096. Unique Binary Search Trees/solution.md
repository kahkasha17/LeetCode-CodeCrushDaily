# [96. Unique Binary Search Trees](https://leetcode.com/problems/unique-binary-search-trees)

## Description

<!-- description:start -->

<p>Given an integer <code>n</code>, return <em>the number of structurally unique <strong>BST&#39;</strong>s (binary search trees) which has exactly </em><code>n</code><em> nodes of unique values from</em> <code>1</code> <em>to</em> <code>n</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/0000-0099/0096.Unique%20Binary%20Search%20Trees/images/uniquebstn3.jpg" style="width: 600px; height: 148px;" />
<pre>
<strong>Input:</strong> n = 3
<strong>Output:</strong> 5
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> n = 1
<strong>Output:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 19</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Dynamic Programming

We define $f[i]$ to represent the number of binary search trees that can be generated from $[1, i]$. Initially, $f[0] = 1$, and the answer is $f[n]$.

We can enumerate the number of nodes $i$, then the number of nodes in the left subtree $j \in [0, i - 1]$, and the number of nodes in the right subtree $k = i - j - 1$. The number of combinations of the number of nodes in the left subtree and the right subtree is $f[j] \times f[k]$, so $f[i] = \sum_{j = 0}^{i - 1} f[j] \times f[i - j - 1]$.

Finally, return $f[n]$.

The time complexity is $O(n)$, and the space complexity is $O(n)$. Here, $n$ is the number of nodes.

#### Java

```java
class Solution {
    public int numTrees(int n) {
        int[] f = new int[n + 1];
        f[0] = 1;
        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j < i; ++j) {
                f[i] += f[j] * f[i - j - 1];
            }
        }
        return f[n];
    }
}
```

#### C++

```cpp
class Solution {
public:
    int numTrees(int n) {
        vector<int> f(n + 1);
        f[0] = 1;
        for (int i = 1; i <= n; ++i) {
            for (int j = 0; j < i; ++j) {
                f[i] += f[j] * f[i - j - 1];
            }
        }
        return f[n];
    }
};
```
<!-- solution:end -->

<!-- problem:end -->