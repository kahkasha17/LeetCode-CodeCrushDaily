# [357. Count Numbers with Unique Digits](https://leetcode.com/problems/count-numbers-with-unique-digits)

## Description

<!-- description:start -->

<p>Given an integer <code>n</code>, return the count of all numbers with unique digits, <code>x</code>, where <code>0 &lt;= x &lt; 10<sup>n</sup></code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> n = 2
<strong>Output:</strong> 91
<strong>Explanation:</strong> The answer should be the total numbers in the range of 0 &le; x &lt; 100, excluding 11,22,33,44,55,66,77,88,99
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> n = 0
<strong>Output:</strong> 1
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>0 &lt;= n &lt;= 8</code></li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: State Compression + Digit DP

This problem essentially asks for the number of numbers in the given range $[l, ..r]$ that satisfy certain conditions. The conditions are related to the composition of the numbers rather than their size, so we can use the concept of Digit DP to solve it. In Digit DP, the size of the number has little impact on the complexity.

For the range $[l, ..r]$ problem, we generally convert it to the problem of $[1, ..r]$ and then subtract the result of $[1, ..l - 1]$, i.e.:

$$
ans = \sum_{i=1}^{r} ans_i -  \sum_{i=1}^{l-1} ans_i
$$

However, for this problem, we only need to find the value for the range $[1, ..10^n-1]$.

Here, we use memoized search to implement Digit DP. We search from the starting point downwards, and at the lowest level, we get the number of solutions. We then return the answers layer by layer upwards, and finally get the final answer from the starting point of the search.

Based on the problem information, we design a function $\textit{dfs}(i, \textit{mask}, \textit{lead})$, where:

-   The digit $i$ represents the current position being searched, starting from the highest digit, i.e., $i = 0$ represents the highest digit.
-   The digit $\textit{mask}$ represents the current state of the number, i.e., the $j$-th bit of $\textit{mask}$ being $1$ indicates that the digit $j$ has been used.
-   The boolean $\textit{lead}$ indicates whether the current number only contains leading $0$s.

The function executes as follows:

If $i$ exceeds the length of the number $n$, i.e., $i < 0$, it means the search is over, directly return $1$.

Otherwise, we enumerate the digits $j$ from $0$ to $9$ for the position $i$. For each $j$:

-   If the $j$-th bit of $\textit{mask}$ is $1$, it means the digit $j$ has been used, so we skip it.
-   If $\textit{lead}$ is true and $j = 0$, it means the current number only contains leading $0$s. When we recurse to the next level, $\textit{lead}$ remains true.
-   Otherwise, we recurse to the next level, update the $j$-th bit of $\textit{mask}$ to $1$, and set $\textit{lead}$ to false.

Finally, we sum all the results from the recursive calls to the next level, which is the answer.

The answer is $\textit{dfs}(n - 1, 0, \textit{True})$.

The time complexity is $O(n \times 2^D \times D)$, and the space complexity is $O(n \times 2^D)$. Here, $n$ is the length of the number $n$, and $D = 10$.

#### Python3

```python
class Solution:
    def countNumbersWithUniqueDigits(self, n: int) -> int:
        @cache
        def dfs(i: int, mask: int, lead: bool) -> int:
            if i < 0:
                return 1
            ans = 0
            for j in range(10):
                if mask >> j & 1:
                    continue
                if lead and j == 0:
                    ans += dfs(i - 1, mask, True)
                else:
                    ans += dfs(i - 1, mask | 1 << j, False)
            return ans

        return dfs(n - 1, 0, True)
```

#### Java

```java
class Solution {
    private Integer[][] f;

    public int countNumbersWithUniqueDigits(int n) {
        f = new Integer[n][1 << 10];
        return dfs(n - 1, 0, true);
    }

    private int dfs(int i, int mask, boolean lead) {
        if (i < 0) {
            return 1;
        }
        if (!lead && f[i][mask] != null) {
            return f[i][mask];
        }
        int ans = 0;
        for (int j = 0; j <= 9; ++j) {
            if ((mask >> j & 1) == 1) {
                continue;
            }
            if (lead && j == 0) {
                ans += dfs(i - 1, mask, true);
            } else {
                ans += dfs(i - 1, mask | 1 << j, false);
            }
        }
        if (!lead) {
            f[i][mask] = ans;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int countNumbersWithUniqueDigits(int n) {
        int f[n + 1][1 << 10];
        memset(f, -1, sizeof(f));
        auto dfs = [&](auto&& dfs, int i, int mask, bool lead) -> int {
            if (i < 0) {
                return 1;
            }
            if (!lead && f[i][mask] != -1) {
                return f[i][mask];
            }
            int ans = 0;
            for (int j = 0; j <= 9; ++j) {
                if (mask >> j & 1) {
                    continue;
                }
                if (lead && j == 0) {
                    ans += dfs(dfs, i - 1, mask, true);
                } else {
                    ans += dfs(dfs, i - 1, mask | 1 << i, false);
                }
            }
            if (!lead) {
                f[i][mask] = ans;
            }
            return ans;
        };
        return dfs(dfs, n - 1, 0, true);
    }
};
```

<!-- solution:end -->

<!-- problem:end -->