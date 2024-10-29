
# [5. Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring)


## Description


<p>Given a string <code>s</code>, return <em>the longest</em> <span data-keyword="palindromic-string"><em>palindromic</em></span> <span data-keyword="substring-nonempty"><em>substring</em></span> in <code>s</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;babad&quot;
<strong>Output:</strong> &quot;bab&quot;
<strong>Explanation:</strong> &quot;aba&quot; is also a valid answer.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;cbbd&quot;
<strong>Output:</strong> &quot;bb&quot;
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 1000</code></li>
	<li><code>s</code> consist of only digits and English letters.</li>
</ul>


## Solutions


### Solution 1: Dynamic Programming

We define $f[i][j]$ to represent whether the string $s[i..j]$ is a palindrome, initially $f[i][j] = true$.

Next, we define variables $k$ and $mx$, where $k$ represents the starting position of the longest palindrome, and $mx$ represents the length of the longest palindrome. Initially, $k = 0$, $mx = 1$.

Considering $f[i][j]$, if $s[i] = s[j]$, then $f[i][j] = f[i + 1][j - 1]$; otherwise, $f[i][j] = false$. If $f[i][j] = true$ and $mx < j - i + 1$, then we update $k = i$, $mx = j - i + 1$.

Since $f[i][j]$ depends on $f[i + 1][j - 1]$, we need to ensure that $i + 1$ is before $j - 1$, so we need to enumerate $i$ from large to small, and enumerate $j$ from small to large.

The time complexity is $O(n^2)$, and the space complexity is $O(n^2)$. Here, $n$ is the length of the string $s$.

<!-- tabs:start -->


#### Java

```java
class Solution {
    public String longestPalindrome(String s) {
        int n = s.length();
        boolean[][] f = new boolean[n][n];
        for (var g : f) {
            Arrays.fill(g, true);
        }
        int k = 0, mx = 1;
        for (int i = n - 2; i >= 0; --i) {
            for (int j = i + 1; j < n; ++j) {
                f[i][j] = false;
                if (s.charAt(i) == s.charAt(j)) {
                    f[i][j] = f[i + 1][j - 1];
                    if (f[i][j] && mx < j - i + 1) {
                        mx = j - i + 1;
                        k = i;
                    }
                }
            }
        }
        return s.substring(k, k + mx);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        int n = s.size();
        vector<vector<bool>> f(n, vector<bool>(n, true));
        int k = 0, mx = 1;
        for (int i = n - 2; ~i; --i) {
            for (int j = i + 1; j < n; ++j) {
                f[i][j] = false;
                if (s[i] == s[j]) {
                    f[i][j] = f[i + 1][j - 1];
                    if (f[i][j] && mx < j - i + 1) {
                        mx = j - i + 1;
                        k = i;
                    }
                }
            }
        }
        return s.substr(k, mx);
    }
};
```


#### JavaScript

```js
/**
 * @param {string} s
 * @return {string}
 */
var longestPalindrome = function (s) {
    const n = s.length;
    const f = Array(n)
        .fill(0)
        .map(() => Array(n).fill(true));
    let k = 0;
    let mx = 1;
    for (let i = n - 2; i >= 0; --i) {
        for (let j = i + 1; j < n; ++j) {
            f[i][j] = false;
            if (s[i] === s[j]) {
                f[i][j] = f[i + 1][j - 1];
                if (f[i][j] && mx < j - i + 1) {
                    mx = j - i + 1;
                    k = i;
                }
            }
        }
    }
    return s.slice(k, k + mx);
};
```


### Solution 2: Enumerate Palindrome Midpoint

We can enumerate the midpoint of the palindrome, spread to both sides, and find the longest palindrome.

The time complexity is $O(n^2)$, and the space complexity is $O(1)$. Here, $n$ is the length of the string $s$.



#### Java

```java
class Solution {
    private String s;
    private int n;

    public String longestPalindrome(String s) {
        this.s = s;
        n = s.length();
        int start = 0, mx = 1;
        for (int i = 0; i < n; ++i) {
            int a = f(i, i);
            int b = f(i, i + 1);
            int t = Math.max(a, b);
            if (mx < t) {
                mx = t;
                start = i - ((t - 1) >> 1);
            }
        }
        return s.substring(start, start + mx);
    }

    private int f(int l, int r) {
        while (l >= 0 && r < n && s.charAt(l) == s.charAt(r)) {
            --l;
            ++r;
        }
        return r - l - 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        int n = s.size();
        int start = 0, mx = 1;
        auto f = [&](int l, int r) {
            while (l >= 0 && r < n && s[l] == s[r]) {
                l--, r++;
            }
            return r - l - 1;
        };
        for (int i = 0; i < n; ++i) {
            int a = f(i, i);
            int b = f(i, i + 1);
            int t = max(a, b);
            if (mx < t) {
                mx = t;
                start = i - (t - 1 >> 1);
            }
        }
        return s.substr(start, mx);
    }
};
```

