
# [8. String to Integer (atoi)](https://leetcode.com/problems/string-to-integer-atoi)


## Description


<p>Implement the <code>myAtoi(string s)</code> function, which converts a string to a 32-bit signed integer.</p>

<p>The algorithm for <code>myAtoi(string s)</code> is as follows:</p>

<ol>
	<li><strong>Whitespace</strong>: Ignore any leading whitespace (<code>&quot; &quot;</code>).</li>
	<li><strong>Signedness</strong>: Determine the sign by checking if the next character is <code>&#39;-&#39;</code> or <code>&#39;+&#39;</code>, assuming positivity is neither present.</li>
	<li><strong>Conversion</strong>: Read the integer by skipping leading zeros&nbsp;until a non-digit character is encountered or the end of the string is reached. If no digits were read, then the result is 0.</li>
	<li><strong>Rounding</strong>: If the integer is out of the 32-bit signed integer range <code>[-2<sup>31</sup>, 2<sup>31</sup> - 1]</code>, then round the integer to remain in the range. Specifically, integers less than <code>-2<sup>31</sup></code> should be rounded to <code>-2<sup>31</sup></code>, and integers greater than <code>2<sup>31</sup> - 1</code> should be rounded to <code>2<sup>31</sup> - 1</code>.</li>
</ol>

<p>Return the integer as the final result.</p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">s = &quot;42&quot;</span></p>

<p><strong>Output:</strong> <span class="example-io">42</span></p>

<p><strong>Explanation:</strong></p>

<pre>
The underlined characters are what is read in and the caret is the current reader position.
Step 1: &quot;42&quot; (no characters read because there is no leading whitespace)
         ^
Step 2: &quot;42&quot; (no characters read because there is neither a &#39;-&#39; nor &#39;+&#39;)
         ^
Step 3: &quot;<u>42</u>&quot; (&quot;42&quot; is read in)
           ^
</pre>
</div>

<p><strong class="example">Example 2:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">s = &quot; -042&quot;</span></p>

<p><strong>Output:</strong> <span class="example-io">-42</span></p>

<p><strong>Explanation:</strong></p>

<pre>
Step 1: &quot;<u>   </u>-042&quot; (leading whitespace is read and ignored)
            ^
Step 2: &quot;   <u>-</u>042&quot; (&#39;-&#39; is read, so the result should be negative)
             ^
Step 3: &quot;   -<u>042</u>&quot; (&quot;042&quot; is read in, leading zeros ignored in the result)
               ^
</pre>
</div>

<p><strong class="example">Example 3:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">s = &quot;1337c0d3&quot;</span></p>

<p><strong>Output:</strong> <span class="example-io">1337</span></p>

<p><strong>Explanation:</strong></p>

<pre>
Step 1: &quot;1337c0d3&quot; (no characters read because there is no leading whitespace)
         ^
Step 2: &quot;1337c0d3&quot; (no characters read because there is neither a &#39;-&#39; nor &#39;+&#39;)
         ^
Step 3: &quot;<u>1337</u>c0d3&quot; (&quot;1337&quot; is read in; reading stops because the next character is a non-digit)
             ^
</pre>
</div>

<p><strong class="example">Example 4:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">s = &quot;0-1&quot;</span></p>

<p><strong>Output:</strong> <span class="example-io">0</span></p>

<p><strong>Explanation:</strong></p>

<pre>
Step 1: &quot;0-1&quot; (no characters read because there is no leading whitespace)
         ^
Step 2: &quot;0-1&quot; (no characters read because there is neither a &#39;-&#39; nor &#39;+&#39;)
         ^
Step 3: &quot;<u>0</u>-1&quot; (&quot;0&quot; is read in; reading stops because the next character is a non-digit)
          ^
</pre>
</div>

<p><strong class="example">Example 5:</strong></p>

<div class="example-block">
<p><strong>Input:</strong> <span class="example-io">s = &quot;words and 987&quot;</span></p>

<p><strong>Output:</strong> <span class="example-io">0</span></p>

<p><strong>Explanation:</strong></p>

<p>Reading stops at the first non-digit character &#39;w&#39;.</p>
</div>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>0 &lt;= s.length &lt;= 200</code></li>
	<li><code>s</code> consists of English letters (lower-case and upper-case), digits (<code>0-9</code>), <code>&#39; &#39;</code>, <code>&#39;+&#39;</code>, <code>&#39;-&#39;</code>, and <code>&#39;.&#39;</code>.</li>
</ul>


## Solutions


### Solution 1: Traverse the String

First, we determine whether the string is empty. If it is, we directly return $0$.

Otherwise, we need to traverse the string, skip the leading spaces, and determine whether the first non-space character is a positive or negative sign.

Then we traverse the following characters. If it is a digit, we judge whether adding this digit will cause integer overflow. If it does, we return the result according to the positive or negative sign. Otherwise, we add the digit to the result. We continue to traverse the following characters until we encounter a non-digit character or the traversal ends.

After the traversal ends, we return the result according to the positive or negative sign.

The time complexity is $O(n)$, where $n$ is the length of the string. We only need to process all characters in turn. The space complexity is $O(1)$.


#### Java

```java
class Solution {
    public int myAtoi(String s) {
        if (s == null) return 0;
        int n = s.length();
        if (n == 0) return 0;
        int i = 0;
        while (s.charAt(i) == ' ') {
            
            if (++i == n) return 0;
        }
        int sign = 1;
        if (s.charAt(i) == '-') sign = -1;
        if (s.charAt(i) == '-' || s.charAt(i) == '+') ++i;
        int res = 0, flag = Integer.MAX_VALUE / 10;
        for (; i < n; ++i) {
           
            if (s.charAt(i) < '0' || s.charAt(i) > '9') break;
       
            if (res > flag || (res == flag && s.charAt(i) > '7'))
                return sign > 0 ? Integer.MAX_VALUE : Integer.MIN_VALUE;
            res = res * 10 + (s.charAt(i) - '0');
        }
        return sign * res;
    }
}
```


#### JavaScript

```js
const myAtoi = function (str) {
    str = str.trim();
    if (!str) return 0;
    let isPositive = 1;
    let i = 0,
        ans = 0;
    if (str[i] === '+') {
        isPositive = 1;
        i++;
    } else if (str[i] === '-') {
        isPositive = 0;
        i++;
    }
    for (; i < str.length; i++) {
        let t = str.charCodeAt(i) - 48;
        if (t > 9 || t < 0) break;
        if (ans > 2147483647 / 10 || ans > (2147483647 - t) / 10) {
            return isPositive ? 2147483647 : -2147483648;
        } else {
            ans = ans * 10 + t;
        }
    }
    return isPositive ? ans : -ans;
};
```

