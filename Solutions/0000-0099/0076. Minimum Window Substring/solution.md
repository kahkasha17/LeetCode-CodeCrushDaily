# [76. Minimum Window Substring](https://leetcode.com/problems/minimum-window-substring)

## Description

<!-- description:start -->

<p>Given two strings <code>s</code> and <code>t</code> of lengths <code>m</code> and <code>n</code> respectively, return <em>the <strong>minimum window</strong></em> <span data-keyword="substring-nonempty"><strong><em>substring</em></strong></span><em> of </em><code>s</code><em> such that every character in </em><code>t</code><em> (<strong>including duplicates</strong>) is included in the window</em>. If there is no such substring, return <em>the empty string </em><code>&quot;&quot;</code>.</p>

<p>The testcases will be generated such that the answer is <strong>unique</strong>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;ADOBECODEBANC&quot;, t = &quot;ABC&quot;
<strong>Output:</strong> &quot;BANC&quot;
<strong>Explanation:</strong> The minimum window substring &quot;BANC&quot; includes &#39;A&#39;, &#39;B&#39;, and &#39;C&#39; from string t.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;a&quot;, t = &quot;a&quot;
<strong>Output:</strong> &quot;a&quot;
<strong>Explanation:</strong> The entire string s is the minimum window.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;a&quot;, t = &quot;aa&quot;
<strong>Output:</strong> &quot;&quot;
<strong>Explanation:</strong> Both &#39;a&#39;s from t must be included in the window.
Since the largest window of s only has one &#39;a&#39;, return empty string.
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>m == s.length</code></li>
	<li><code>n == t.length</code></li>
	<li><code>1 &lt;= m, n &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> and <code>t</code> consist of uppercase and lowercase English letters.</li>
</ul>

<p>&nbsp;</p>
<p><strong>Follow up:</strong> Could you find an algorithm that runs in <code>O(m + n)</code> time?</p>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Counting + Two Pointers

We use a hash table or array $need$ to count the number of occurrences of each character in string $t$, and another hash table or array $window$ to count the number of occurrences of each character in the sliding window. In addition, we define two pointers $j$ and $i$ to point to the left and right boundaries of the window, respectively. The variable $cnt$ represents how many characters in $t$ are already included in the window. The variables $k$ and $mi$ represent the starting position and length of the minimum covering substring, respectively.

We traverse the string $s$ from left to right. For the currently traversed character $s[i]$:

We add it to the window, i.e., $window[s[i]] = window[s[i]] + 1$. If $need[s[i]] \geq window[s[i]]$ at this time, it means that $s[i]$ is a "necessary character", so we increment $cnt$ by one. If $cnt$ equals the length of $t$, it means that all characters in $t$ are already included in the window at this time, so we can try to update the starting position and length of the minimum covering substring. If $i - j + 1 \lt mi$, it means that the substring represented by the current window is shorter, so we update $mi = i - j + 1$ and $k = j$. Then, we try to move the left boundary $j$. If $need[s[j]] \geq window[s[j]]$ at this time, it means that $s[j]$ is a "necessary character". When moving the left boundary, the character $s[j]$ will be removed from the window, so we need to decrement $cnt$ by one, then update $window[s[j]] = window[s[j]] - 1$, and move $j$ one step to the right. If $cnt$ does not equal the length of $t$, it means that all characters in $t$ are not yet included in the window at this time, so we don't need to move the left boundary, just move $i$ one step to the right and continue to traverse.

After the traversal, if the minimum covering substring is not found, return an empty string, otherwise return $s[k:k+mi]$.

The time complexity is $O(m + n)$, and the space complexity is $O(C)$. Here, $m$ and $n$ are the lengths of strings $s$ and $t$ respectively; and $C$ is the size of the character set, in this problem $C = 128$.

#### Java

```java
class Solution {
    public String minWindow(String s, String t) {
        int[] need = new int[128];
        int[] window = new int[128];
        int m = s.length(), n = t.length();
        for (int i = 0; i < n; ++i) {
            ++need[t.charAt(i)];
        }
        int cnt = 0, j = 0, k = -1, mi = 1 << 30;
        for (int i = 0; i < m; ++i) {
            ++window[s.charAt(i)];
            if (need[s.charAt(i)] >= window[s.charAt(i)]) {
                ++cnt;
            }
            while (cnt == n) {
                if (i - j + 1 < mi) {
                    mi = i - j + 1;
                    k = j;
                }
                if (need[s.charAt(j)] >= window[s.charAt(j)]) {
                    --cnt;
                }
                --window[s.charAt(j++)];
            }
        }
        return k < 0 ? "" : s.substring(k, k + mi);
    }
}
```

#### C++

```cpp
class Solution {
public:
    string minWindow(string s, string t) {
        int need[128]{};
        int window[128]{};
        int m = s.size(), n = t.size();
        for (char& c : t) {
            ++need[c];
        }
        int cnt = 0, j = 0, k = -1, mi = 1 << 30;
        for (int i = 0; i < m; ++i) {
            ++window[s[i]];
            if (need[s[i]] >= window[s[i]]) {
                ++cnt;
            }
            while (cnt == n) {
                if (i - j + 1 < mi) {
                    mi = i - j + 1;
                    k = j;
                }
                if (need[s[j]] >= window[s[j]]) {
                    --cnt;
                }
                --window[s[j++]];
            }
        }
        return k < 0 ? "" : s.substr(k, mi);
    }
};
```

<!-- solution:end -->

<!-- problem:end -->