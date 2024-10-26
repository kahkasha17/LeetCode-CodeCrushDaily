# [2405. Optimal Partition of String](https://leetcode.com/problems/optimal-partition-of-string)


## Description

<!-- description:start -->

<p>Given a string <code>s</code>, partition the string into one or more <strong>substrings</strong> such that the characters in each substring are <strong>unique</strong>. That is, no letter appears in a single substring more than <strong>once</strong>.</p>

<p>Return <em>the <strong>minimum</strong> number of substrings in such a partition.</em></p>

<p>Note that each character should belong to exactly one substring in a partition.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;abacaba&quot;
<strong>Output:</strong> 4
<strong>Explanation:</strong>
Two possible partitions are (&quot;a&quot;,&quot;ba&quot;,&quot;cab&quot;,&quot;a&quot;) and (&quot;ab&quot;,&quot;a&quot;,&quot;ca&quot;,&quot;ba&quot;).
It can be shown that 4 is the minimum number of substrings needed.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;ssssss&quot;
<strong>Output:</strong> 6
<strong>Explanation:
</strong>The only valid partition is (&quot;s&quot;,&quot;s&quot;,&quot;s&quot;,&quot;s&quot;,&quot;s&quot;,&quot;s&quot;).
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= s.length &lt;= 10<sup>5</sup></code></li>
	<li><code>s</code> consists of only English lowercase letters.</li>
</ul>

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Greedy

According to the problem description, each substring should be as long as possible and contain unique characters. Therefore, we can greedily partition the string.

We define a binary integer $\textit{mask}$ to record the characters that have appeared in the current substring. The $i$-th bit of $\textit{mask}$ being $1$ indicates that the $i$-th letter has already appeared, and $0$ indicates that it has not appeared. Additionally, we need a variable $\textit{ans}$ to record the number of substrings, initially $\textit{ans} = 1$.

Traverse each character in the string $s$. For each character $c$, convert it to an integer $x$ between $0$ and $25$, then check if the $x$-th bit of $\textit{mask}$ is $1$. If it is $1$, it means the current character $c$ is a duplicate in the current substring. In this case, increment $\textit{ans}$ by $1$ and reset $\textit{mask}$ to $0$. Otherwise, set the $x$-th bit of $\textit{mask}$ to $1$. Then, update $\textit{mask}$ to the bitwise OR result of $\textit{mask}$ and $2^x$.

Finally, return $\textit{ans}$.

The time complexity is $O(n)$, where $n$ is the length of the string $s$. The space complexity is $O(1)$.


#### Java

```java
class Solution {
    public int partitionString(String s) {
        int ans = 1, mask = 0;
        for (int i = 0; i < s.length(); ++i) {
            int x = s.charAt(i) - 'a';
            if ((mask >> x & 1) == 1) {
                ++ans;
                mask = 0;
            }
            mask |= 1 << x;
        }
        return ans;
    }
}
```

#### C++

```cpp
class Solution {
public:
    int partitionString(string s) {
        int ans = 1, mask = 0;
        for (char& c : s) {
            int x = c - 'a';
            if (mask >> x & 1) {
                ++ans;
                mask = 0;
            }
            mask |= 1 << x;
        }
        return ans;
    }
};
```

