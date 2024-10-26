# [32. Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses)


## Description


<p>Given a string containing just the characters <code>&#39;(&#39;</code> and <code>&#39;)&#39;</code>, return <em>the length of the longest valid (well-formed) parentheses </em><span data-keyword="substring-nonempty"><em>substring</em></span>.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;(()&quot;
<strong>Output:</strong> 2
<strong>Explanation:</strong> The longest valid parentheses substring is &quot;()&quot;.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;)()())&quot;
<strong>Output:</strong> 4
<strong>Explanation:</strong> The longest valid parentheses substring is &quot;()()&quot;.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> s = &quot;&quot;
<strong>Output:</strong> 0
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>0 &lt;= s.length &lt;= 3 * 10<sup>4</sup></code></li>
	<li><code>s[i]</code> is <code>&#39;(&#39;</code>, or <code>&#39;)&#39;</code>.</li>
</ul>


## Solutions


### Solution 1: Dynamic Programming

We define $f[i]$ to be the length of the longest valid parentheses that ends with $s[i-1]$, and the answer is $max(f[i])$.

When $i \lt 2$, the length of the string is less than $2$, and there is no valid parentheses, so $f[i] = 0$.

When $i \ge 2$, we consider the length of the longest valid parentheses that ends with $s[i-1]$, that is, $f[i]$:

-   If $s[i-1]$ is a left parenthesis, then the length of the longest valid parentheses that ends with $s[i-1]$ must be $0$, so $f[i] = 0$.
-   If $s[i-1]$ is a right parenthesis, there are the following two cases:
    -   If $s[i-2]$ is a left parenthesis, then the length of the longest valid parentheses that ends with $s[i-1]$ is $f[i-2] + 2$.
    -   If $s[i-2]$ is a right parenthesis, then the length of the longest valid parentheses that ends with $s[i-1]$ is $f[i-1] + 2$, but we also need to consider whether $s[i-f[i-1]-2]$ is a left parenthesis. If it is a left parenthesis, then the length of the longest valid parentheses that ends with $s[i-1]$ is $f[i-1] + 2 + f[i-f[i-1]-2]$.

Therefore, we can get the state transition equation:

$$
\begin{cases}
f[i] = 0, & \textit{if } s[i-1] = '(',\\
f[i] = f[i-2] + 2, & \textit{if } s[i-1] = ')' \textit{ and } s[i-2] = '(',\\
f[i] = f[i-1] + 2 + f[i-f[i-1]-2], & \textit{if } s[i-1] = ')' \textit{ and } s[i-2] = ')' \textit{ and } s[i-f[i-1]-2] = '(',\\
\end{cases}
$$

Finally, we only need to return $max(f)$.

The time complexity is $O(n)$, and the space complexity is $O(n)$, where $n$ is the length of the string.


#### Java

```java
class Solution {
    public int longestValidParentheses(String s) {
        int n = s.length();
        int[] f = new int[n + 1];
        int ans = 0;
        for (int i = 2; i <= n; ++i) {
            if (s.charAt(i - 1) == ')') {
                if (s.charAt(i - 2) == '(') {
                    f[i] = f[i - 2] + 2;
                } else {
                    int j = i - f[i - 1] - 1;
                    if (j > 0 && s.charAt(j - 1) == '(') {
                        f[i] = f[i - 1] + 2 + f[j - 1];
                    }
                }
                ans = Math.max(ans, f[i]);
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
    int longestValidParentheses(string s) {
        int n = s.size();
        int f[n + 1];
        memset(f, 0, sizeof(f));
        for (int i = 2; i <= n; ++i) {
            if (s[i - 1] == ')') {
                if (s[i - 2] == '(') {
                    f[i] = f[i - 2] + 2;
                } else {
                    int j = i - f[i - 1] - 1;
                    if (j && s[j - 1] == '(') {
                        f[i] = f[i - 1] + 2 + f[j - 1];
                    }
                }
            }
        }
        return *max_element(f, f + n + 1);
    }
};
```


#### JavaScript

```js
/**
 * @param {string} s
 * @return {number}
 */
var longestValidParentheses = function (s) {
    const n = s.length;
    const f = new Array(n + 1).fill(0);
    for (let i = 2; i <= n; ++i) {
        if (s[i - 1] === ')') {
            if (s[i - 2] === '(') {
                f[i] = f[i - 2] + 2;
            } else {
                const j = i - f[i - 1] - 1;
                if (j && s[j - 1] === '(') {
                    f[i] = f[i - 1] + 2 + f[j - 1];
                }
            }
        }
    }
    return Math.max(...f);
};
```


### Solution 2: Using Stack

-   Maintain a stack to store the indices of left parentheses. Initialize the bottom element of the stack with the value -1 to facilitate the calculation of the length of valid parentheses.
-   Iterate through each element of the string:
    -   If the character is a left parenthesis, push the index of the character onto the stack.
    -   If the character is a right parenthesis, pop an element from the stack to represent that we have found a valid pair of parentheses.
        -   If the stack is empty, it means we couldn't find a left parenthesis to match the right parenthesis. In this case, push the index of the character as a new starting point.
        -   If the stack is not empty, calculate the length of the valid parentheses and update it.

Summary:

The key to this algorithm is to maintain a stack to store the indices of left parentheses and then update the length of the valid substring of parentheses by pushing and popping elements.

The time complexity is $O(n)$, and the space complexity is $O(n)$, where $n$ is the length of the string.


#### JavaScript

```js
/**
 * @param {string} s
 * @return {number}
 */
var longestValidParentheses = function (s) {
    let ans = 0;
    const stack = [-1];
    for (i = 0; i < s.length; i++) {
        if (s.charAt(i) === '(') {
            stack.push(i);
        } else {
            stack.pop();
            if (stack.length === 0) {
                stack.push(i);
            } else {
                ans = Math.max(ans, i - stack[stack.length - 1]);
            }
        }
    }
    return ans;
};
```
