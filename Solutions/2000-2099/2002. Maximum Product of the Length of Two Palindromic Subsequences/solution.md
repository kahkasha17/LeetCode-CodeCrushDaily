# [2002. Maximum Product of the Length of Two Palindromic Subsequences](https://leetcode.com/problems/maximum-product-of-the-length-of-two-palindromic-subsequences)

## Description

<!-- description:start -->

<p>Given a string <code>s</code>, find two <strong>disjoint palindromic subsequences</strong> of <code>s</code> such that the <strong>product</strong> of their lengths is <strong>maximized</strong>. The two subsequences are <strong>disjoint</strong> if they do not both pick a character at the same index.</p>

<p>Return <em>the <strong>maximum</strong> possible <strong>product</strong> of the lengths of the two palindromic subsequences</em>.</p>

<p>A <strong>subsequence</strong> is a string that can be derived from another string by deleting some or no characters without changing the order of the remaining characters. A string is <strong>palindromic</strong> if it reads the same forward and backward.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>
<img alt="example-1" src="https://fastly.jsdelivr.net/gh/doocs/leetcode@main/solution/2000-2099/2002.Maximum%20Product%20of%20the%20Length%20of%20Two%20Palindromic%20Subsequences/images/two-palindromic-subsequences.png" style="width: 550px; height: 124px;" />
<pre>
<strong>Input:</strong> s = &quot;leetcodecom&quot;
<strong>Output:</strong> 9
<strong>Explanation</strong>: An optimal solution is to choose &quot;ete&quot; for the 1<sup>st</sup> subsequence and &quot;cdc&quot; for the 2<sup>nd</sup> subsequence.
The product of their lengths is: 3 * 3 = 9.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;bb&quot;
<strong>Output:</strong> 1
<strong>Explanation</strong>: An optimal solution is to choose &quot;b&quot; (the first character) for the 1<sup>st</sup> subsequence and &quot;b&quot; (the second character) for the 2<sup>nd</sup> subsequence.
The product of their lengths is: 1 * 1 = 1.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;accbcaxxcxx&quot;
<strong>Output:</strong> 25
<strong>Explanation</strong>: An optimal solution is to choose &quot;accca&quot; for the 1<sup>st</sup> subsequence and &quot;xxcxx&quot; for the 2<sup>nd</sup> subsequence.
The product of their lengths is: 5 * 5 = 25.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>2 &lt;= s.length &lt;= 12</code></li>
	<li><code>s</code> consists of lowercase English letters only.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Binary Enumeration

We notice that the length of the string $s$ does not exceed $12$, so we can use the method of binary enumeration to enumerate all subsequences of $s$. Suppose the length of $s$ is $n$, we can use $2^n$ binary numbers of length $n$ to represent all subsequences of $s$. For each binary number, the $i$-th bit being $1$ means the $i$-th character of $s$ is in the subsequence, and $0$ means it is not in the subsequence. For each binary number, we judge whether it is a palindrome subsequence and record it in the array $p$.

Next, we enumerate each number $i$ in $p$. If $i$ is a palindrome subsequence, then we can enumerate a number $j$ from the complement of $i$, $mx = (2^n - 1) \oplus i$. If $j$ is also a palindrome subsequence, then $i$ and $j$ are the two palindrome subsequences we are looking for. Their lengths are the number of $1$s in the binary representation of $i$ and $j$, denoted as $a$ and $b$, respectively. Then their product is $a \times b$. We take the maximum of all possible $a \times b$.

The time complexity is $(2^n \times n + 3^n)$, and the space complexity is $O(2^n)$. Here, $n$ is the length of the string $s$.

#### Java

```java
class Solution {
    public int maxProduct(String s) {
        int n = s.length();
        boolean[] p = new boolean[1 << n];
        Arrays.fill(p, true);
        for (int k = 1; k < 1 << n; ++k) {
            for (int i = 0, j = n - 1; i < n; ++i, --j) {
                while (i < j && (k >> i & 1) == 0) {
                    ++i;
                }
                while (i < j && (k >> j & 1) == 0) {
                    --j;
                }
                if (i < j && s.charAt(i) != s.charAt(j)) {
                    p[k] = false;
                    break;
                }
            }
        }
        int ans = 0;
        for (int i = 1; i < 1 << n; ++i) {
            if (p[i]) {
                int a = Integer.bitCount(i);
                int mx = ((1 << n) - 1) ^ i;
                for (int j = mx; j > 0; j = (j - 1) & mx) {
                    if (p[j]) {
                        int b = Integer.bitCount(j);
                        ans = Math.max(ans, a * b);
                    }
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
    int maxProduct(string s) {
        int n = s.size();
        vector<bool> p(1 << n, true);
        for (int k = 1; k < 1 << n; ++k) {
            for (int i = 0, j = n - 1; i < j; ++i, --j) {
                while (i < j && !(k >> i & 1)) {
                    ++i;
                }
                while (i < j && !(k >> j & 1)) {
                    --j;
                }
                if (i < j && s[i] != s[j]) {
                    p[k] = false;
                    break;
                }
            }
        }
        int ans = 0;
        for (int i = 1; i < 1 << n; ++i) {
            if (p[i]) {
                int a = __builtin_popcount(i);
                int mx = ((1 << n) - 1) ^ i;
                for (int j = mx; j; j = (j - 1) & mx) {
                    if (p[j]) {
                        int b = __builtin_popcount(j);
                        ans = max(ans, a * b);
                    }
                }
            }
        }
        return ans;
    }
};
```


<!-- solution:end -->

<!-- problem:end -->