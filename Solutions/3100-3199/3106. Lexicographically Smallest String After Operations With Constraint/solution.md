# [3106. Lexicographically Smallest String After Operations With Constraint](https://leetcode.com/problems/lexicographically-smallest-string-after-operations-with-constraint)

## Description

<!-- description:start -->

<p>You are given a string <code>s</code> and an integer <code>k</code>.</p>

<p>Define a function <code>distance(s<sub>1</sub>, s<sub>2</sub>)</code> between two strings <code>s<sub>1</sub></code> and <code>s<sub>2</sub></code> of the same length <code>n</code> as:</p>

<ul>
	<li>The<strong> sum</strong> of the <strong>minimum distance</strong> between <code>s<sub>1</sub>[i]</code> and <code>s<sub>2</sub>[i]</code> when the characters from <code>&#39;a&#39;</code> to <code>&#39;z&#39;</code> are placed in a <strong>cyclic</strong> order, for all <code>i</code> in the range <code>[0, n - 1]</code>.</li>
</ul>

<p>For example, <code>distance(&quot;ab&quot;, &quot;cd&quot;) == 4</code>, and <code>distance(&quot;a&quot;, &quot;z&quot;) == 1</code>.</p>

<p>You can <strong>change</strong> any letter of <code>s</code> to <strong>any</strong> other lowercase English letter, <strong>any</strong> number of times.</p>

<p>Return a string denoting the <strong><span data-keyword="lexicographically-smaller-string">lexicographically smallest</span></strong> string <code>t</code> you can get after some changes, such that <code>distance(s, t) &lt;= k</code>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">s = &quot;zbbz&quot;, k = 3</span></p>

<p><strong>Output:</strong> <span class="example-io">&quot;aaaz&quot;</span></p>

<p><strong>Explanation:</strong></p>

<p>Change <code>s</code> to <code>&quot;aaaz&quot;</code>. The distance between <code>&quot;zbbz&quot;</code> and <code>&quot;aaaz&quot;</code> is equal to <code>k = 3</code>.</p>
</div>

<p><strong class="example">Example 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">s = &quot;xaxcd&quot;, k = 4</span></p>

<p><strong>Output:</strong> <span class="example-io">&quot;aawcd&quot;</span></p>

<p><strong>Explanation:</strong></p>

<p>The distance between &quot;xaxcd&quot; and &quot;aawcd&quot; is equal to k = 4.</p>
</div>

<p><strong class="example">Example 3:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">s = &quot;lol&quot;, k = 0</span></p>

<p><strong>Output:</strong> <span class="example-io">&quot;lol&quot;</span></p>

<p><strong>Explanation:</strong></p>

<p>It&#39;s impossible to change any character as <code>k = 0</code>.</p>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 100</code></li>
	<li><code>0 &lt;= k &lt;= 2000</code></li>
	<li><code>s</code> consists only of lowercase English letters.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Enumeration

We can traverse each position of the string $s$. For each position, we enumerate all characters less than the current character, calculate the cost $d$ to change to this character. If $d \leq k$, we change the current character to this character, subtract $d$ from $k$, end the enumeration, and continue to the next position.

After the traversal, we get a string that meets the conditions.

The time complexity is $O(n \times |\Sigma|)$, and the space complexity is $O(n)$. Here, $n$ is the length of the string $s$, and $|\Sigma|$ is the size of the character set. In this problem, $|\Sigma| \leq 26$.

#### Java

```java
class Solution {
    public String getSmallestString(String s, int k) {
        char[] cs = s.toCharArray();
        for (int i = 0; i < cs.length; ++i) {
            char c1 = cs[i];
            for (char c2 = 'a'; c2 < c1; ++c2) {
                int d = Math.min(c1 - c2, 26 - c1 + c2);
                if (d <= k) {
                    cs[i] = c2;
                    k -= d;
                    break;
                }
            }
        }
        return new String(cs);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string getSmallestString(string s, int k) {
        for (int i = 0; i < s.size(); ++i) {
            char c1 = s[i];
            for (char c2 = 'a'; c2 < c1; ++c2) {
                int d = min(c1 - c2, 26 - c1 + c2);
                if (d <= k) {
                    s[i] = c2;
                    k -= d;
                    break;
                }
            }
        }
        return s;
    }
};
```
<!-- solution:end -->

<!-- problem:end -->