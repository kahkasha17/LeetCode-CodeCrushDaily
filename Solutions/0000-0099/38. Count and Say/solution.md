# [38. Count and Say](https://leetcode.com/problems/count-and-say)

## Description

<!-- description:start -->

<p>The <strong>count-and-say</strong> sequence is a sequence of digit strings defined by the recursive formula:</p>

<ul>
	<li><code>countAndSay(1) = &quot;1&quot;</code></li>
	<li><code>countAndSay(n)</code> is the run-length encoding of <code>countAndSay(n - 1)</code>.</li>
</ul>

<p><a href="http://en.wikipedia.org/wiki/Run-length_encoding" target="_blank">Run-length encoding</a> (RLE) is a string compression method that works by replacing consecutive identical characters (repeated 2 or more times) with the concatenation of the character and the number marking the count of the characters (length of the run). For example, to compress the string <code>&quot;3322251&quot;</code> we replace <code>&quot;33&quot;</code> with <code>&quot;23&quot;</code>, replace <code>&quot;222&quot;</code> with <code>&quot;32&quot;</code>, replace <code>&quot;5&quot;</code> with <code>&quot;15&quot;</code> and replace <code>&quot;1&quot;</code> with <code>&quot;11&quot;</code>. Thus the compressed string becomes <code>&quot;23321511&quot;</code>.</p>

<p>Given a positive integer <code>n</code>, return <em>the </em><code>n<sup>th</sup></code><em> element of the <strong>count-and-say</strong> sequence</em>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">n = 4</span></p>

<p><strong>Output:</strong> <span class="example-io">&quot;1211&quot;</span></p>

<p><strong>Explanation:</strong></p>

<pre>
countAndSay(1) = &quot;1&quot;
countAndSay(2) = RLE of &quot;1&quot; = &quot;11&quot;
countAndSay(3) = RLE of &quot;11&quot; = &quot;21&quot;
countAndSay(4) = RLE of &quot;21&quot; = &quot;1211&quot;
</pre>
</div>

<p><strong class="example">Example 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">n = 1</span></p>

<p><strong>Output:</strong> <span class="example-io">&quot;1&quot;</span></p>

<p><strong>Explanation:</strong></p>

<p>This is the base case.</p>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= n &lt;= 30</code></li>
</ul>

<p>&nbsp;</p>
<strong>Follow up:</strong> Could you solve it iteratively?

<!-- description:end -->

## Solutions

<!-- solution:start -->

### Solution 1: Simulation

The task requires outputting the appearance sequence of the $n$-th item, where the $n$-th item is the description of the $n-1$-th item in the sequence. Therefore, we iterate $n-1$ times. In each iteration, we use fast and slow pointers, denoted as j and i respectively, to record the current character's position and the position of the next character that is not equal to the current character. We then update the sequence of the previous item to be $j-i$ occurrences of the current character.

Time Complexity:

1. The outer loop runs `n - 1` times, iterating to generate the "Count and Say" sequence up to the nth term.
2. The inner while loop iterates through each character in the current string s and counts the consecutive occurrences of the same character.
3. The inner while loop runs in $O(m)$ time, where m is the length of the current string s.

Overall, the time complexity is $O(n \times m)$, where n is the input parameter representing the term to generate, and m is the maximum length of the string in the sequence.

Space Complexity: $O(m)$.


#### Java

```java
class Solution {
    public String countAndSay(int n) {
        String s = "1";
        while (--n > 0) {
            StringBuilder t = new StringBuilder();
            for (int i = 0; i < s.length();) {
                int j = i;
                while (j < s.length() && s.charAt(j) == s.charAt(i)) {
                    ++j;
                }
                t.append((j - i) + "");
                t.append(s.charAt(i));
                i = j;
            }
            s = t.toString();
        }
        return s;
    }
}
```

#### C++

```cpp
class Solution {
public:
    string countAndSay(int n) {
        string s = "1";
        while (--n) {
            string t = "";
            for (int i = 0; i < s.size();) {
                int j = i;
                while (j < s.size() && s[j] == s[i]) ++j;
                t += to_string(j - i);
                t += s[i];
                i = j;
            }
            s = t;
        }
        return s;
    }
};
```


#### JavaScript

```js
const countAndSay = function (n) {
    let s = '1';

    for (let i = 2; i <= n; i++) {
        let count = 1,
            str = '',
            len = s.length;

        for (let j = 0; j < len; j++) {
            if (j < len - 1 && s[j] === s[j + 1]) {
                count++;
            } else {
                str += `${count}${s[j]}`;
                count = 1;
            }
        }
        s = str;
    }
    return s;
};
```

<!-- solution:end -->

<!-- problem:end -->