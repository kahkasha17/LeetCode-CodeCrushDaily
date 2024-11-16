# [161. One Edit Distance 🔒](https://leetcode.com/problems/one-edit-distance)

## Description

<!-- description:start -->

<p>Given two strings <code>s</code> and <code>t</code>, return <code>true</code> if they are both one edit distance apart, otherwise return <code>false</code>.</p>

<p>A string <code>s</code> is said to be one distance apart from a string <code>t</code> if you can:</p>

<ul>
	<li>Insert <strong>exactly one</strong> character into <code>s</code> to get <code>t</code>.</li>
	<li>Delete <strong>exactly one</strong> character from <code>s</code> to get <code>t</code>.</li>
	<li>Replace <strong>exactly one</strong> character of <code>s</code> with <strong>a different character</strong> to get <code>t</code>.</li>
</ul>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;ab&quot;, t = &quot;acb&quot;
<strong>Output:</strong> true
<strong>Explanation:</strong> We can insert &#39;c&#39; into s&nbsp;to get&nbsp;t.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;&quot;, t = &quot;&quot;
<strong>Output:</strong> false
<strong>Explanation:</strong> We cannot get t from s by only one step.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>0 &lt;= s.length, t.length &lt;= 10<sup>4</sup></code></li>
	<li><code>s</code> and <code>t</code> consist of lowercase letters, uppercase letters, and digits.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Discuss Different Cases

Let $m$ represent the length of string $s$, and $n$ represent the length of string $t$. We can assume that $m$ is always greater than or equal to $n$.

If $m-n > 1$, return false directly;

Otherwise, iterate through $s$ and $t$, if $s[i]$ is not equal to $t[i]$:

-   If $m \neq n$, compare $s[i+1:]$ with $t[i:]$, return true if they are equal, otherwise return false;
-   If $m = n$, compare $s[i:]$ with $t[i:]$, return true if they are equal, otherwise return false.

If the iteration ends, it means that all the characters of $s$ and $t$ that have been iterated are equal, at this time it needs to satisfy $m=n+1$.

The time complexity is $O(m)$, where $m$ is the length of string $s$. The space complexity is $O(1)$.

#### Java

```java
class Solution {
    public boolean isOneEditDistance(String s, String t) {
        int m = s.length(), n = t.length();
        if (m < n) {
            return isOneEditDistance(t, s);
        }
        if (m - n > 1) {
            return false;
        }
        for (int i = 0; i < n; ++i) {
            if (s.charAt(i) != t.charAt(i)) {
                if (m == n) {
                    return s.substring(i + 1).equals(t.substring(i + 1));
                }
                return s.substring(i + 1).equals(t.substring(i));
            }
        }
        return m == n + 1;
    }
}
```

#### C++

```cpp
class Solution {
public:
    bool isOneEditDistance(string s, string t) {
        int m = s.size(), n = t.size();
        if (m < n) return isOneEditDistance(t, s);
        if (m - n > 1) return false;
        for (int i = 0; i < n; ++i) {
            if (s[i] != t[i]) {
                if (m == n) return s.substr(i + 1) == t.substr(i + 1);
                return s.substr(i + 1) == t.substr(i);
            }
        }
        return m == n + 1;
    }
};
```

<!-- solution:end -->

<!-- problem:end -->